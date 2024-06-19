# React Native 拆包实践6 - Android 启动流程

> 完成了iOS的拆包之后，接下来看看Android如何按需加载jsbundle，在此之前同样需要先了解react native应用在android上的启动流程。先从概念入手，再配合源码介绍，一步步找到它加载bundle的方式。

### 概念

#### 1. ReactContext

ReactContext继承于ContextWrapper，是ReactNative应用的上下文，在官方文档中是这样描述它的：

> Abstract ContextWrapper for Android application or activity Context and CatalystInstance

#### 2. CatalystInstance

CatalystInstance是ReactNative中Java层、C++层、JS层通信的管理类，负责Java层、JS层核心Module映射表与回调，是三端通信的入口与桥梁。（ps: catalyst的翻译是一个化学概念：触媒，催化剂。CatalystInstance的作用是协调）

#### 3. ReactInstanceManager

ReactInstanceManager是ReactNative实例的管理类，创建ReactContext、CatalystInstance等类，解析ReactPackage生成映射表，并且配合ReactRootView管理View的创建与生命周期等功能。

#### 4. ReactRootView

应用的UI容器，它本质上是一个FrameLayout，监听size的改变，UI manager可以根据size的更改来重新布局子元素。同时它将所有的touch event通过JSTouchDispatcher发送给JS层。在主Activity的onCreate中最终调用了`getPlainActivity().setContentView(mReactRootView);`将该view设置为contentView。

```dart
/**
 * Default root view for catalyst apps. Provides the ability to listen for size changes so that a UI
 * manager can re-layout its elements. It delegates handling touch events for itself and child views
 * and sending those events to JS by using JSTouchDispatcher. This view is overriding {@link
 * ViewGroup#onInterceptTouchEvent} method in order to be notified about the events for all of its
 * children and it's also overriding {@link ViewGroup#requestDisallowInterceptTouchEvent} to make
 * sure that {@link ViewGroup#onInterceptTouchEvent} will get events even when some child view start
 * intercepting it. In case when no child view is interested in handling some particular touch event,
 * this view's {@link View#onTouchEvent} will still return true in order to be notified about all
 * subsequent touch events related to that gesture (in case when JS code wants to handle that
 * gesture).
 */
public class ReactRootView extends FrameLayout implements RootView, ReactRoot {...}
```

### 启动

新创建的ReactNative项目中，`MainActivity`是整个App的启动入口，这个Activity继承自`ReactActivity`，并将自己所有的生命周期都代理给ReactActivtiyDelegate类来实现。也就是说onCreate等方法都是将调用ReactActivtiyDelegate类的onCreate等方法。代码如下所示：

```java
public class ReactActivityDelegate {
  private final @Nullable Activity mActivity;
  private final @Nullable String mMainComponentName;
  private @Nullable ReactRootView mReactRootView;
  
  public ReactActivityDelegate(ReactActivity activity, @Nullable String mainComponentName) {
    mActivity = activity;
    mMainComponentName = mainComponentName;
  }

  protected void onCreate(Bundle savedInstanceState) {
    String mainComponentName = getMainComponentName();
    if (mainComponentName != null) {
      loadApp(mainComponentName);
    }
    ...
  }

  protected void loadApp(String appKey) {
    if (mReactRootView != null) {
      throw new IllegalStateException("Cannot loadApp while app is already running.");
    }
    mReactRootView = createRootView();
    mReactRootView.startReactApplication(
      getReactNativeHost().getReactInstanceManager(),
      appKey,
      getLaunchOptions());
    // getPlainActivity()就是当前的MainActivity
    getPlainActivity().setContentView(mReactRootView);
  }
}
```

可以看到，在onCreate中，实际的初始化操作发生在`loadApp`方法中。在此方法中，创建了`ReactRootView`的实例`mReactRootView`，在初始化结束时将它设置为MainActivity的contentView。在此之前，还调用了`startReactApplication`方法，在该方法中完成了几乎所有的初始化工作。

```tsx
public void startReactApplication(ReactInstanceManager reactInstanceManager, String moduleName, @Nullable Bundle initialProperties) {
  startReactApplication(reactInstanceManager, moduleName, initialProperties, null);
}
```

该方法，需要三个入参：`ReactInstanceManager`，`moduleName`(也就是js中注册的component name) 和`initialProperties`(启动时，可以通过这个参数传递一些参数到js)。后两个参数没有什么特别，第一个参数正是我们开始时介绍的重要成员之一，它来自于`getReactNativeHost().getReactInstanceManager()`

```cpp
  protected ReactNativeHost getReactNativeHost() {
    return ((ReactApplication) getPlainActivity().getApplication()).getReactNativeHost();
  }
```

它是从Application中获得的，App中的Application实现了ReactApplication接口：

```csharp
public interface ReactApplication {
  ReactNativeHost getReactNativeHost();
}
```

该接口，只需要提供一个`ReactNativeHost`。这个类的实现比较简单，当需`ReactInstanceManager`时，它会创建一个`ReactInstanceManager`实例

```java
public abstract class ReactNativeHost {

  private final Application mApplication;
  private @Nullable ReactInstanceManager mReactInstanceManager;

  protected ReactNativeHost(Application application) {
    mApplication = application;
  }

  public ReactInstanceManager getReactInstanceManager() {
    if (mReactInstanceManager == null) {
      mReactInstanceManager = createReactInstanceManager();
    }
    return mReactInstanceManager;
  }
  
  protected ReactInstanceManager createReactInstanceManager() {
    
    ReactInstanceManagerBuilder builder = ReactInstanceManager.builder()
      .setApplication(mApplication)
      .setJSMainModulePath(getJSMainModuleName())
      .setUseDeveloperSupport(getUseDeveloperSupport())
      .setRedBoxHandler(getRedBoxHandler())
      .setJavaScriptExecutorFactory(getJavaScriptExecutorFactory())
      .setUIImplementationProvider(getUIImplementationProvider())
      .setJSIModulesPackage(getJSIModulePackage())
      .setInitialLifecycleState(LifecycleState.BEFORE_CREATE);
    // 添加ReactPackage
    for (ReactPackage reactPackage : getPackages()) {
      builder.addPackage(reactPackage);
    }
    // 获取js Bundle的加载路径
    String jsBundleFile = getJSBundleFile();
    if (jsBundleFile != null) {
      builder.setJSBundleFile(jsBundleFile);
    } else {
      builder.setBundleAssetName(Assertions.assertNotNull(getBundleAssetName()));
    }
    ReactInstanceManager reactInstanceManager = builder.build();
    return reactInstanceManager;
  }
}
```

创建好`ReactInstanceManager`，我们回到ReactActivityDelegate的`loadApp`，下要分析的便是`startReactApplication`方法：

```dart
  /**
   * Schedule rendering of the react component rendered by the JS application from the given JS
   * module (@{param moduleName}) using provided {@param reactInstanceManager} to attach to the
   * JS context of that manager. Extra parameter {@param launchOptions} can be used to pass initial
   * properties for the react component.
   */
  public void startReactApplication(
      ReactInstanceManager reactInstanceManager,
      String moduleName,
      @Nullable Bundle initialProperties,
      @Nullable String initialUITemplate) {

    try {
      // 省略线程检查的代码
      ...
      mReactInstanceManager = reactInstanceManager;
      mJSModuleName = moduleName;
      mAppProperties = initialProperties;
      mInitialUITemplate = initialUITemplate;
      ...
      // 创建RN应用上下文
      if (!mReactInstanceManager.hasStartedCreatingInitialContext()) {
        mReactInstanceManager.createReactContextInBackground();
      }
      // 把自己也就是ReactRootView添加到mReactInstanceManager的mAttachedReactRoots（Set<ReactRoot>）中去
      attachToReactInstanceManager();
    } finally {
      ...
    }
  }
```

其中最核心的就是`mReactInstanceManager.createReactContextInBackground()`

```java
  @ThreadConfined(UI)
  public void createReactContextInBackground() {
    ...
    mHasStartedCreatingInitialContext = true;
    recreateReactContextInBackgroundInner();
  }
```

由于我们这里只考虑从文件系统中加载jsbundle所以跳过debug的代码分析。

```java
  @ThreadConfined(UI)
  private void recreateReactContextInBackgroundInner() {
    ...
    // 在debug模式下，使用了metrojs，jsbundle将从service中获取
    if (mUseDeveloperSupport && mJSMainModulePath != null) {...}

    // 非debug模式：
    recreateReactContextInBackgroundFromBundleLoader();
  }

  @ThreadConfined(UI)
  private void recreateReactContextInBackgroundFromBundleLoader() {
    ...
    recreateReactContextInBackground(mJavaScriptExecutorFactory, mBundleLoader);
  }

  @ThreadConfined(UI)
  private void recreateReactContextInBackground(
    JavaScriptExecutorFactory jsExecutorFactory,
    JSBundleLoader jsBundleLoader) {

    final ReactContextInitParams initParams = new ReactContextInitParams(
      jsExecutorFactory,
      jsBundleLoader);
    if (mCreateReactContextThread == null) {
      runCreateReactContextOnNewThread(initParams);
    } else {
      mPendingReactContextInitParams = initParams;
    }
  }

  @ThreadConfined(UI)
  private void runCreateReactContextOnNewThread(final ReactContextInitParams initParams) {
    ...
    // 如果mCurrentReactContext非null，则先tearDownReactContext然后将其设置为null

    mCreateReactContextThread = new Thread(null,
            new Runnable() {
              @Override
              public void run() {
                ...
                // 如果当前的reactContext正在销毁当中，则等它销毁了再执行下面的创建操作

                try {
                  ...
                  final ReactApplicationContext reactApplicationContext =
                      createReactContext(
                          initParams.getJsExecutorFactory().create(),
                          initParams.getJsBundleLoader());
                  mCreateReactContextThread = null;
                  
                  final Runnable maybeRecreateReactContextRunnable =
                      new Runnable() {
                        @Override
                        public void run() {
                          if (mPendingReactContextInitParams != null) {
                            runCreateReactContextOnNewThread(mPendingReactContextInitParams);
                            mPendingReactContextInitParams = null;
                          }
                        }
                      };
                  Runnable setupReactContextRunnable =
                      new Runnable() {
                        @Override
                        public void run() {
                          try {
                            setupReactContext(reactApplicationContext);
                          } catch (Exception e) {
                            mDevSupportManager.handleException(e);
                          }
                        }
                      };

                  reactApplicationContext.runOnNativeModulesQueueThread(setupReactContextRunnable);
                  UiThreadUtil.runOnUiThread(maybeRecreateReactContextRunnable);
                } catch (Exception e) {
                  mDevSupportManager.handleException(e);
                }
              }
            },
            "create_react_context");
    mCreateReactContextThread.start();
  }
```

方法中创建了mCreateReactContextThread线程，并执行了该线程，在该线程中，首先创建了reactApplicationContext，忽略一些非核心代码，并通过官方对ReactContext的描述（它是Android Context以及CatalystInstance的Wrapper），此处创建了ReactContext和CatalystInstance实例。并通过调用runJSBundle方法加载了jsbundle文件。加载jsbundle实际是通过JSBundleLoader，而它有多种，可参考该类中的对应的静态方法`createXXXXXXXLoader`。

```java
  private ReactApplicationContext createReactContext(
      JavaScriptExecutor jsExecutor,
      JSBundleLoader jsBundleLoader) {
    
    final ReactApplicationContext reactContext = new ReactApplicationContext(mApplicationContext);
    ...
    NativeModuleRegistry nativeModuleRegistry = processPackages(reactContext, mPackages, false);

    CatalystInstanceImpl.Builder catalystInstanceBuilder = new CatalystInstanceImpl.Builder()
      .setReactQueueConfigurationSpec(ReactQueueConfigurationSpec.createDefault())
      .setJSExecutor(jsExecutor)
      .setRegistry(nativeModuleRegistry)
      .setJSBundleLoader(jsBundleLoader)
      .setNativeModuleCallExceptionHandler(exceptionHandler);
    final CatalystInstance catalystInstance = catalystInstanceBuilder.build();

    if (mJSIModulePackage != null) {
      catalystInstance.addJSIModules(mJSIModulePackage
        .getJSIModules(reactContext, catalystInstance.getJavaScriptContextHolder()));
    }

    if (mBridgeIdleDebugListener != null) {
      catalystInstance.addBridgeIdleDebugListener(mBridgeIdleDebugListener);
    }
    // 加载jsbundle
    catalystInstance.runJSBundle();
    // 创建的catalystInstance实例赋值给reactContext的内部mCatalystInstance
    reactContext.initializeWithInstance(catalystInstance);
    return reactContext;
  }
```

此后又创建了两个线程maybeRecreateReactContextRunnable和setupReactContextRunnable。其中maybeRecreateReactContextRunnable同过代码猜测它只是一个补丁，用于修正多线程下可能出现的context创建失败的情况，此处可以先忽略它。在创建好reactContext以及catalystInstance并完成jsbundle的加载后将调用 setupReactContextRunnable，它调用了另一个方法setupReactContext并把创建好的reactApplicationContext传入其中。

```java
  private void setupReactContext(final ReactApplicationContext reactContext) {

    synchronized (mAttachedReactRoots) {
      synchronized (mReactContextLock) {
        mCurrentReactContext = Assertions.assertNotNull(reactContext);
      }

      CatalystInstance catalystInstance =
          Assertions.assertNotNull(reactContext.getCatalystInstance());
      catalystInstance.initialize();
      moveReactContextToCurrentLifecycleState();

      // mAttachedRootViews保存的是ReactRootView
      for (ReactRoot reactRoot : mAttachedReactRoots) {
        attachRootViewToInstance(reactRoot);
      }
    }

    ReactInstanceEventListener[] listeners =
      new ReactInstanceEventListener[mReactInstanceEventListeners.size()];
    final ReactInstanceEventListener[] finalListeners =
        mReactInstanceEventListeners.toArray(listeners);

    UiThreadUtil.runOnUiThread(
        new Runnable() {
          @Override
          public void run() {
            for (ReactInstanceEventListener listener : finalListeners) {
              listener.onReactContextInitialized(reactContext);
            }
          }
        });

    reactContext.runOnJSQueueThread(
        new Runnable() {
          @Override
          public void run() {
            Process.setThreadPriority(Process.THREAD_PRIORITY_DEFAULT);
          }
        });
    reactContext.runOnNativeModulesQueueThread(
        new Runnable() {
          @Override
          public void run() {
            Process.setThreadPriority(Process.THREAD_PRIORITY_DEFAULT);
          }
        });
  }
```

页面的加载发发生在attachRootViewToInstance中

```java
  private void attachRootViewToInstance(final ReactRoot reactRoot) {
    UIManager uiManagerModule = UIManagerHelper.getUIManager(mCurrentReactContext, reactRoot.getUIManagerType());
    // 设置rootTag以及从native传入的参数
    @Nullable Bundle initialProperties = reactRoot.getAppProperties();
    final int rootTag = uiManagerModule.addRootView(
      reactRoot.getRootViewGroup(),
      initialProperties == null ?
            new WritableNativeMap() : Arguments.fromBundle(initialProperties),
        reactRoot.getInitialUITemplate());
    reactRoot.setRootViewTag(rootTag);

    // 启动流程入口：由Java层调用启动，它将触发js的runApplication
    reactRoot.runApplication();
    
    UiThreadUtil.runOnUiThread(
        new Runnable() {
          @Override
          public void run() {
            reactRoot.onStage(ReactStage.ON_ATTACH_TO_INSTANCE);
          }
        });
  }
```

至此也就完成了启动流程的分析，下一节中我们将开始通过自定义的实现来进行按需加载不同的jsbundle。

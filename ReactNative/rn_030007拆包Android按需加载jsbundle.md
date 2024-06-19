# React Native 拆包 - Android 按需加载jsbundle

上一节中，我们梳理了一下Android中React Native的启动流程，其中有大篇幅的代码分析，看起来容易失去主线。在开始实现按需加载的逻辑前，来换一种角度看看启动流程。

下图中列出了RN的核心类及其从属关系，他们的作用请参考上篇文章

![](rnhx.png)

React-Native核心类示意图

由于Application持有着ReactHost，可以认为这些核心类在一个App中都是以单例的形式存在，除了ReactRootView。如果使用默认的方式开发RN那么ReactRootView也同样是一个单例。

整个RN的启动源于起始的一个ReactRootView的startReactApplication方法，当进入该方法后，启动流程便交给了ReactInstanceManager:

```rust
-> createReactContextInBackground()
  -> recreateReactContextInBackgroundInner()
    -> recreateReactContextInBackgroundFromBundleLoader()
      -> recreateReactContextInBackground(
    JavaScriptExecutorFactory jsExecutorFactory,
    JSBundleLoader jsBundleLoader) // mJavaScriptExecutorFactory和mBundleLoader都是在ReactInstanceManager初始化的时候别赋值的，ReactInstanceManager的初始化时再ReactHost中进行的，其中使用了Builder来构建ReactInstanceManager
        -> runCreateReactContextOnNewThread
          -> createReactContext //在其中初始化了自己的成员变量ReactContext（实际的类型ReactApplicationContext）在初始化的过程中也初始化了CatalystInstance，并把CatalystInstance赋值给ReactContext
            -> catalystInstance.runJSBundle();  //runJSBundle实际的工作就是调用JSBundleLoader加载js文件，（CatalystInstance的实现类是CatalystInstanceImpl）
          -> setupReactContext 
            -> attachRootViewToInstance
              -> reactRoot.runApplication(); // 调用了js的AppRegistry的runApplication方法
            -> listener.onReactContextInitialized(reactContext); // 通过执行callback通知RectContext已经ready
```

#### 需求

需求和iOS的部分相同，在启动时加载在公共jsbundle，加载完成后显示首页，首页部分也是由ReactNative实现，点击首页中的一个button，再加载并显示另一个ReactNative实现的子App。

#### 预加载jsbundle的公共部分

预加载意在初始化ReactNative的核心类同时把公共的js代码加载进js引擎，通过梳理启动流程，我们知道可通过直接调用`ReactInstanceManager`的`createReactContextInBackground`方法，同时在调用前启动对`onReactContextInitialized`调用的监听，这部分的代码可以交由启动屏的Activity完成：

```java
// SplashActivity是App的入口页面
public class SplashActivity extends AppCompatActivity implements ReactInstanceManager.ReactInstanceEventListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_splash);

        ReactInstanceManager reactInstanceManager = ((ReactApplication)getApplication()).getReactNativeHost().getReactInstanceManager();
        // 监听加载结束的回调
        reactInstanceManager.addReactInstanceEventListener(this);

        if (!reactInstanceManager.hasStartedCreatingInitialContext()) {
            reactInstanceManager.createReactContextInBackground();
        }
    }

    @Override
    public void onReactContextInitialized(ReactContext context) {
        // 当加载完成后启动首页
        startActivity(new Intent(SplashActivity.this, MainAppActivity.class));
        // ... 此处可以关闭Splash Screen
    }
}
```

由于我们改变了加载JS的流程，所以实现了自己的`ReactActivity`和`ReactActivityDelegate`。其他页面，例如`MainAppActivity`都是由ReactNative的实现，并集成自我们自定义的`ReactActivity`，即`AsyncReactActivity`。

#### 异步加载js的AsyncReactActivity

在实现`AsyncReactActivity`，我们先来完成`AsyncReactActivityDelegate`，这个delegate的只是为了将原本protect的方法public出来供我们的`AsyncReactActivity`使用。

```java
public class AsyncReactActivityDelegate extends ReactActivityDelegate {

    public AsyncReactActivityDelegate(Activity activity, @Nullable String mainComponentName) {
        super(activity, mainComponentName);
    }

    public AsyncReactActivityDelegate(ReactActivity activity, @Nullable String mainComponentName) {
        super(activity, mainComponentName);
    }

    @Override
    public ReactNativeHost getReactNativeHost() {
        return super.getReactNativeHost();
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Override
    public void loadApp(String appKey) {
        super.loadApp(appKey);
    }

    @Override
    public void onPause() {
        super.onPause();
    }

    @Override
    public void onResume() {
        super.onResume();
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
    }
}
```

我们再来看一下`AsyncReactActivity`的实现，使用了抽象类。demo中的MainAppActivity以及SubAppActivity都继承了该类，在这两个子类中只需要重载`getMainComponentName`和`getJSBundleName`方法，用于提供rn的入口module name以及jsbunle name。

在`onCreate`方法中先加载对应的业务jsbundle，在加载完成后再执行`mDelegate`的`onCreate`方法。加载实际使用的是`catalystInstance`的`loadScriptFromAssets`，由于核心类已经初始化完成，所以可以直接获取。`mDelegate`的`onCreate`则用于创建一个ReactRootView，并attach到ReactInstanceManager中。

> 那么在加载基础jsbundle时为什么没有ReactRootView的创建呢？同过源码可以看到setupReactContext中遍历mAttachedReactRoots，并依次调用attach方法，但在加载基础jsbundle时直接使用了`ReactInstanceManager`的`createReactContextInBackground`方法，而非ReactRootView的startReactApplication（在startReactApplication中会将当前view添加到mAttachedReactRoots）

```java
public abstract class AsyncReactActivity extends AppCompatActivity
        implements DefaultHardwareBackBtnHandler, PermissionAwareActivity {

    private static Set<String> loadedJSBundles = new HashSet<>();
    private final AsyncReactActivityDelegate mDelegate;

    protected AsyncReactActivity() {
        mDelegate = createReactActivityDelegate();
    }

    protected @Nullable String getMainComponentName() {
        return null;
    }

    protected @Nullable String getJSBundleName() {
        return null;
    }

    protected AsyncReactActivityDelegate createReactActivityDelegate() {
        return new AsyncReactActivityDelegate(this, getMainComponentName()) {
            @Override
            protected ReactRootView createRootView() {
                return new RNGestureHandlerEnabledRootView(AsyncReactActivity.this);
            }
        };
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 1. load jsbundle file
        loadJSBundleFromAssets(() -> {
            // 2. call mDelegate.onCreate
            mDelegate.onCreate(savedInstanceState);
        });
    }

    private void loadJSBundleFromAssets(LoadJSBundleListener listener) {
        CatalystInstance catalystInstance = getReactContext().getCatalystInstance();
        String source = getJSBundleName();
        if (catalystInstance == null || source == null) {
            return;
        }
        if (loadedJSBundles.contains(source)) {
            listener.onJSBundleLoaded();
            return;
        }
        if(!source.startsWith("assets://")) {
            source = "assets://" + source;
        }
        catalystInstance.loadScriptFromAssets(this.getAssets(), source, false);
        loadedJSBundles.add(source);
        listener.onJSBundleLoaded();
    }
    // ...
}
```

至此，我们以及完成了jsbundle的异步加载。至于Main App跳转至Sub App的实现需要依赖于native module了。这部分的代码就比较简单了，这里给出一个示例：

```java
public class RNNavigation extends ReactContextBaseJavaModule {
    
    public RNNavigation(@Nonnull ReactApplicationContext reactContext) {
        super(reactContext);
    }

    @Nonnull
    @Override
    public String getName() {
        return "RNNavigation";
    }

    @ReactMethod
    public void navigateTo(String name) {
        Activity activity = getCurrentActivity();
        if (activity == null) {
            return;
        }
        MainApplication application = (MainApplication) activity.getApplication();
        if (application == null) {
            return;
        }
        Class<?> klass = application.existingModules().get(name);
        activity.startActivity(new Intent(activity, klass));
    }

    @ReactMethod
    public void goBack() {
        Activity activity = getCurrentActivity();
        if (activity == null) {
            return;
        }
        activity.finish();
    }
}
```

通过在Application中保存一个Map来记录SubApp的class。
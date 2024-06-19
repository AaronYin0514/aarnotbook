# React Native 拆包 - 按需加载js module

> 首先，明确一下需求。App启动时加载一个RN的主应用，当点击主应用中的一个Button后，打开一个RN子应用，这个子应用可以理解为微信小程序。

#### 拆分jsbundle

根据上一节的内容，使用自定义的`createModuleIdFactory`将所有js module赋予了一个固定Id，在此基础上便可以开始拆包了，即将原先单独的jsbundle文件拆分为多个jsbundle文件。根据需求，我们的目标是将jsbundle分为`base.jsbundle`，`main_app.jsbundle`，`sub_app.jsbundle`。顾名思义，base中包含了react native自有的js，三方库中的js以及各个app中一些公用的js，main_app和sub_app则只包含各自的业务代码。

这个过程中所需的命令行如下所示：

```java
react-native bundle --platform ios --dev false --entry-file base.js --bundle-output build/ios/common.jsbundle --assets-dest build/ios/

react-native bundle --platform ios --dev false --entry-file index.js --bundle-output build/ios/main_app_all.jsbundle --assets-dest build/ios/

react-native bundle --platform ios --dev false --entry-file rewards/index.js --bundle-output build/ios/sub_app_all.jsbundle --assets-dest build/ios/

node diff.js ./build/ios/common.jsbundle ./build/ios/main_app_all.jsbundle ./build/ios/main_app.jsbundle
node diff.js ./build/ios/common.jsbundle ./build/ios/sub_app_all.jsbundle ./build/ios/sub_app.jsbundle
```

前三个命令用于打包jsbundle，不同点在于指定了不同的入口文件以及输出文件的路径。  
其中`base.js`中定义了所有的公共模块（react native自有的js，三方库中的js以及各个app中一些公用的js）

```jsx
// react-native js & 3rd part lib js
require("react-native");
require("react");
require("react-navigation");
require("react-native-gesture-handler");
require("react-native-reanimated");
require("react-navigation-hooks");

// reused js module
require("./src/components/Header")
```

`index.js`和`rewards/index.js`则为两个App的入口定义，两个打包命令打出的jsbundle都是可以直接在production环境下使用的。

其后的两条命令则是将main_app_all.jsbundle和sub_app_all.jsbundle中，与common.jsbundle相同的部分删除，从而得到只包含各自页面代码的main_app.jsbundle和sub_app.jsbundle。diff的代码比较简单，这里就不过多解释了：

```jsx
var fs = require('fs');
var readline = require('readline');

function readFileToArr(fReadName, callback) {
  var fRead = fs.createReadStream(fReadName);
  var objReadline = readline.createInterface({
      input:fRead
  });
  var arr = new Array();
  objReadline.on('line',function (line) {
      arr.push(line);
  });
  objReadline.on('close',function () {
      callback(arr);
  });
}

var argvs = process.argv.splice(2);
var commonFile = argvs[0]; // common.jsbundle
var businessFile = argvs[1]; // business.jsbundle
var diffOut = argvs[2]; // diff.jsbundle
readFileToArr(commonFile, function (c_data) {
  var diff = [];
  var commonArrs = c_data;
  readFileToArr(businessFile, function (b_data) {
    var businessArrs = b_data;
    for (let i = 0; i < businessArrs.length; i++) {
      if (commonArrs.indexOf(businessArrs[i]) === -1) {
        diff.push(businessArrs[i]);
      }
    }
    var newContent = diff.join('\n');
    fs.writeFileSync(diffOut, newContent);
  });
});
```

经过这一系列操作，得到产物为：

1.  common.jsbundle
2.  main_app.jsbundle
3.  sub_app.jsbundle
4.  assets -- _include all the images_

将这些产物添加进项目，即可开始native部分的编码。通过需求，我们知道需要按需加载这三个jsbundle，而common.jsbundle是需要在app启动的初期就完成加载的，在其成功后可以根据需要加载main_app或sub_app。在我们的例子中，将优先显示main_app中的内容。

在我们没有进行任何修改时，AppDelegate中加载jsbundle的代码如下所示：

```objectivec
...
RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                 moduleName:@"split_jsbundle"
                                          initialProperties:nil];
...
// 其后将rootView赋值给rootViewController的view

...
- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge {
[[if]] DEBUG
  return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
[[else]]
  return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
[[endif]]
}
```

#### native部分按需加载jsbundle

先来逐行分析这些代码的功能：

1.  `RCTBridge`的初始化：`RCTBridge`作为js和native通信的唯一桥梁需要在第一时间进行初始化，同时`RCTBridge`实例在rn项目中也是以单例的形式存在的。  
    `RCTBridge`提供了两个初始化方法，一个是基于`RCTBridgeDelegate`，另一个是基于bundle url。而两者在内部实现时，均调用了

```objectivec
- (instancetype)initWithDelegate:(id<RCTBridgeDelegate>)delegate
                       bundleURL:(NSURL *)bundleURL
                  moduleProvider:(RCTBridgeModuleListProvider)block
                   launchOptions:(NSDictionary *)launchOptions {...}
```

在完成一些赋值操作后，调用了`setUp`方法，忽略一些log相关的配置，在其中初始化了另一个bridge，即`RCTCxxBridge`。会发现几乎所有的`RCTBridge`最终的实施者都是`RCTCxxBridge`，`setUp`最核心的部分也是通过`RCTCxxBridge`的`start`方法完成的。这里附上该方法的实现，并忽略掉一些非核心功能的代码：

```objectivec
- (void)start
{
...
  // init JS thread
...
  dispatch_group_t prepareBridge = dispatch_group_create();

// register native modules
  [self registerExtraModules];
...
  __weak RCTCxxBridge *weakSelf = self;

  // Prepare executor factory (shared_ptr for copy into block)
...

  // Dispatch the instance initialization as soon as the initial module metadata has
  // been collected (see initModules)
  dispatch_group_enter(prepareBridge);
  [self ensureOnJavaScriptThread:^{
    [weakSelf _initializeBridge:executorFactory];
    dispatch_group_leave(prepareBridge);
  }];

  // Load the source asynchronously, then store it for later execution.
  dispatch_group_enter(prepareBridge);
  __block NSData *sourceCode;
  [self loadSource:^(NSError *error, RCTSource *source) {
    if (error) {
      [weakSelf handleError:error];
    }
    sourceCode = source.data;
    dispatch_group_leave(prepareBridge);
  } onProgress:^(RCTLoadingProgress *progressData) {
    // display loading progress ...
  }];

  // Wait for both the modules and source code to have finished loading
  dispatch_group_notify(prepareBridge, dispatch_get_global_queue(QOS_CLASS_USER_INTERACTIVE, 0), ^{
    RCTCxxBridge *strongSelf = weakSelf;
    if (sourceCode && strongSelf.loading) {
      [strongSelf executeSourceCode:sourceCode sync:NO];
    }
  });
}
```

可以看到，这个方法最主要的目的就是将jsbundle加载进内存，也就是`dispatch_group_notify`中的那部分代码，执行了`executeSourceCode`。进入该方法的实现：

```objectivec
- (void)executeSourceCode:(NSData *)sourceCode sync:(BOOL)sync
{
  // This will get called from whatever thread was actually executing JS.
  dispatch_block_t completion = ^{
    // Log start up metrics early before processing any other js calls
    [self logStartupFinish];
    // Flush pending calls immediately so we preserve ordering
    [self _flushPendingCalls];

    // Perform the state update and notification on the main thread, so we can't run into
    // timing issues with RCTRootView
    dispatch_async(dispatch_get_main_queue(), ^{
      [[NSNotificationCenter defaultCenter]
       postNotificationName:RCTJavaScriptDidLoadNotification
       object:self->_parentBridge userInfo:@{@"bridge": self}];

      // Starting the display link is not critical to startup, so do it last
      [self ensureOnJavaScriptThread:^{
        // Register the display link to start sending js calls after everything is setup
        [self->_displayLink addToRunLoop:[NSRunLoop currentRunLoop]];
      }];
    });
  };

  if (sync) {
    [self executeApplicationScriptSync:sourceCode url:self.bundleURL];
    completion();
  } else {
    [self enqueueApplicationScript:sourceCode url:self.bundleURL onComplete:completion];
  }
...
}
```

它有两种模式，同步和异步，但结果都是调用了`completion`block，在其中发出了一个通知`RCTJavaScriptDidLoadNotification`，表明加载已完成。

通过上述分析，我们得到了两个重要的信息，其一，初始化bridge时，完成了jsbundle的加载；其二，加载是通过调用`executeSourceCode`实现的；其三，加载完成是通过Notification的方式通知外界的。

2.  `RCTRootView`的初始化，先看代码：

```objectivec
- (instancetype)initWithBridge:(RCTBridge *)bridge
                    moduleName:(NSString *)moduleName
             initialProperties:(NSDictionary *)initialProperties
{
  ...
  if (self = [super initWithFrame:CGRectZero]) {
    self.backgroundColor = [UIColor whiteColor];

    _bridge = bridge;
    _moduleName = moduleName;
    _appProperties = [initialProperties copy];
    _loadingViewFadeDelay = 0.25;
    _loadingViewFadeDuration = 0.25;
    _sizeFlexibility = RCTRootViewSizeFlexibilityNone;

    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(bridgeDidReload)
                                                 name:RCTJavaScriptWillStartLoadingNotification
                                               object:_bridge];

    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(javaScriptDidLoad:)
                                                 name:RCTJavaScriptDidLoadNotification
                                               object:_bridge];

    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(hideLoadingView)
                                                 name:RCTContentDidAppearNotification
                                               object:self];

    [self showLoadingView];

    // Immediately schedule the application to be started.
    // (Sometimes actual `_bridge` is already batched bridge here.)
    [self bundleFinishedLoading:([_bridge batchedBridge] ?: _bridge)];
  }
  ...
  return self;
}
```

通过代码可以看到，它和初始化UIView没有什么本质区别，它本身也是UIView的一个子类。不同之处在于在初始化时注册了三个通知的监听事件，其中最核心的便是`RCTJavaScriptDidLoadNotification`通知，也是RCTCxxBridge在完成jsbundle加载后发出的通知。收到通知后，它调用了：

```cpp
[bridge enqueueJSCall:@"AppRegistry"
               method:@"runApplication"
                 args:@[moduleName, appParameters]
           completion:NULL];
```

AppRegistry是JS运行所有React Native应用的入口。应用的根组件应当通过AppRegistry.registerComponent方法注册自己，然后原生系统才可以加载应用的代码包并且在启动完成之后通过调用AppRegistry.runApplication来真正运行应用。

根据我们的需求，common.jsbundle是要在启动时加载，在加载完成后再加载main_app.jsbundle来显示主页面。由上述代码可见，初始化bridge时，会加载指定的jsbundle，我们可以通过bridge的初始化来加载common.jsbundle。在获取到`RCTJavaScriptDidLoadNotification`通知后，再使用`executeSourceCode`来加载main_app.jsbundle。由于`executeSourceCode`是一个私有方法，就需要添加一个Category来调用它，具体代码如下：

```objectivec
@interface RCTBridge (RnLoadJS)
- (void)executeSourceCode:(NSData *)sourceCode sync:(BOOL)sync;
@end
```

接下来我们再来看看这个完整的AppDelegate：

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  [[NSNotificationCenter defaultCenter] addObserver:self
                                           selector:@selector(javaScriptDidLoad:)
                                               name:RCTJavaScriptDidLoadNotification
                                             object:nil];
  NSURL *main = [[NSBundle mainBundle] URLForResource:@"common" withExtension:@"jsbundle"];
  RCTBridge *bridge = [[RCTBridge alloc] initWithBundleURL:main moduleProvider:nil launchOptions:launchOptions];
  [[RNJSLoader sharedInstance] setupBridge:bridge];
  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  self.window.rootViewController = [self loadingViewController];
  [self.window makeKeyAndVisible];
  return YES;
}

- (void)javaScriptDidLoad:(NSNotification *)notification {
  [[NSNotificationCenter defaultCenter] removeObserver:self];
  NSURL *main = [[NSBundle mainBundle] URLForResource:@"main_app" withExtension:@"jsbundle"];
  ReactViewController *rootVC = [[ReactViewController alloc] initWithModuleName:@"split_jsbundle" url:main];
  self.rootNavigationController = [[UINavigationController alloc] initWithRootViewController:rootVC];
  [self.rootNavigationController setNavigationBarHidden:YES animated:NO];
  self.window.rootViewController = self.rootNavigationController;
}

- (UIViewController *)loadingViewController {
  UIViewController *vc = [[UIViewController alloc] init];
  vc.view.backgroundColor = [UIColor whiteColor];
  return vc;
}
```

其中涉及到两个辅助类`ReactViewController`和`RNJSLoader`。代码如下所示：  
`ReactViewController`:

```objectivec
[[import]] <React/RCTBridge.h>
[[import]] <React/RCTRootView.h>
[[import]] "ReactViewController.h"
[[import]] "RNJSLoader.h"

@interface ReactViewController ()
@property (nonatomic, strong) NSURL* url;
@property (nonatomic, strong) NSString* name;
@end

@implementation ReactViewController

- (instancetype)initWithModuleName:(NSString *)name url:(NSURL *)url {
  if (self = [super init]) {
    self.url = url;
    self.name = name;
  }
  return self;
}

- (void)viewDidLoad {
  [super viewDidLoad];
  self.view.backgroundColor = [UIColor whiteColor];
  if ([[RNJSLoader sharedInstance] loadJSModule:self.name path:self.url]) {
    [self initView];
  }
}

- (void)initView {
  RCTBridge *bridge = [RNJSLoader sharedInstance].bridge;
  RCTRootView* view = [[RCTRootView alloc] initWithBridge:bridge moduleName:self.name initialProperties:nil];
  view.frame = self.view.bounds;
  view.backgroundColor = [UIColor whiteColor];
  [self setView:view];
}

@end
```

`RNJSLoader`:

```objectivec
// .h file
[[import]] <React/RCTBridge.h>

@interface RNJSLoader : NSObject

@property(nonatomic, strong, readonly)RCTBridge *bridge;
@property(nonatomic, strong, readonly)NSDictionary<NSString *, NSString *> *existingModues;

+ (instancetype)sharedInstance;
- (void)setupBridge:(RCTBridge *)bridge;
- (BOOL)loadJSModule:(NSString *)name path:(NSURL *)url;

@end

// .m file
[[import]] "RNJSLoader.h"
[[import]] <React/RCTBridge+Private.h>

@interface RCTBridge (RnLoadJS)
- (void)executeSourceCode:(NSData *)sourceCode sync:(BOOL)sync;
@end

@interface RNJSLoader ()
@property(nonatomic, strong) RCTBridge *bridge;
@property(nonatomic, strong) NSDictionary<NSString *, NSString *> *existingModues;
@property(nonatomic, strong) NSMutableDictionary<NSString *, NSURL *> *jsModules;
@end

@implementation RNJSLoader

+ (instancetype)sharedInstance {
  static RNJSLoader *sharedInstance = nil;
  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
  });
  return sharedInstance;
}

- (instancetype)init {
  if (self = [super init]) {
    self.jsModules = [NSMutableDictionary dictionary];
    self.existingModues = @{@"split_jsbundle": @"main_app", @"rewards": @"sub_app"};
  }
  return self;
}

- (void)setupBridge:(RCTBridge *)bridge {
  self.bridge = bridge;
}

- (BOOL)loadJSModule:(NSString *)name path:(NSURL *)url {
  if (!self.bridge) {
    return NO;
  }
  NSURL *cachedUrl = [self.jsModules objectForKey:name];
  if (cachedUrl) {
    return YES;
  }
  NSError *error = nil;
  NSData *sourceData = [NSData dataWithContentsOfURL:url options:NSDataReadingMappedIfSafe error:&error];
  if (error) {
    return NO;
  }
  
  [self.bridge.batchedBridge executeSourceCode:sourceData sync:NO];
  [self.jsModules setValue:url forKey:name];
  return YES;
}

@end

```

#### React Native 触发 加载sub_app.jsbundle

由于触发是发生在JS端，所以这里就需要一个react native的native module来实现该功能了，这里没有什么特别需要说明的，直接上代码：

```objectivec
// .h file
[[import]] <React/RCTBridgeModule.h>
@interface RNNavigation : NSObject<RCTBridgeModule>
@end

// .m file
[[import]] "RNNavigation.h"
[[import]] "AppDelegate.h"
[[import]] "RNJSLoader.h"
[[import]] "ReactViewController.h"

@implementation RNNavigation

RCT_EXPORT_MODULE();

RCT_EXPORT_METHOD(navigateTo:(NSString *)name)
{
  NSString *bundleName = [[[RNJSLoader sharedInstance] existingModues] objectForKey:name];
  if (!bundleName) {
    return;
  }
  AppDelegate *delegate = (AppDelegate *)UIApplication.sharedApplication.delegate;
  NSURL *app = [[NSBundle mainBundle] URLForResource:bundleName withExtension:@"jsbundle"];
  ReactViewController *vc = [[ReactViewController alloc] initWithModuleName:name url:app];
  [delegate.rootNavigationController pushViewController:vc animated:YES];
}

RCT_EXPORT_METHOD(goBack)
{
  AppDelegate *delegate = (AppDelegate *)UIApplication.sharedApplication.delegate;
  [delegate.rootNavigationController popViewControllerAnimated:YES];
}

- (dispatch_queue_t)methodQueue {
  return dispatch_get_main_queue();
}

@end
```

  
  
作者：西西的一天  
链接：https://www.jianshu.com/p/832d7c01b101  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
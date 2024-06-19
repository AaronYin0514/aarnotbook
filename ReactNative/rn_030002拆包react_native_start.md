# React Native 拆包实践2 - react-native start

在上一节中，找到了react-native启动bundle server的入口，即`runServer`函数，它的定义为：

```tsx
async function runServer(_argv: Array<string>, ctx: Config, args: Args)
```

函数有三个入参，调用这个函数需要传入参数，那么是谁来调用这个函数呢？在server.ts中声明了这个command：

```css
export default {
  name: 'start',
  func: runServer,
  description: 'starts the webserver',
  options: [
    {
      name: '--port [number]',
      parse: (val: string) => Number(val),
    },
    ...
  ],
};
```

可以看到在声明中有一组options，也就是我们在调用`react-native start`时可以带一些参数，例如带第一个option后`react-native start --port 8888`来指定端口号。那么是谁来将这个`start command`转化为`runServer`的函数调用并把options转化为函数的入参呢，它就是[commander.js](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Ftj%2Fcommander.js)。到此，我们知道了bundle server启动的流程`react-native start` -> `commander.js` -> `runServer` -> `metro.js`。在解析`runServer`之前，需要先了解一下metro的核心概念，它有助于我们理解runServer函数的实现

### Metro.js

metro是一个JavaScript的bundler，用于打包React-Native的js代码以及assets文件。metro接收一个index.js也就是RN的入口文件和其他打包选项，最终生成一个jsbundle文件，其中包括了所有js代码以及第三方依赖的js代码。当我们有一些资源文件时，也会将这些资源文件（例如图片）按照一个rn可读取的目录结构复制到指定的目录。

> 例如在打包bundle时执行`react-native bundle --platform ios --dev false --entry-file index.js --bundle-output build/index.ios.jsbundle`命令，其中传入了一个`--entry-file index.js`就是用于指定这个入口文件。

在这个打包过程中，一共包含了三个阶段：

1.  Resolution  
    解析，通过入口文件解析整个依赖关系，比如index.js中会import其他js文件，依次类推，解析所有依赖的js文件
2.  Transformation  
    转换，所有的js文件需要被转换为指定平台（react-native）所能解析的语法格式，类似与 [Babel](https://links.jianshu.com/go?to=https%3A%2F%2Fbabeljs.io%2F)所做的工作。在执行过程中，该步骤可以与步骤1，Resolution并行执行
3.  Serialization  
    序列号，在这一步中，会将各个模块转换后的js文件合并生成一个或多个jsbundle文件。通常如果我们没有做任何配置，rn只会生成一个jsbundle文件。

根据Metro官方文档中的Quick Start，我们可以更好的理解这一过程（ps：这个Quick Start只是一个示例，仅能用于非常简单的玩具rn项目）

-   编译  
    Metro可通过npm install进行安装，此处指定了入口和出口文件

```csharp
const config = await Metro.loadConfig();

await Metro.runBuild(config, {
  entry: 'index.js',
  out: 'bundle.js',
});
```

-   运行bundle server  
    获取到metro config后，可以根据这个配置运行一个bundle server

```csharp
const config = await Metro.loadConfig();

await Metro.runServer(config, {
  port: 8080,
});
```

-   在bundle server中加入一些中间件  
    启动http server是可以加入一些中间件，也可以使用expressjs

```php
const Metro = require('metro');
const express = require('express');
const app = express();
const server = require('http').Server(app);

Metro.loadConfig().then(async config => {
  const connectMiddleware = await Metro.createConnectMiddleware(config);
  const {server: {port}} = config;

  app.use(connectMiddleware.middleware);
  server.listen(port);
  connectMiddleware.attachHmrServer(server);
});
```

在了解了metro的一些核心概念后，metro server启动的流程大致可以总结为：读取配置 -> 配置中间件 -> 启动bundle server。有了这个认识后，我们可以看看runServer的实现了。

### runServer 分析

先上源码吧

```javascript
async function runServer(_argv: Array<string>, ctx: Config, args: Args) {
  // 初始化相关的配置，配置std out
  let eventsSocket:
    | ReturnType<typeof eventsSocketModule.attachToServer>
    | undefined;
  const terminal = new Terminal(process.stdout);
  const ReporterImpl = getReporterImpl(args.customLogReporterPath);
  const terminalReporter = new ReporterImpl(terminal);
  const reporter = {
    update(event: any) {
      terminalReporter.update(event);
      if (eventsSocket) {
        eventsSocket.reportEvent(event);
      }
    },
  };

  // 获取 metro config
  const metroConfig = await loadMetroConfig(ctx, {
    config: args.config,
    maxWorkers: args.maxWorkers,
    port: args.port,
    resetCache: args.resetCache,
    watchFolders: args.watchFolders,
    projectRoot: args.projectRoot,
    sourceExts: args.sourceExts,
    reporter,
  });

  // 配置asset插件
  if (args.assetPlugins) {
    metroConfig.transformer.assetPlugins = args.assetPlugins.map(plugin =>
      require.resolve(plugin),
    );
  }

  // 中间件配置
  const middlewareManager = new MiddlewareManager({
    host: args.host,
    port: metroConfig.server.port,
    watchFolders: metroConfig.watchFolders,
  });

  middlewareManager.getConnectInstance().use(
    // @ts-ignore morgan and connect types mismatch
    morgan(
      'combined',
      !logger.isVerbose()
        ? {
            skip: (_req, res) => res.statusCode < 400,
          }
        : undefined,
    ),
  );

  metroConfig.watchFolders.forEach(
    middlewareManager.serveStatic.bind(middlewareManager),
  );

  const customEnhanceMiddleware = metroConfig.server.enhanceMiddleware;

  metroConfig.server.enhanceMiddleware = (middleware: any, server: unknown) => {
    if (customEnhanceMiddleware) {
      middleware = customEnhanceMiddleware(middleware, server);
    }

    return middlewareManager.getConnectInstance().use(middleware);
  };

  // 运行metro server
  const serverInstance = await Metro.runServer(metroConfig, {
    host: args.host,
    secure: args.https,
    secureCert: args.cert,
    secureKey: args.key,
    hmrEnabled: true,
  });

  // 配置开发相关的socket中间件
  const wsProxy = webSocketProxy.attachToServer(
    serverInstance,
    '/debugger-proxy',
  );
  const ms = messageSocket.attachToServer(serverInstance, '/message');
  eventsSocket = eventsSocketModule.attachToServer(
    serverInstance,
    '/events',
    ms,
  );

  middlewareManager.attachDevToolsSocket(wsProxy);
  middlewareManager.attachDevToolsSocket(ms);

  if (args.interactive) {
    enableWatchMode(ms);
  }

  middlewareManager
    .getConnectInstance()
    .use('/reload', (_req: http.IncomingMessage, res: http.ServerResponse) => {
      ms.broadcast('reload');
      res.end('OK');
    });

  serverInstance.keepAliveTimeout = 30000;

  await releaseChecker(ctx.root);
}
```

源码中，可以看到`loadMetroConfig`用于获取metro config，和官方文档相比，这里添加了一下额外的参数。参数的含义通过名字可以了解一二，具体可以参考官方文档。在获取metro config之前，还做了一下初始化操作，例如启动一个terminal session，用于输出日志。

接下来，是获取assert插件，来处理资源文件

```jsx
  if (args.assetPlugins) {
    metroConfig.transformer.assetPlugins = args.assetPlugins.map(plugin =>
      require.resolve(plugin),
    );
  }
```

之后开始配置一些server中间件，file watcher，用于监控js文件的改变。处理完成后，便是server的启动了，这里和示例的类似，通过metro config和一些额外的参数启动server

```csharp
  const serverInstance = await Metro.runServer(metroConfig, {
    host: args.host,
    secure: args.https,
    secureCert: args.cert,
    secureKey: args.key,
    hmrEnabled: true,
  });
```

在server运行后，有配置了一些和开发相关websocket的中间件。
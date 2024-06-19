# React Native 拆包实践3 - react-native bundle

下面我们来看看`react-native bundle`的实现，`bundle`也是`react-native`的一个子命令，和`start`(ps: 实际在代码里是`server`)同级：

```javascript
// cli/packages/cli/src/commands/index.ts
export const projectCommands = [
  server,
  bundle,
  ...
] as Command[];
```

跳入bundle的实现，在下面的代码中给出了注释，主要的流程可以总结为：解析参数 -> 启动metro server -> 打包js和资源文件，并保存于磁盘指定路径 -> 停止metro server

```tsx
// cli/packages/cli/src/commands/bundle/buildBundle.ts
async function buildBundle(
  args: CommandLineArgs,
  ctx: Config,
  output: typeof outputBundle = outputBundle,
) {
  const config = await loadMetroConfig(ctx, {
    maxWorkers: args.maxWorkers,
    resetCache: args.resetCache,
    config: args.config,
  });
  // 判断打包的平台是ios，android或是web，如果都不是，则抛出异常，结束打包
  if (config.resolver.platforms.indexOf(args.platform) === -1) {
    ...
    throw new Error('Bundling failed');
  }

  // This is used by a bazillion of npm modules we don't control so we don't
  // have other choice than defining it as an env variable here.
  process.env.NODE_ENV = args.dev ? 'development' : 'production';
  // 根据命令行的入参 --sourcemap-output 构建 sourceMapUrl
  // --sourcemap-output: File name where to store the sourcemap file for resulting bundle
  let sourceMapUrl = args.sourcemapOutput;
  if (sourceMapUrl && !args.sourcemapUseAbsolutePath) {
    sourceMapUrl = path.basename(sourceMapUrl);
  }
  // 根据解析得到参数，构建RequestOptions，传递给打包函数
  const requestOpts: RequestOptions = {
    entryFile: args.entryFile,  // 入口文件，也就是react-native生成模板工程的index.js
    sourceMapUrl,
    dev: args.dev, // 生产环境还是开发环境
    minify: args.minify !== undefined ? args.minify : !args.dev, // 是否压缩生成的jsbundle
    platform: args.platform,
  };

  const server = new Server(config);

  try {
    // 开始打包
    const bundle = await output.build(server, requestOpts);
    // 将打包生成的bundle存储在--bundle-output指定的位置
    await output.save(bundle, args, logger.info);
    // 处理资源文件，解析，并在下一步保存在--assets-dest指定的位置
    const outputAssets: AssetData[] = await server.getAssets({
      ...Server.DEFAULT_BUNDLE_OPTIONS,
      ...requestOpts,
      bundleType: 'todo',
    });
    return await saveAssets(outputAssets, args.platform, args.assetsDest);
  } finally {
    server.end(); // 所有工作处理后，即可停止server
  }
}
```

`sourceMapUrl`?  
从上述代码可以看到具体的打包实现都在`output.build(server, requestOpts)`中，output是outputBundle类型，这部分代码在[Metro JS](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Ffacebook%2Fmetro)中，具体的路径为：`metro/packages/metro/src/shared/output/bundle.js`

```javascript
...
function buildBundle(
  packagerClient: Server,
  requestOptions: RequestOptions,
): Promise<{
  code: string,
  map: string,
  ...
}> {
  return packagerClient.build({
    ...Server.DEFAULT_BUNDLE_OPTIONS,
    ...requestOptions,
    bundleType: 'bundle',
  });
}
...
exports.build = buildBundle;
```

可以看到这里的packagerClient就是从外面传入的server，就是Metro Server，加了这一层封装的目的是为了在`requestOptions`中添加一个`bundleType`参数，值为`'bundle'`。`Server.build`的源码则在`metro/packages/metro/src/Server.js`中定义：

```kotlin
  async build(options: BundleOptions): Promise<{code: string, map: string, ...}> {
    const {
      entryFile,
      graphOptions,
      onProgress,
      serializerOptions,
      transformOptions,
    } = splitBundleOptions(options);
    // Resolution和Transformation
    const {prepend, graph} = await this._bundler.buildGraph(
      entryFile,
      transformOptions,
      {
        onProgress,
        shallow: graphOptions.shallow,
      },
    );
    // 开始 Serialization
    const entryPoint = path.resolve(this._config.projectRoot, entryFile);
    const bundleOptions = {
      asyncRequireModulePath: this._config.transformer.asyncRequireModulePath,
      processModuleFilter: this._config.serializer.processModuleFilter,
      createModuleId: this._createModuleId,
      getRunModuleStatement: this._config.serializer.getRunModuleStatement,
      dev: transformOptions.dev,
      projectRoot: this._config.projectRoot,
      modulesOnly: serializerOptions.modulesOnly,
      runBeforeMainModule: this._config.serializer.getModulesRunBeforeMainModule(
        path.relative(this._config.projectRoot, entryPoint),
      ),
      runModule: serializerOptions.runModule,
      sourceMapUrl: serializerOptions.sourceMapUrl,
      sourceUrl: serializerOptions.sourceUrl,
      inlineSourceMap: serializerOptions.inlineSourceMap,
    };
    let bundleCode = null;
    let bundleMap = null;
    if (this._config.serializer.customSerializer) {
      const bundle = this._config.serializer.customSerializer(
        entryPoint,
        prepend,
        graph,
        bundleOptions,
      );
      if (typeof bundle === 'string') {
        bundleCode = bundle;
      } else {
        bundleCode = bundle.code;
        bundleMap = bundle.map;
      }
    } else {
      bundleCode = bundleToString(
        baseJSBundle(entryPoint, prepend, graph, bundleOptions),
      ).code;
    }
    if (!bundleMap) {
      bundleMap = sourceMapString(
        [...prepend, ...this._getSortedModules(graph)],
        {
          excludeSource: serializerOptions.excludeSource,
          processModuleFilter: this._config.serializer.processModuleFilter,
        },
      );
    }
    return {
      code: bundleCode,
      map: bundleMap,
    };
  }
```

在这个`build`函数中，首先执行了`buildGraph`，而`this._bundler`的初始化发生在Server的constructor中。

```javascript
  constructor(config: ConfigT, options?: ServerOptions) {
    ...
    this._createModuleId = config.serializer.createModuleIdFactory();
    this._bundler = new IncrementalBundler(config, {
      watch: options ? options.watch : undefined,
    });
    this._nextBundleBuildID = 1;
  }
```

此处的`_bundler`是`IncrementalBundler`的实例，它的`buildGraph`函数完成了打包过程中前两步Resolution和Transformation。可以跳入定义`metro/packages/metro/src/IncrementalBundler.js`查看它的完整实现。

constructor中除了配置了一些参数外，初始化了_bundler，还有我们之后再拆包中一个核心的函数`createModuleIdFactory`，它是从`config.serializer`中获取的。

> `createModuleIdFactory`负责固定 module 的 ID。在打包好的jsbundle中，__d中定义的各个 module 后都有一个数字表示，并在jsbundle文件最后的 require 方法中进行调用（如 require(41)），这其中的数字就是createModuleIdFactory方法生成的。

完成了Resolution和Transformation后，便是Serialization了。首先创建了一个`bundleOptions`，包含了所有Serialization中需要用到的参数，其中这两个是我们的重点：

```css
processModuleFilter: this._config.serializer.processModuleFilter,
createModuleId: this._createModuleId,
```

`this._createModuleId`在初始化时已经创建好了，另一个`processModuleFilter`，它是用于过滤Transformation的结果，决定哪些module可以被序列化到最终输出的jsbundle里。它是我们拆包的另一个核心函数。

接下来，通过判断有没有自定义的`Serializer`，决定由谁来序列化。当没有自定一个serializer时，将才有自带的`bundleToString`函数进行序列化，序列化前，根据上一步结果通过`baseJSBundle`构建一个Bundle对象。  
`bundleToString`的入参是Bundle，根据`baseJSBundle`的返回值我们可以知道Bundle的内部结构是这样的：

```css
{
  pre: string,
  post: string,
  modules: [[number, string]],
}
```

其中最主要的是这个modules，看似是一个二维数组，其实可以理解为Tuple的数组，一个Tuple代表了一个js module，number是它的id，这个id是由`createModuleId`产生的，string就是这个module的js代码的序列化结果。

有了这个认识后，bundleToString的工作就是很明显很多，`sortedModules`是将js module干id进行了一个排序。再遍历这个数组，将module code进行了一个字符串拼接，拼接到code变量上。code上还同时拼接了pre和post。

```tsx
function bundleToString(
  bundle: Bundle,
): {|+code: string, +metadata: BundleMetadata|} {
  let code = bundle.pre.length > 0 ? bundle.pre + '\n' : '';
  const modules = [];
  const sortedModules = bundle.modules
    .slice()
    .sort((a: [number, string], b: [number, string]) => a[0] - b[0]);

  for (const [id, moduleCode] of sortedModules) {
    if (moduleCode.length > 0) {
      code += moduleCode + '\n';
    }
    modules.push([id, moduleCode.length]);
  }

  if (bundle.post.length > 0) {
    code += bundle.post;
  } else {
    code = code.slice(0, -1);
  }

  return {
    code,
    metadata: {pre: bundle.pre.length, post: bundle.post.length, modules},
  };
}
```

接下来，我们可以看看`baseJSBundle`的实现。有了对`bundleToString`的理解，这部分代码就很好懂了，其中调用了三次`processModules`函数，分别用于生产preCode，postCode，以及modules。

```tsx
function baseJSBundle(
  entryPoint: string,
  preModules: $ReadOnlyArray<Module<>>,
  graph: Graph<>,
  options: SerializerOptions,
): Bundle {
  for (const module of graph.dependencies.values()) {
    options.createModuleId(module.path);
  }

  const processModulesOptions = {
    filter: options.processModuleFilter,
    createModuleId: options.createModuleId,
    dev: options.dev,
    projectRoot: options.projectRoot,
  };

  // Do not prepend polyfills or the require runtime when only modules are requested
  if (options.modulesOnly) {
    preModules = [];
  }

  const preCode = processModules(preModules, processModulesOptions)
    .map(([_, code]) => code)
    .join('\n');

  const modules = [...graph.dependencies.values()].sort(
    (a: Module<MixedOutput>, b: Module<MixedOutput>) =>
      options.createModuleId(a.path) - options.createModuleId(b.path),
  );

  const postCode = processModules(
    getAppendScripts(
      entryPoint,
      [...preModules, ...modules],
      graph.importBundleNames,
      {
        asyncRequireModulePath: options.asyncRequireModulePath,
        createModuleId: options.createModuleId,
        getRunModuleStatement: options.getRunModuleStatement,
        inlineSourceMap: options.inlineSourceMap,
        projectRoot: options.projectRoot,
        runBeforeMainModule: options.runBeforeMainModule,
        runModule: options.runModule,
        sourceMapUrl: options.sourceMapUrl,
        sourceUrl: options.sourceUrl,
      },
    ),
    processModulesOptions,
  )
    .map(([_, code]) => code)
    .join('\n');

  return {
    pre: preCode,
    post: postCode,
    modules: processModules(
      [...graph.dependencies.values()],
      processModulesOptions,
    ).map(([module, code]) => [options.createModuleId(module.path), code]),
  };
}
```

在`processModules`中，主要的作用就是一个filter，进行了两次filter，第一次过滤出所有的js module；第二次filter是有外面传入的filter决定的。

```tsx
function processModules(
  modules: $ReadOnlyArray<Module<>>,
  {
    filter = () => true,
    createModuleId,
    dev,
    projectRoot,
  }: {|
    +filter?: (module: Module<>) => boolean,
    +createModuleId: string => number,
    +dev: boolean,
    +projectRoot: string,
  |},
): $ReadOnlyArray<[Module<>, string]> {
  return [...modules]
    .filter(isJsModule)
    .filter(filter)
    .map((module: Module<>) => [
      module,
      wrapModule(module, {
        createModuleId,
        dev,
        projectRoot,
      }),
    ]);
}
```

### 总结

到此我们已经完整了梳理了一遍bundle的执行过程，也从中了解到我们最关注的两个函数`createModuleIdFactory`和`processModuleFilter`是何时被触发的。下一节中，我们就一起来实现自己的`createModuleIdFactory`和`processModuleFilter`。

# React Native 拆包 - createModuleIdFactory

`createModuleIdFactory`负责固定module的ID。在打包生成的`jsbundle`中，`__d`中定义的各个`module`后都有一个数字表示，并在最后的`require`方法中进行调用，这其中的数字就是createModuleIdFactory方法生成的。如果添加了新的`module`，比如说引入了新的三方库，那么ID会重新生成。下面的实例代码便是通过常规的package命令行代码`react-native bundle --platform ios --dev false --entry-file index.js --bundle-output build/ios/main.jsbundle --assets-dest build/ios/`打包生成的`main.jsbundle`

```jsx
// 全局函数和全局变量的定义，其中也包括了__d函数的定义
var __BUNDLE_START_TIME__=...
...
__d(function(...){},123,[]);
...
__d(function(a,s,t,e,n,r,u){n.exports={name:"statusbar",displayName:"statusbar"}},484,[]);
__r(82);
__r(0);
```

其中使用最多的就是这个`__d`函数，每一个js module打包后都会对应一个`__d`函数，从头部的全局函数定义中，可以把该函数分离出来：

```jsx
__d = function (r, i, n) {
    if (null != e[i]) return;
    var o = {
        dependencyMap: n,
        factory: r,
        hasError: !1,
        importedAll: t,
        importedDefault: t,
        isInitialized: !1,
        publicModule: { exports: {} }
    };
    e[i] = o
}
```

它由三个入参  
r：js module的具体实现；  
i：当前js module的Id  
n：该js module所依赖的其他js module的Id  
由这三个参数，在配合其他的一些全局变量，构建出了一个对象`o`，再把`o`绑在object `e`中，从此处可以看到`i`也就是这个Id必须是唯一的。

当我们需要拆包时，公共module的ID每次构建时必须是固定的，因此0.56+版本的`metro`可以通过自定义`createModuleIdFactory`方法将基础module的ID固定下来。

先来看看metro中该方法的实现`metro/src/lib/createModuleIdFactory.js`

```jsx
'use strict';

function createModuleIdFactory(): (path: string) => number {
  const fileToIdMap: Map<string, number> = new Map();
  let nextId = 0;
  return (path: string) => {
    let id = fileToIdMap.get(path);
    if (typeof id !== 'number') {
      id = nextId++;
      fileToIdMap.set(path, id);
    }
    return id;
  };
}

module.exports = createModuleIdFactory;
```

这段代码比较简单，很容易读懂，参数`path`是module的路径，比如`node_modules/react-native/Libraries/Image/Image.ios.js`。在函数内部定义了两个局部变量`fileToIdMap`和`nextId`，这两个变量在一次打包时是被共享了，类似于static。每次调用时，会先检查`fileToIdMap`中是否已经有这个路径了，如果有，直接返回对应的Id，如果没有，nextId加一并将这个path存入map中，再返回Id。

可以使用module的相对路径作为这个id，根据这个思路来提供自定义的`createModuleIdFactory`

```jsx
'use strict';
const path = require('path');
const pathSep = path.posix.sep;

function createModuleIdFactory() {
  const fileToIdMap = new Map();
  const projectRootPath = `${process.cwd()}`;
  return (path) => {
    let moduleName = '';
    if (path.indexOf(`node_modules${pathSep}react-native${pathSep}Libraries${pathSep}`) > 0) {
      moduleName = path.substr(path.lastIndexOf(pathSep) + 1);
    } else if (path.indexOf(projectRootPath) === 0) {
      moduleName = path.substr(projectRootPath.length + 1);
    }
    moduleName = moduleName.replace('.js', '');
    moduleName = moduleName.replace('.png', '');
    let regExp = pathSep === '\\' ? new RegExp('\\\\', 'gm') : new RegExp(pathSep, 'gm');
    moduleName = moduleName.replace(regExp, '_');
    return moduleName;
  };
}

module.exports = createModuleIdFactory;
```

其实可以直接使用path，但传入的path参数，是该module的绝对路径，也就是说在path中有一个完全相同的前缀，所以此处将前缀去掉，而对于react-native的js module，由于路径较深，这里也将它去掉：

```jsx
if (path.indexOf(`node_modules${pathSep}react-native${pathSep}Libraries${pathSep}`) > 0) {
  moduleName = path.substr(path.lastIndexOf(pathSep) + 1);
} else if (path.indexOf(projectRootPath) === 0) {
  moduleName = path.substr(projectRootPath.length + 1);
}
```

自定义的`createModuleIdFactory`的使用方式：  
react-native项目根目录下有名为`metro.config.js`的配置文件，用于配置metro运行参数，我们需要定义的就是serializer下的createModuleIdFactory。

```jsx
const createModuleIdFactory = require('./config/createModuleIdFactory');
module.exports = {
  transformer: {
    getTransformOptions: async () => ({
      transform: {
        experimentalImportSupport: false,
        inlineRequires: false,
      },
    }),
  },
  serializer: {
    createModuleIdFactory: createModuleIdFactory
  }
};
```

配置好后，再次运行`react-native bundle --platform ios --dev false --entry-file index.js --bundle-output build/ios/main.jsbundle --assets-dest build/ios/`，便是基于新的createModuleIdFactory来创建Id的。当我们在此查看打包后的jsbundle文件时，便会看到如下结果：

```javascript
...
__d(
    function (g, r, i, a, m, e, d) {
        var t = r(d[0]);
        Object.defineProperty(e, "__esModule", { value: !0 }), e.default = void 0;
        var o = t(r(d[1])), u = r(d[2]), n = u.StyleSheet.create({ image: { height: 24 } });
        e.default = function (t) {
            var c = t.routeName, l = t.focused, f = { Home: r(l ? d[3] : d[4]), My: r(l ? d[5] : d[6]) };
            return o.default.createElement(u.Image, { style: n.image, source: f[c], resizeMode: "contain" })
        }
    },
    "src_components_TabBarIcon_index",
    [
        "node_modules_@babel_runtime_helpers_interopRequireDefault",
        "node_modules_react_index",
        "react-native-implementation",
        "src_assets_icons_home_fill",
        "src_assets_icons_home",
        "src_assets_icons_my_fill",
        "src_assets_icons_my"
    ]
);
...
```

此时，__d函数的第二第三个参数已经替换成了module的路径。

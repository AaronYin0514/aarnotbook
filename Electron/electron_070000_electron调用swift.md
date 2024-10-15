## Electron 调用 Swift：深入了解跨语言通信

### 通信机制

通常，Electron 与 Swift 之间的通信可以通过以下两种方式实现：

- **使用 Node.js 子进程**：在 Electron 中启动一个子进程运行 Swift 程序。
- **使用 Native Module**：通过编写 Node.js 插件，让 Node.js 直接调用 Swift 代码。

在此，我们主要探讨第二种方式，因为它更高效且更易于维护。

### 一、创建 Native Module

**1 创建 Swift 文件**

在项目根目录下创建一个 `SwiftModule.swift` 文件，内容如下：

```swift
import Foundation

@objc public class SwiftModule: NSObject {
    @objc public func helloWorld() -> String {
        return "Hello from Swift!"
    }
}
```

这段代码定义了一个名为 `SwiftModule` 的类，提供了一个方法 `helloWorld`，用于返回一句问候语。

**2 构建 Node.js 插件**

我们需要在项目中创建一份 `binding.gyp` 文件，用于配置编译信息：

```json
{
  "targets": [
    {
      "target_name": "SwiftModule",
      "sources": [ "SwiftModule.swift" ],
      "xcode_settings": {
        "OTHER_CPLUSPLUSFLAGS": [ "-std=c++14" ]
      }
    }
  ]
}
```

然后使用 `node-gyp` 构建这个模块：

```shell
npm install -g node-gyp
node-gyp configure
node-gyp build
```

**3 在 Electron 应用中调用**

在 Electron 的主进程中，我们可以通过 `require` 引入并使用这个模块：

```javascript
const { app, BrowserWindow } = require('electron');
const swift = require('./build/Release/SwiftModule');

let mainWindow;

app.on('ready', () => {
    mainWindow = new BrowserWindow({});
    
    // 调用 Swift 方法
    const greeting = swift.helloWorld();
    console.log(greeting); // 输出：Hello from Swift!
});
```

### 二、数据交互流程示意图

以下是 Electron 和 Swift 之间数据交互的示意图。

![](electron-swift-0001.png)





















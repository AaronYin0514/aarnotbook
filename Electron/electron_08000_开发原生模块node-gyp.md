## 开发原生模块node-gyp

### 简介

`Node-API`（`C API`）用于构建nodejs原生插件的接口，`node-addon-api`是对 `Node-API` 的`C++`封装，API更容易使用。

### 开发一个最简单原生模块

**1 安装Node**

**2 安装`node-gyp`**

```shell
npm install node-gyp -g
```

**3 创建一个新项目**

```shell
mkdir helloword
cd helloword
npm init
```

**4 安装`node-addon-api`依赖**

```shell
npm install node-addon-api --save-dev
```

**5 开发原生模块**

创建`hello.cpp`

```c++
#include <napi.h>

Napi::String Hello(const Napi::CallbackInfo& info) {
	Napi::Env env = info.Env();
	return Napi::String::New(env, "hello word");
}

Napi::Object Init(Napi::Env env, Napi::Object exports) {
	exports.Set("hello", Napi::Function::New(env, Hello));
	return exports;
}

NODE_API_MODULE(addon, Init)
```

简单说明：

- `NODE_API_MODULE`是插件的入口函数，调用初始化函数`Init`
- `Init`初始化函数，参数`exports`类似`node`中的`export`模块，提供原生API给`javascript`端调用
- `Hello`就是提供给`javascript`端调用的原生API

**6 编译配置**

创建`binding.gyp`文件，格式类似json，提供编译配置

```json
{ 
    "targets": [ 
        { 
            "target_name": "hello", 
            "sources": ["hello.cpp"],
            "include_dirs": [
                "<!@(node -p \"require('node-addon-api').include\")"
            ],
            "defines": ['NAPI_DISABLE_CPP_EXCEPTIONS']
        }
    ]
}
```

**7 编译**

```shell
node-gyp configure
node-gyp build
```

**8 `node`端调用**

创建`app.js`

```javascript
const addon = require("./build/Release/hello.node")

console.log(addon.hello())
```

执行

```shell
node app.js # hello word
```

### OC混编案例

**1 安装Node**

**2 安装`node-gyp`**

```shell
npm install node-gyp -g
```

**3 创建一个新项目**

```shell
mkdir helloword
cd helloword
npm init
```

**4 安装`node-addon-api`依赖**

```shell
npm install node-addon-api --save-dev
```

**5 开发原生模块**

创建`src`目录，并创建`OC`代码

`Person.h`

```object-c
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface Person : NSObject

@property (nonatomic, assign) NSInteger age;
@property (nonatomic, strong) NSString *name;

- (NSString *)sayHello;

@end

NS_ASSUME_NONNULL_END
```

`Person.m`

```object-c
#import "Person.h"

@implementation Person

- (NSString *)sayHello {
    return [NSString stringWithFormat:@"我叫%@，我今年%ld了", self.name, self.age];
}

@end
```

创建`hello.mm`

```c++
#include <napi.h>
#import "Person.h"

Napi::String Hello(const Napi::CallbackInfo& info) { 
    Napi::Env env = info.Env();
    Person *xiaoming = [Person new];
    xiaoming.name = @"小明";
    xiaoming.age = 10;
    NSString *hellostr = [xiaoming sayHello];
    return Napi::String::New(env, std::string([hellostr UTF8String])); 
} 

Napi::Object Init(Napi::Env env, Napi::Object exports) { 
    exports.Set("hello", Napi::Function::New(env, Hello));
    return exports; 
}

NODE_API_MODULE(addon, Init)
```

**6 编译配置**

创建`binding.gyp`文件，格式类似json，提供编译配置

```json
{
    "targets": [{
        "target_name": "hello",
        "sources": [],
        "conditions": [
            ['OS=="mac"', {
                "sources": [
                    "src/hello.mm",
                    "src/Person.h",
                    "src/Person.m"
                ],
            }]
        ],
        'include_dirs': [
            "<!@(node -p \"require('node-addon-api').include\")"
        ],
        'libraries': [],
        'dependencies': [
            "<!(node -p \"require('node-addon-api').gyp\")"
        ],
        'defines': ['NAPI_DISABLE_CPP_EXCEPTIONS'],
        "xcode_settings": {
            "MACOSX_DEPLOYMENT_TARGET": "10.14",
            "SYSTEM_VERSION_COMPAT": 1,
            "OTHER_CPLUSPLUSFLAGS": ["-std=c++17", "-stdlib=libc++"],
            "OTHER_LDFLAGS": []
        }
    }]
}
```

**7 编译**

```shell
node-gyp configure
node-gyp build
```

**8 `node`端调用**

创建`app.js`

```javascript
const addon = require("./build/Release/hello.node")

console.log(addon.hello())
```

执行

```shell
node app.js # 我叫小明，我今年10了
```






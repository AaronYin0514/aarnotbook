# React Native 拆包 - bundle server的启动过程

从这周开始，准备启动一个新的专题《React Native拆包实践》，目的是想完成jsbundle的拆分，分为基础包和业务包，从而实现js的按需加载，其一可以提高启动速度，其二可以变相实现一个App中同时容纳多个RN模块。

先来一个预热吧，当我们使用`react-native run-ios`或通过Xcode启动RN项目时，都会自动启动一个bundle server，用来在dev模式下加载js代码，那么这部分是如何实现的呢？使用Xcode开启RN项目，可以看到在`build phases`中有一步名为`Start Packager`的操作，其中有一段shell脚本，如下所示：

```script
export RCT_METRO_PORT="${RCT_METRO_PORT:=8081}"
echo "export RCT_METRO_PORT=${RCT_METRO_PORT}" > "${SRCROOT}/../node_modules/react-native/scripts/.packager.env"
if [ -z "${RCT_NO_LAUNCH_PACKAGER+xxx}" ] ; then
  if nc -w 5 -z localhost ${RCT_METRO_PORT} ; then
    if ! curl -s "http://localhost:${RCT_METRO_PORT}/status" | grep -q "packager-status:running" ; then
      echo "Port ${RCT_METRO_PORT} already in use, packager is either not running or not running correctly"
      exit 2
    fi
  else
    open "$SRCROOT/../node_modules/react-native/scripts/launchPackager.command" || echo "Can't start packager automatically"
  fi
fi
```

我们来逐行解释一些：

1.  `export RCT_METRO_PORT="${RCT_METRO_PORT:=8081}"` 在当前session中声明一个环境变量`RCT_METRO_PORT`，也就是这个bundle server的端口号，定义为8081
2.  `echo...` 将这个端口号写入了`.packager.env`中，用于后面使用这个端口号
3.  `if [ -z "${RCT_NO_LAUNCH_PACKAGER+xxx}" ]` 检查是否声明了`RCT_NO_LAUNCH_PACKAGER+xxx`这个环境变量，`-z`用于判断这个变量的长度是否为0，如果为0则执行then后的脚本，这个环境变量的声明用于调用方不想要启动这个server，比如在构建production包时
4.  `if nc -w 5 -z localhost ${RCT_METRO_PORT}` 当步骤3中没有声明那个环境变量时，将做这个检测。`nc -w 5 -z localhost 8081`：使用Natcat工具，`-w 5`扫描5秒，`-z localhost 8081`扫描localhost的8081端口。当这个端口已经被占用时，执行第5步，当没有被占用时，执行第7步
5.  `if ! curl -s "http://localhost:${RCT_METRO_PORT}/status" | grep -q "packager-status:running"`，当端口被占用时，需要检测一下这个端口是不是被之前启动好的bundle server在使用（比如在之前已经运行了`npm run start`）。检测的方式是向`http://localhost:${RCT_METRO_PORT}/status`发送一个get请求，再由`| grep -q "packager-status:running"`看看response中是否包含`packager-status:running`。当不包含时执行步骤6
6.  `echo "Port ${RCT_METRO_PORT} ..."`，输出一个log，之后就`exit 2`，此时也将break当前Xcode的build
7.  `open "$SRCROOT/../node_modules/react-native/scripts/launchPackager.command" || echo "Can't start packager automatically"` 运行`launchPackager.command`，如果失败则输出一个log。
8.  `launchPackager.command`是什么呢？代码如下

```shell
#!/bin/bash
# Copyright (c) Facebook, Inc. and its affiliates.
#
# This source code is licensed under the MIT license found in the
# LICENSE file in the root directory of this source tree.

# Set terminal title
echo -en "\\033]0;Metro Bundler\\a"
clear

THIS_DIR=$(cd -P "$(dirname "$(readlink "${BASH_SOURCE[0]}" || echo "${BASH_SOURCE[0]}")")" && pwd)

# shellcheck source=/dev/null
. "$THIS_DIR/packager.sh"

if [[ -z "$CI" ]]; then
  echo "Process terminated. Press <enter> to close the window"
  read -r
fi
```

其实就执行了当前目录下的另一个shell脚本packager.sh：

```shell
#!/bin/bash
# Copyright (c) Facebook, Inc. and its affiliates.
#
# This source code is licensed under the MIT license found in the
# LICENSE file in the root directory of this source tree.

# scripts directory
THIS_DIR=$(cd -P "$(dirname "$(readlink "${BASH_SOURCE[0]}" || echo "${BASH_SOURCE[0]}")")" && pwd)
REACT_NATIVE_ROOT="$THIS_DIR/.."
# Application root directory - General use case: react-native is a dependency
PROJECT_ROOT="$THIS_DIR/../../.."

# check and assign NODE_BINARY env
# shellcheck disable=SC1090
source "${THIS_DIR}/node-binary.sh"

# When running react-native tests, react-native doesn't live in node_modules but in the PROJECT_ROOT
if [ ! -d "$PROJECT_ROOT/node_modules/react-native" ];
then
  PROJECT_ROOT="$THIS_DIR/.."
fi
# Start packager from PROJECT_ROOT
cd "$PROJECT_ROOT" || exit
"$NODE_BINARY" "$REACT_NATIVE_ROOT/cli.js" start "$@"

```

做了一些check后，最后执行：`"$NODE_BINARY" "$REACT_NATIVE_ROOT/cli.js" start "$@"`，替换掉环境变量之后为：`node ../cli.js start`，$@指定的是传入的所有参数，而在调用[packager.sh](https://links.jianshu.com/go?to=http%3A%2F%2Fpackager.sh)时没有任何参数。cli.js代码如下，这个cli其实就是Metro的CLI工具了：

```javascript
#!/usr/bin/env node
/**
 * Copyright (c) Facebook, Inc. and its affiliates.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 *
 * @format
 */

'use strict';

var cli = require('@react-native-community/cli');

if (require.main === module) {
  cli.run();
}

module.exports = cli;
```

在这个js文件中仅仅是把@react-native-community/cli这个命令行工具expose出去了，在node_module下找不到这个依赖，具体的实现可查阅[https://github.com/react-native-community/cli](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Freact-native-community%2Fcli)。具体的路径为`./packages/cli/src/commands/index.ts`。由于笔者的js功底有限，并未理解`require.main === module`的含义，有兴趣可以参考stackoverflow的文章[https://stackoverflow.com/questions/45136831/node-js-require-main-module](https://links.jianshu.com/go?to=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F45136831%2Fnode-js-require-main-module)。  
`cli.run()`的实现大致如下，可以理解他做的事情就是初始化：

```javascript
async function run() {
  try {
    await setupAndRun();
  } catch (e) {
    handleError(e);
  }
}

...

async function setupAndRun() {
  // config 日志 和 运行环境
  ...
  // detachedCommands 追踪源码其实就是react-native 的 init 命令，用于创建一个rn项目
  for (const command of detachedCommands) {
    // Attaches a new command onto global `commander` instance.
    attachCommand(command);
  }

  try {
    // when we run `config`, we don't want to output anything to the console. We
    // expect it to return valid JSON
    if (process.argv.includes('config')) {
      logger.disable();
    }

    const ctx = loadConfig();

    logger.enable();
    // 继续加载其他的 命令行，只是加载并不执行，其中projectCommands包括了
    // export const projectCommands = [
    //   server,
    //   bundle,
    //   ramBundle,
    //   link,
    //   unlink,
    //   install,
    //   uninstall,
    //   upgrade,
    //   info,
    //   config,
    //   doctor,
    // ] as Command[];
    for (const command of [...projectCommands, ...ctx.commands]) {
      attachCommand(command, ctx);
    }
  } catch (e) {
    logger.enable();
    logger.debug(e.message);
    logger.debug(
      'Failed to load configuration of your project. Only a subset of commands will be available.',
    );
  }

  commander.parse(process.argv);

  if (commander.rawArgs.length === 2) {
    commander.outputHelp();
  }

  // We handle --version as a special case like this because both `commander`
  // and `yargs` append it to every command and we don't want to do that.
  // E.g. outside command `init` has --version flag and we want to preserve it.
  if (commander.args.length === 0 && commander.rawArgs.includes('--version')) {
    console.log(pkgJson.version);
  }
}
```

回到`packager.sh`，在这个脚本中，最后执行了：  
`"$NODE_BINARY" "$REACT_NATIVE_ROOT/cli.js" start "$@"`，其中这个start在哪定义的呢？  
根据上面对cli.run()的分析，其中加载了很多命令全局命令行中，其中未见`start`命令，其实他藏在server命令中，也就是projectCommands中的一个命令，它的定义如下，它的name就是`start`，而实现就是这个`runServer`：

```jsx
import path from 'path';
import runServer from './runServer';

export default {
  name: 'start',
  func: runServer,
  description: 'starts the webserver',
  options: [
    ...
  ],
};
```

终于在runServer中看到了Metro的身影。

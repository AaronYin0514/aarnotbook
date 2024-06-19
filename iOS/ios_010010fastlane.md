---
tags: ios, fastlane
---

## Apple双重认证

![[ios_010000Apple双重认证#生成苹果App专用密码]]

## 将App专用密码添加到环境变量

将生成的密码添加到环境变量，例如将下面内容添加到`～/.zshrc`文件中，并执行`source ～/.zshrc`

```shell
export FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD="App专用密码"
```

## fastlane生成session

执行下面命令，生成session

```shell
fastlane spaceauth -u APPLE_ID
```

将生成结果添加到环境变量中：

```shell
export FASTLANE_SESSION='---\n- !ruby/object:HTTP::Cookie\n name: ... accessed_at: *2\n'
```


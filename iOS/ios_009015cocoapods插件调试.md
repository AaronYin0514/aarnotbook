---
tags: ios, cocoapods, pod
---

# CocoaPods插件调试

以调试`cocoapods-packager`为例

## 调试

### 1 下载`cocoapods-packager`

下载`cocoapods-packager`源码到目标工程目录中，修改目录名称为`cocoapods-packager`

### 2 安装Gem

目标工程目录下运行`bundle init`，生成Gemfile文件并编辑

```
# frozen_string_literal: true

source "https://rubygems.org"

# gem "rails"
gem 'cocoapods-packager', path: './cocoapods-packager'
```

运行`bundle install`安装gem

### 3 调试

运行下面命令

```
bundle exec pod package KSEUSDK.podspec --verbose --force 
```

## 断点调试

参考这篇文章《[CocoaPods源码与插件断点调试](https://zhuanlan.zhihu.com/p/360699729)》

---
tags: mac, yarn
---

# yarn

## 安装

```shell
npm i -g yarn
# 查看yarn版本
yarn -V
```

## yarn常用命令

### 初始化项目

```shell
yarn init

# 跳过会话，直接通过默认值生成 package.json
yarn init --yes
# 简写
yarn init -y
```

### 安装包

```shell
yarn add 包名

# 开发环境，会记录再package.json中的devDependencies
yarn add 包名 -D

# 全局安装
yarn global add 包名
```

### 更新依赖

```shell
# 升级所有依赖项，不记录在 package.json 中
yarn upgrade

# 升级制定包
yarn upgrade 包名

# 忽略版本规则，升级到最新版本，并且更新 package.json
yarn upgrade --latest
```

### 移除依赖

```shell
yarn remove 包名
```

### 安装package.json中的所有文件

```shell
# 在 node_modules 目录安装 package.json 中列出的所又依赖
yarn

# 安装时，如果 node_modules 中有相应的包则不会重新下载
yarn install

# 强制重新下载安装
yarn install --force
```

### 运行脚本

`yarn run`用来执行在`package.json`中`scripts`属性下定义的脚本

```json
{
	"scripts": {
		"dev": "node app.js",
		"start": "node app.js"
	}
}
```

`yarn start`运行start脚本

### 显示某个包信息

```shell
yarn info 包名

# 输出json格式
yarn info 包名 --json

# 输出README部分
yarn info 包名 readme
```

### 列出项目的所有依赖

```shell
# 列出当前项目的依赖
yarn list

# 限制依赖的深度
yarn list --depth=0

# 列出全局安装的模块
sudo yarn global list
```

### 缓存

```shell
# 列出已缓存的每个包
sudo yarn cache list

# 返回 全局缓存位置
sudo yarn cache dir

# 清楚缓存
sudo yarn cache clean
```
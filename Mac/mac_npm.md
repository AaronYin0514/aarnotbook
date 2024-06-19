---
tags: mac, npm
---

# npm

## npm常用命令

### 初始化项目

```shell
npm init

# 跳过会话，直接通过默认值生成 package.json
npm init -y
```

### 安装包

```shell
npm install 包名

# 简写
npm i 包名

# 开发环境，会记录再package.json中的devDependencies
npm i 包名 -D

# 全局安装
npm i 包名 -g
```

### 更新依赖

```shell
# 升级所有依赖项，不记录在 package.json 中
# 可以通过 ‘--save|-D’ 指定升级哪类依赖
npm update

# 升级指定包
npm update 包名
```

### 移除依赖

```shell
npm uninstall 包名
```

### 安装package.json文件中的所有文件

```shell
# 在 node_modules 目录安装 package.json 中列出的所又依赖
# 安装时，如果 node_modules 中有相应的包则不会重新下载
npm install

# 简写
npm i

# 强制重新下载安装
npm install --force
```

### 运行脚本

`npm run`用来执行在 `package.json` 中 `scripts` 属性下定义的脚本

```json
{ 
	"scripts": {
		"dev": "node app.js",
		"start": "node app.js"
	}
}
```

`npm run start`运行start脚本

### 显示某个包名

```shell
npm info 包名

# 输出json格式
npm info 包名 --json

# 输出README部分
npm info 包名 readme
```

### 列出所有依赖

```shell
npm list
```

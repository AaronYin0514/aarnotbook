---
tags: eletron
---

# Eletron + React环境搭建

## 用React脚手架工具创建项目

创建工程

```shell
npx create-react-app project-name
cd project-name
```

## 安装Electron

最好使用全局模式VPN安装electron。网络原因可能导致安装Electron失败，设置国内Electron镜像；

```shell
npm config set electron_mirror "https://npmmirror.com/mirrors/electron/"
```

如果报错，可以在`~/.npmrc`配置中直接修改

```shell
...
electron_mirro=https://npmmirror.com/mirrors/electron/
...
```

安装Electron

```shell
yarn add electron
```

如果遇到类似报错：`cannot resolve https://npm.taobao.org/mirrors/electron/12.0.15/electron-v12.0.15-darwin-x64.zip: status code 404`

访问`https://npm.taobao.org/mirrors`，进入`electron`目录，进入相应版本目录，选择对应的文件（如：`electron-v12.0.15-darwin-x64.zip`）和`SHASUMS256.txt`文件，下载后，放入下面目录中

```
~/Library/Caches/electron
```

## 配置package.json

```json
...
"main": "main.js",
"scripts": {
	...
	"dev": "electron ."
},
```

## 创建`main.js`

根目录下创建`main.js`

```javascript
const { app, BrowserWindow } = require('electron')

function createWindow() {
	const mainWindow = new BrowserWindow({
		width: 1024,
		height: 680,
		webPreferences: {
			nodeIntegration: true
		}
	})

	mainWindow.loadURL('http://localhost:3000')
}

app.whenReady().then(() => {
	createWindow()

	app.on('activate', function () {
		// On macOS it's common to re-create a window in the app when the
		// dock icon is clicked and there are no other windows open.
		if (BrowserWindow.getAllWindows().length === 0) createWindow()
	})
})

// Quit when all windows are closed, except on macOS. There, it's common
// for applications and their menu bar to stay active until the user quits
// explicitly with Cmd + Q.
app.on('window-all-closed', function () {
	if (process.platform !== 'darwin') app.quit()
})
```

## 运行

运行：`yarn start`启动**React**；`yarn dev`启动**Electron**

```shell
yarn start
yarn dev
```

## electron-is-dev

开发环境是加载本地web url，正式环境打包设置。`electron-is-dev`用来获取`electron`的环境变量

```shell
yarn add electron-is-dev -D
```

```javascript
const { app, BrowserWindow, ipcMain } = require('electron')
const isDev = require('electron-is-dev')

function createWindow() {
	const mainWindow = new BrowserWindow({
		width: 1024,
		height: 680,
		webPreferences: {
			nodeIntegration: true
		}
	})
	
	const urlLocation = isDev ? 'http://localhost:3000' : 'dummyurl'
	mainWindow.loadURL(urlLocation)
}
```

## concurrently

**concurrently**工具可以帮助我们并行运行**React**和**Electron**，安装:

```shell
yarn add concurrently -D
```

配置:

```json
"scripts": {
...
"dev": "concurrently \"electron .\" \" react-scripts start\""
},
```

## wait-on

运行`yarn dev`命令，但是Electron在React加载完成前已经启动，但是我们希望React加载完成后再启动Electron。安装`wait-on`：

```
yarn add wait-on -D
```

配置：

```json
"scripts": {
...
"dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \" react-scripts start\""
},
```

## React启动时不打开浏览器

我们希望React启动时不要自动打开浏览器，需要修改环境变量`BROWSER=none`，但是**cross-env**工具可以帮助我们做到多平台的兼容，安装：

```shell
yarn add cross-env -D
```

配置：

```json
"scripts": {
...
"dev": "concurrently \"wait-on http://localhost:3000 && electron .\" \" cross-env BROWSER=none react-scripts start\""
},
```

## 启动

```shell
yarn dev
```





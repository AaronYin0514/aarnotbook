---
tags: electron
---

# Electron-React打包构建

## Electron打包方式

-   [electron-forge](https://github.com/electron-userland/electron-forge)
-   [electron-builder](https://github.com/electron-userland/electron-builder)
-   [electron-packager](https://github.com/electron/electron-packager)

## Electron-React electron-builder打包流程

### 1 安装electron-builder

```shell
yarn add electron-builder -D
```

### 2 打包React项目

package.json

```json
	...
	"scripts": {
		...
		"build": "react-scripts build"
		...
	},
	"homepage": "./",
	...
```

`homepage`选项使打包后文件使用相对路径，否则使用绝对路径`file://...`导致`electron`加载找不到文件。查看`build/index.html`:

```html
...<script defer="defer" src="./static/js/main.8754d9b5.js">...<link href="./static/css/main.7eb8f3cc.css" rel="stylesheet">...
```

运行`yarn build`，打包后的文件在`build`目录下

修改`electron`加载的url。main.js:

```javascript
const urlLocation = isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, './index.html')}`
mainWindow.loadURL(urlLocation)
```

此处文件路径没有使用`'./build/index.html'`与后面配置`"main": "./build/main.js"`有关

### 3 webpack打包Electron

使用webpack，指定electron要打包的文件，可以更好的控制安装包大小，package.json

```json
...
	"build": {
		"appId": "xxxxxx",
		"productName": "XXXXX",
		"files": [
			"build/**/*",
			"node_modules/**/*",
			"package.json"
		],
		"directories": {
			"buildResources": "assets"
		},
		"extraMetadata": {
			"main": "./build/main.js"
		},
		"extends": null,
		"mac": {
			"category": "public.app-category.productivity",
			"artifactName": "${productName}-${version}-${arch}.${ext}"
		},
		"dmg": {
			"background": "assets/appdmg.png",
			"icon": "assets/icon.png",
			"iconSize": 100,
			"contents": [
				{
					"x": 380,
					"y": 280,
					"type": "link",
					"path": "/Applications"
				},
				{
					"x": 110,
					"y": 280,
					"type": "file"
				}
			],
			"window": {
				"width": 500,
				"height": 500
			}
		},
		"win": {
			"target": [
				"msi",
				"nsis"
			],
			"icon": "assets/icon.png",
			"artifactName": "${productName}-Web-Setup-${version}.${ext}",
			"publisherName": "Viking Zhang"
		},
		"nsis": {
			"allowToChangeInstallationDirectory": true,
			"oneClick": false,
			"perMachine": false
		}
	}
...
```

* **appId**: 安装包id
* **productName**：项目名称
* **files**：指定`electron`打包的文件，需要看下`main.js`依赖了那些文件
* **directories**：资源文件目录
* **extraMetadata**：使用webpack打包后，`main.js`被打包到`build`目录下了
* **extends**：electron-builder默认会把入口文件改为react-cra，需要设置为`null`
* **mac**：mac配置，如`category`表示应用分类
* **dmg**：dmg安装包配置

配置构建脚本，package.json

```javascript
...
	"scripts": {
		...
		"pack": "electron-builder --dir",
		"dist": "electron-builder",
		"release": "electron-builder",
		"prerelease": "react-scripts build && webpack --mode production",
		"prepack": "react-scripts build && webpack --mode production",
		"predist": "react-scripts build && webpack --mode production"
		...
	}
...
```

钩子函数，在脚本名称前加`pre`，运行该脚本前，会先运行pre脚本。

webpack5 `--mode production`是必传参数












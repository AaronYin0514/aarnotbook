---
tags: electron
---

# 上下文隔离

**上下文隔离**确保preload脚本、Electron与webContents中加载的网站运行在不同的上下文中。例如：启用上下文隔离时，在`prelaod.js`中设置`window.hello = 'wave'`，在网站中访问它的值(`window.hello`)是`undefined`。

Electron12默认开启上下文隔离。

## 使用方法

**main.js**

```javascript
const { app, BrowserWindow } = require('electron')
const path = require('path')
...
const mainWindow = new BrowserWindow({
	width: 1280,
	height: 720,
	webPreferences: {
		preload: path.join(app.getAppPath(), 'preload.js')
	}
})
...
```

**preload.js**

```javascript
// preload with contextIsolation enabled  
const { contextBridge } = require('electron')  
  
contextBridge.exposeInMainWorld('myAPI', {  
	doAThing: () => {}  
})
```

**render.js**

```javascript
// use the exposed API in the renderer  
window.myAPI.doAThing()
```

## 关闭上下文隔离

**main.js**

```javascript
const { app, BrowserWindow } = require('electron')
const path = require('path')
...
const mainWindow = new BrowserWindow({
	width: 1280,
	height: 720,
	webPreferences: {
		nodeIntegration: true,
		contextIsolation: false,
	}
})
...
```

**render.js**

```javascript
const shell = window.require('shelljs')
const sqlite3 = window.require('sqlite3').verbose()
```
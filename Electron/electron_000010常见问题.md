---
tags: electron
---
# 常见问题

## 修改React本地服务端口号

`create-react-app`生成的项目，默认端口号是`3000`。`3000`端口是`webpack`配置里面写的，可以通过传递一个`PORT`全局变量，来修改这个端口。

```shell
# npm
npm install cross-env

# yarn
yarn add cross-env
```

修改`package.json`:

```bash
"scripts": {
    "start": "cross-env PORT=5000 react-scripts start",
    //...
}
```

## electron渲染进程导入node报错

例如electron react渲染进程中导入`ipcRender`编译时报错

```javascript
const {ipcRender} = window.require('electron')
```

## electron加载本地资源

创建window时，设置`webPreferences`的`webSecurity`属性为`false`

```javascrit
let win = createWindow({
    width: 920,
    height: 610,
    center: true,
    skipTaskbar: false,
    transparent: false,
    title: 'feng',
    // 加入这个参数即可
    webPreferences: {
        webSecurity: false
    }
})
```


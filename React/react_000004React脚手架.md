---
tags: javascript, js, react
---

# React脚手架

React脚手架可以帮助我们快速创建一个React工程。

## 安装本地环境

```shell
npm install -g create-react-app
```

## 利用脚手架创建hell-react工程

```shell
create-react-app hello-react
cd hello-react
npm start
```

成功启动react后，通过访问[http://127.0.0.1:3000](http://127.0.0.1:3000)就可以访问react工程站点了。

## 工程目录接受

### 1 public文件夹（静态资源文件夹）

|  文件   | 作用  |
|  ----  | ----  |
| favicon.icon  | 网站页签图标 |
| index.html  | 主页面 |
| logo192.png  | logo图 |
| logo512.png  | logo图 |
| manifest.json  | 应用加壳的配置文件 |
| robots.txt  | 爬虫协议文件 |

### 2 src文件夹(源码文件夹)

|  文件   | 作用  |
|  ----  | ----  |
| App.css  | App组件的样式 |
| App.js  | App组件 |
| App.test.js  | 用于给App做测试 |
| index.css  | 样式 |
| index.js  | 入口文件 |
| logo.svg  | logo图 |
| reportWebVitals.js | 页面性能分析文件(需要web-vitals库的支持) |
| setupTests.js | 组件单元测试的文件(需要jest-dom库的支持) |



---
tags:
  - 大模型
---

# Llama 3.1

## Mac部署Llama 3.1

### 安装Ollama

下载安装[ollama](https://ollama.com/)应用程序

### 安装Docker

下载安装[docker](https://www.docker.com/)应用程序

### Open WebUI

安装Web服务，文档地址[open webui](https://docs.openwebui.com/)

运行命令，默认端口3000，可以更改

```shell
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

### 配置

#### 注册管理员账号

访问[http://localhost:3000/](http://localhost:3000/)，第一次需要注册一个账号，第一个注册的账号为管理员，可管理其它账号。登陆好后进行模型配置

#### 模型配置

点击界面左下角用户头像图标选择： 设置 > 管理员设置

进入管理员设置界面，选择模型：模型 > 从 Ollama.com 拉取一个模型

输入模型的名称就可以了，不知道名称，可以点击下面的链接[https://ollama.com/library](https://ollama.com/library)

这里输入**llama3.1**，默认下载8B的模型

### 运行

![](ollama-webui.png)





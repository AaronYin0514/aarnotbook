---
tags: python
---

# 虚拟环境

## 依赖安装

1. 查看是否安装**virtualenv**    

```shell
pip3 freeze
```

如果还没有安装**virtualenv**则安装，或者后面安装**virtualenvwrapper**报错也可能是**virtualenv**安装的问题，重新安装一次即可。

```shell
pip3 install virtualenv
```

2. 安装**virtualenvwrapper**

```shell
pip3 install virtualenvwrapper
```

3. 配置

需要为**virtualenvwrapper**指定工作目录，由于当前环境存在多个python版本，还要为**virtualenvwrapper**指定我要使用的python3。最后调用**virtualenvwrapper**配置脚本。

在`~/.zshrc`文件（选择你使用的shell配置文件）中进行配置。

```shell
export VIRTUALENVWRAPPER_PYTHON=/usr/local/bin/python3
export WORKON_HOME=~/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

查看python3命令执行文件或virtualenvwrapper.sh目录：

```shell
which python3
which virtualenvwrapper.sh
```

4. 执行配置

```shell
source ~/.zshrc
```


## 创建虚拟环境

```shell
mkvirtualenv [虚拟环境名称]
```

## 删除

```shell
rmvirtualenv [虚拟环境名称]
```

## 进入虚拟环境

```shell
workon [虚拟环境名称]
```

## 退出虚拟环境

```shell
deactivate
```

## 查看所有虚拟环境

```shell
workon [两次tab键]
```

## 查看虚拟环境中已安装的包

```shell
pip list
pip freeze
```
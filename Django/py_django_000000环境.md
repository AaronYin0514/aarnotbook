---
tags: python django
---

# Django环境

## 创建虚拟环境

[[py_cc_02000虚拟环境virtualenv|安装虚拟环境依赖]]

1. 创建虚拟环境

```shell
mkvirtualenv [虚拟环境名称]
```

2. 进入虚拟环境

```shell
workon [虚拟环境名称]
```

## 安装Django

```shell
pip3 install django
```

## 创建Django项目

```shell
django-admin startproject [项目名称]
```

## Django项目目录说明

- **manage.py**：一个命令行工具，可以使你用多种方式对Django项目进行交互
- 内层的目录：项目的真正的Python包
- **\__init__.py**：一个空文件，它告诉Python这个目录应该被看做一个Python包
- **settings.py**：项目的配置
- **urls.py**：项目的URL声明
- **wsgi.py**：项目与WSGI兼容的Web服务器入

## 数据库配置

在settings.py文件中，通过**DATABASES**项进行数据库设置。Django支持sqlitt、mysql等主流数据库，默认使用sqlite。

```python
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.sqlite3',
		'NAME': BASE_DIR / 'db.sqlite3',
	}
}
```

## 编码和时区配置

在settings.py文件中，通过**LANGUAGE_CODE**项进行编码设置。默认`en-us`。中文使用：

```python
LANGUAGE_CODE = 'zh-Hans'
```

在settings.py文件中，通过**TIME_ZONE**项进行编码设置。默认`UTC`。

```python
TIME_ZONE = 'Asia/Shanghai'
```

## 启动服务器

```shell
python3 manage.py runserver ip:port
```

- ip非必填，默认端口8000
- 这是一个python编写的轻量级web服务器，供开发使用


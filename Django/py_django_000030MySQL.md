---
tags: python django mysql
---

# Python3 + Django + MySQL

## 安装MySQL模块

```shell
pip3 install pymysql
```

## 创建数据库

```shell
create databases [数据库名称] charset=utf8
```

## 配置Mysql

在settings.py文件中，通过DATABASES项进行数据库设置

```python
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.mysql',
		'NAME': '数据库名称',
		'USER':'Mysql用户名',
		'PASSWORD':'Mysql密码',
		'HOST':'localhost',
		'PORT':'3306',
	}
}
```

在`__init__.py`文件中添加：

```python
import pymysql

pymysql.install_as_MySQLdb()
```




---
tags: python django
---

# Django入门

## 流程

1. [[py_django_000000环境#创建虚拟环境|创建虚拟环境]]
2. [[py_django_000000环境#安装Django|安装Django开发环境]]
3. [[py_django_000000环境#创建Django项目|创建项目]]
4. 配置项目：数据库、编码、时区和管理站点等
5. [[py_django_000010Django入门#创建应用|创建应用]]
6. 创建模型
7. 模型迁移：激活模型->生成迁移文件->执行迁移文件
8. 创建视图
9. 创建模型

## 创建应用

Django中每个应用对应一个业务处理

```shell
python3 manage.py startapp [应用名称]
```

例如创建应用booktest：

```python
python3 manage.py booktest
```

## 创建模型

在对应应用的models.py文件中创建模型。定义类属性（可指定数据类型、约束等）用于生成表结构。例如：

```python
from django.db import models

# Create your models here.

class BookInfo(models.Model):
	btitle=models.CharField(max_length=20)
	bpub_date=models.DateField()

class HeroInfo(models.Model):
	hname=models.CharField(max_length=10)
	hgender=models.BooleanField()
	hcontent=models.CharField(max_length=1000)
	hbook=models.ForeignKey(BookInfo, on_delete=models.CASCADE)
```

## 激活模型

在setting.py文件添加应用，例如添加booktest

```python
INSTALLED_APPS = [
	'django.contrib.admin',
	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
	'booktest'
]
```

## 生成迁移文件

```shell
python3 manage.py makemigrations
```

## 执行迁移文件

```shell
python3 manage.py migrate
```

## 管理后台

[[py_django_000020管理|Django内置管理后台]]

## 视图

在django中，视图对WEB请求进行回应。视图接收reqeust对象作为第一个参数，包含了请求的信息。视图就是一个Python函数，被定义在views.py中：

```python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
	return HttpResponse('index')
```

## URLconf

在Django中，定义URLconf包括正则表达式、视图两部分。Django使用正则表达式匹配请求的URL，一旦匹配成功，则调用应用的视图。

在urls.py中添加路由：

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
	path('admin/', admin.site.urls),
	path('booktest/', include('booktest.urls'))
]
```

`include`函数引入其它URLconf，在booktest目录下创建urls.py文件：

```python
from django.urls import path
from . import views

urlpatterns=[
	path('', views.index),
]
```

访问地址：`http://localhost:8000/booktest`

## 模版

### 配置目录

项目根目录下创建templates目录，在templates目录下创建booktest目录，并创建index.html文件。修改settings.py文件

```python
'DIRS': [os.path.join(BASE_DIR, 'templates')],
```

### views中返回对应路由模版

```python
from django.shortcuts import render
from .models import BookInfo
# Create your views here.

def index(request):
	booklist = BookInfo.objects.all()
	return render(request, 'booktest/index.html', {'booklist': booklist})
```

### 修改模版

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<body>
	<h1>图书列表</h1>
	<ul>
		{%for book in booklist%}
			<li>
			{{book.btitle}}
			</li>
		{%endfor%}
	</ul>
</body>
</html>
```







---
tags: python django
---

# 管理

Django框架内置了管理平台，根据模型自动生成管理模块。

## 添加管理员

根据提示输入用户名、邮箱和密码。

```shell
python3 manage.py createsuperuser
```

## 启动服务器

[[py_django_000000环境#启动服务器|启动服务器]]

## 访问管理站点

```
http://localhost:8000/admin
```

## 注册模型

在对应应用文件夹下**admin.py**文件中，添加如下代码：

```python
from django.contrib import admin
from .models import *

admin.site.register(ModelName)
```

刷新页面，可对指定模型进行增删改查的操作。

## 自定义管理页面

Django提供了admin.ModelAdmin类。通过定义ModelAdmin的子类，来定义模型在Admin界面的显示方式。

```python
class QuestionAdmin(admin.ModelAdmin):
    ...
admin.site.register(Question, QuestionAdmin)
```

### 列表页属性

- list_display：显示字段，可以点击列头进行排序

```
list_display = ['pk', 'btitle', 'bpub_date']
```

- list_filter：过滤字段，过滤框会出现在右侧

```
list_filter = ['btitle']
```

- search_fields：搜索字段，搜索框会出现在上侧

```
search_fields = ['btitle']
```

- list_per_page：分页，分页框会出现在下侧

```
list_per_page = 10
```

### 添加、修改页属性

- fields：属性的先后顺序

```
fields = ['bpub_date', 'btitle']
```

- fieldsets：属性分组

```
fieldsets = [
    ('basic',{'fields': ['btitle']}),
    ('more', {'fields': ['bpub_date']}),
]
```

### 关联对象

例如A与B是一对多关系：

```python
class ModelA(models.Model):
	pass

class ModelB(models.Model):
	ha = models.ForeignKey(ModelA, on_delete=models.CASCADE)
```

创建A时

```python
from django.contrib import admin
from models import BookInfo,HeroInfo

class ModelAInline(admin.StackedInline):
    model = ModelB
    extra = 2

class ModelBAdmin(admin.ModelAdmin):
    inlines = [ModelAInline]

admin.site.register(ModelB, ModelBAdmin)
```

可以将内嵌的方式改为表格

```
class ModelAInline(admin.TabularInline)
```


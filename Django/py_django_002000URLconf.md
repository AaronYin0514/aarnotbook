# URLconf

URLconf用来指定URL与视图的映射关系。

## 配置URLconf

### 配置根URLconf

在settings.py文件中通过**ROOT_URLCONF**指定根级URLconf配置文件路径

```python
ROOT_URLCONF = 'test1.urls'
```

在URLconf配置文件中定义urlpatterns，urlpatterns是存放`django.urls.path()`或`django.urls.re_path()`实例的列表。

```python
urlpatterns = [
	path('admin/', admin.site.urls),
]
```

### 配置模块URLconf



```python
from django.urls import include, path

urlpatterns = [
    # ... snip ...
    path('community/', include('aggregator.urls')),
    path('contact/', include('contact.urls')),
    # ... snip ...
]
```

## 匹配URL

### path

### 正则

## URL反向解析



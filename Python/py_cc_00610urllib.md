---
tags: python
---

# urllib

## 基本使用

`urlopen`发送HTTP请求，并返回服务器响应。

`read`读取服务器响应内容。

```python
import urllib.request

url = 'http://www.baidu.com'
response = urllib.request.urlopen(url)

html = response.read()
print(html)
```

## Request

## 创建Request

```
urllib.request.Request(_url_, _data=None_, _headers={}_, _origin_req_host=None_, _unverifiable=False_, _method=None_)
```

- 第1个参数：URL
- data：请求参数
- headers: 请求头
- method：请求方法（GET、POST、PUT、DELETE、MATCH、HEAD），如果data是None，method默认是GET，其它默认POST
- origin_req_host和unverifiable：与cookies相关

### URL编码/解码

#### 编码

`urllib.parse.urlencode`函数可以将字典或二元元组的序列转换成**application/x-www-form-urlencoded**格式的百分号编码ASCII字符串。

```python
params1 = urllib.parse.urlencode({'key1': 'value1', 'key2': '中文value'})
print(params1) # key1=value1&key2=%E4%B8%AD%E6%96%87value

params2 = urllib.parse.urlencode([("key1", "value1"), ("key2", "中文value")])
print(params2) # key1=value1&key2=%E4%B8%AD%E6%96%87value
```

如果是**POST**请求，需要将字符串转换为bytes数据。

```python
data = urllib.parse.urlencode({'key1': 'value1', 'key2': '中文value'})
data = data.encode('ascii')
print(data) # b'key1=value1&key2=%E4%B8%AD%E6%96%87value'
```

#### 解码

`urllib.parse.unquot`用于解码字符串

```python
value = urllib.parse.unquote("key1=value1&key2=%E4%B8%AD%E6%96%87value")
print(value) # key1=value1&key2=中文value
```

## JSON编码

JSON是网络请求中常用的编码格式。

```python
import json

data = json.dumps({
	'name': '测试地理围栏',
	'center': '115.672126,38.817129',
	'radius': '1000',
	'repeat': 'Mon,Sat'
})

data = data.encode('utf-8')
print(data) # b'{"name": "\\u6d4b\\u8bd5\\u5730\\u7406\\u56f4\\u680f", "center": "115.672126,38.817129", "radius": "1000", "repeat": "Mon,Sat"}'
```

### GET请求

```python
import urllib.request
import urllib.parse

url = "https://restapi.amap.com/v3/geocode/geo"
params = urllib.parse.urlencode({
	'key': 'bb539ea8247bcef91f0467ff1ac51e23',
	'address': '北京市朝阳区阜通东大街6号',
})

url = "%s?%s" % (url, params)
request = urllib.request.Request(url)

response = urllib.request.urlopen(request)
res = response.read().decode('utf-8')
print(res)
```

### POST请求

data不为`None`时，`method`默认为`POST`。

```python
import urllib.request
import json

url = "https://restapi.amap.com/v4/geofence/meta?key=bb539ea8247bcef91f0467ff1ac51e23"

data = json.dumps({
'name': '测试地理围栏',
'center': '115.672126,38.817129',
'radius': '1000',
'repeat': 'Mon,Sat'
})
data = data.encode('utf-8')

headers = {'Content-Type': 'application/json'}

request = urllib.request.Request(url, data=data, headers=headers)

response = urllib.request.urlopen(request)
res = response.read().decode('utf-8')
print(res)
```

## Response



## HTTPError

HTTPError是URLError的子类，我们发出一个请求时，服务器上都会对应一个response应答对象，其中它包含一个数字"响应状态码"。

如果`urlopen或opener.open`不能处理的，会产生一个HTTPError，对应相应的状态码，HTTP状态码表示HTTP协议所返回的响应的状态。

**注意，urllib可以为我们处理重定向的页面（也就是3开头的响应码），100-299范围的号码表示成功，所以我们只能看到400-599的错误号码。**

```python
requset = urllib.request.Request('http://www.baidu.com/itcast')
try:
	urllib.request.urlopen(requset)
except urllib.request.HTTPError as err:
	print('HTTPError - ', err.code)
except urllib.request.URLError as err:
	print('URLError - ', err)
else:
	print("Success")
```

结果：

```
HTTPError -  404
```
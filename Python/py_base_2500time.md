---
tags: python time date
---

# time

## 获取时间戳

```python
import time

t = time.time()
print(t) # 1678957251.1854851
```

## `localtime`和`gmtime`

```python
localtime([secs])
gmtime([secs])
```

参数：时间戳，默认time()

```python
import time

t1 = time.localtime()
t2 = time.gmtime()
t3 = time.localtime(34.54)

print(t1) # time.struct_time(tm_year=2023, tm_mon=3, tm_mday=16, tm_hour=17, tm_min=3, tm_sec=47, tm_wday=3, tm_yday=75, tm_isdst=0)
print(t2) [[time]].struct_time(tm_year=2023, tm_mon=3, tm_mday=16, tm_hour=9, tm_min=3, tm_sec=47, tm_wday=3, tm_yday=75, tm_isdst=0)
print(t3) # time.struct_time(tm_year=1970, tm_mon=1, tm_mday=1, tm_hour=8, tm_min=0, tm_sec=34, tm_wday=3, tm_yday=1, tm_isdst=0)
```

`localtime`和`gmtime`返回`struct_time`格式时间数据

| 元素     | 含义         | 取值                               |
| -------- | ------------ | ---------------------------------- |
| tm_year  | 年           | 4位数字，如2022                    |
| tm_mon   | 月           | 1~12，如2                          |
| tm_mday  | 日           | 1~31，如5                          |
| tm_hour  | 时           | 0~23，如7                          |
| tm_min   | 分           | 0~59，如50                         |
| tm_sec   | 秒           | 0~61（60或61是闰秒）               |
| tm_wday  | 一周的第几日 | 0~6（0为周一，依此类推）           |
| tm_yday  | 一年的第几日 | 1~366(366为儒略历）                |
| tm_isdst | 夏时令       | 1：是夏令时 0：非夏令时 -1：不确定 |

## 格式化

### `strftime`

```python
strftime(tpl,ts)
```

参数：

- tpl：格式字符串
- ts：struct_time格式数据

```python
import time

t = time.gmtime()
str = time.strftime("%Y-%m-%d %H:%M:%S", t)

print(str) # 2023-03-16 09:17:17
```

### `strptime`

```python
strptime(str,tpl)
```

参数：

- str：字符串
- tpl：格式

```python
import time

t = time.strptime("2018-1-26 12:55:20", '%Y-%m-%d %H:%M:%S')
print(t) # time.struct_time(tm_year=2018, tm_mon=1, tm_mday=26, tm_hour=12, tm_min=55, tm_sec=20, tm_wday=4, tm_yday=26, tm_isdst=-1)
```

### 格式

| 时间格式控制符 | 说明                                         |
| -------------- | -------------------------------------------- |
| %Y             | 四位数的年份，取值范围为0001~9999,如1900     |
| %m             | 月份（01~12），例如10                        |
| %d             | 月中的一天（01~31）例如：25                  |
| %B             | 本地完整的月份名称，比如January              |
| %b             | 本地简化的月份名称，比如Jan                  |
| %a             | 本地简化的周日期，Mon~Sun,例如Wed            |
| %A             | 本地完整周日期，”Monday~Sunday,例如Wednesday |
| %H             | 24小时制小时数（00~23),例如：12              |
| %l             | 12小时制小时数（01~12），例如：7             |
| %p             | 上下午，取值为AM或PM                         |
| %M             | 分钟数（00~59），例如26                      |
| %S             | 秒（00~59），例如26                          |

## 转时间戳

### `mktime`

`struct_time`转时间戳

```python
import time

t = (2019, 2, 15, 10, 13, 38, 1, 48, 0)
d = time.mktime(t)

print ("time.mktime(t) : %f" % d)
print ("asctime(localtime(secs)): %s" % time.asctime(time.localtime(d)))
```









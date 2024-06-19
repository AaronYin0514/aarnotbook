---
tags: python django
---

#  模型

## 定义模型

在模型中定义类属性，会生成表中的字段。并指定类属性的数据类型、约束和限制。

**Django**会为表增加自动增长的主键列，也可自定义。

属性名不能使用Python保留关键字，不允许使用连续下划线。

## 定义属性

导入from django.db import models,通过models.Field创建字段类型的对象，赋值给属性

**对于重要数据都做逻辑删除，不做物理删除，实现方法是定义isDelete属性，类型为BooleanField，默认值为False**

| 字段类型         | 说明                                                                                                                                                                                                                                                                                                                        |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AutoField        | 一个根据实际ID自动增长的IntegerField，通常不指定。如果不指定，一个主键字段将自动添加到模型中                                                                                                                                                                                                                                |
| BooleanField     | true/false                                                                                                                                                                                                                                                                                                                  |
| NullBooleanField | 支持null、true、false三种值                                                                                                                                                                                                                                                                                                 |
| CharField        | 字符串，max_length最大长度                                                                                                                                                                                                                                                                                                  |
| TextField        | 大文本字段，一般超过4000使用                                                                                                                                                                                                                                                                                                |
| IntegerField     | 整数                                                                                                                                                                                                                                                                                                                        |
| DecimalField     | 十进制浮点数。max_digits：有效位总数。decimal_places：小数点后位数。                                                                                                                                                                                                                                                        |
| FloatField       | 用Python的float实例来表示的浮点数                                                                                                                                                                                                                                                                                           |
| DateField        | 使用Python的datetime.date实例表示的日期。auto_now：每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为false。auto_now_add：当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为false。auto_now_add, auto_now, and default 这些设置是相互排斥的。 |
| TimeField        | 使用Python的datetime.time实例表示的时间，参数同DateField                                                                                                                                                                                                                                                                    |
| DateTimeField    | 使用Python的datetime.datetime实例表示的日期和时间，参数同DateField                                                                                                                                                                                                                                                          |
| FileField        | 一个上传文件的字段                                                                                                                                                                                                                                                                                                          |
| ImageField       | 继承了FileField的所有属性和方法，但对上传的对象进行校验，确保它是个有效的image                                                                                                                                                                                                                                              |
                                                                                                                                                                                                                                                                                                           
## 字段选项

| 选项        | 说明                                                           |
| ----------- | -------------------------------------------------------------- |
| null        | 如果为True，Django 将空值以NULL 存储到数据库中，默认值是 False |
| blank       | 如果为True，则该字段允许为空白，默认值是 False                 |
| db_column   | 字段的名称，如果未指定，则使用属性的名称                       |
| db_index    | 若值为 True, 则在表中会为此字段创建索引                        |
| default     | 默认值                                                         |
| primary_key | 若为 True, 则该字段会成为模型的主键字段                        |
| unique      | 如果为 True, 这个字段在表中必须有唯一值                        |

## 关系

| 选项            | 说明                           |
| --------------- | ------------------------------ |
| ForeignKey      | 一对多，将字段定义在多的端中   |
| ManyToManyField | 多对多，将字段定义在两端中     |
| OneToOneField   | 一对一，将字段定义在任意一端中 |

维护递归的关联关系，使用'self'指定。

- 用一访问多：对象.模型类小写_set

```python
bookinfo.heroinfo_set
```

- 用一访问一：对象.模型类小写

```python
heroinfo.bookinfo
```

- 访问id：对象.属性_id

```python
heroinfo.book_id
```

## 元选项

-   在模型类中定义类Meta，用于设置元信息
-   元信息db_table：定义数据表名称，推荐使用小写字母，数据表的默认名称

```python
<app_name>_<model_name>
```

- ordering：对象的默认排序字段，获取对象的列表时使用，接收属性构成的列表

```python
class BookInfo(models.Model):
    ...
    class Meta():
        ordering = ['id']
```

- 字符串前加-表示倒序，不加-表示正序

```python
class BookInfo(models.Model):
    ...
    class Meta():
        ordering = ['-id']
```

- 排序会增加数据库的开销





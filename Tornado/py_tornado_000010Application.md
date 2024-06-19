---
tags: python, tornado
---

# Applicaiton

## settings

前面的学习中，我们在创建**tornado.web.Application**的对象时，传入了第一个参数——路由映射列表。实际上Application类的构造函数还接收很多关于tornado web应用的配置参数，在后面的学习中我们用到的地方会为大家介绍。

## debug

**debug**设置tornado是否工作在调试模式，默认为False即工作在生产模式。当设置`debug=True`后，tornado会工作在调试/开发模式，在此种模式下，tornado为方便我们开发而提供了几种特性：

- **自动重启**，tornado应用会监控我们的源代码文件，当有改动保存后便会重启程序，这可以减少我们手动重启程序的次数。需要注意的是，一旦我们保存的更改有错误，自动重启会导致程序报错而退出，从而需要我们保存修正错误后手动启动程序。这一特性也可单独通过`autoreload=True`设置；
- **取消缓存编译的模板**，可以单独通过`compiled_template_cache=False`来设置；
- **取消缓存静态文件hash值**，可以单独通过`static_hash_cache=False`来设置；
- **提供追踪信息**，当**RequestHandler**或者其子类抛出一个异常而未被捕获后，会生成一个包含追踪信息的页面，可以单独通过`serve_traceback=True`来设置。

使用debug参数的方法：

```python
import tornado.web
app = tornado.web.Application([], debug=True)
```
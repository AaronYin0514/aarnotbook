# sklearn.datasets常用功能详解

Sklearn作为Python中经典的机器学习模块，该模块围绕着机器学习提供了很多可直接调用的机器学习算法以及很多经典的数据集，本文就对sklearn中专门用来得到已有或自定义数据集的datasets模块进行详细介绍（datasets中的数据集分为很多种，本文介绍几类常用的数据集生成方法，本文总结的所有内容你都可以在sklearn的官网：[API Reference](https://link.zhihu.com/?target=http%3A//scikit-learn.org/stable/modules/classes.html%23module-sklearn.datasets)中找到对应的更加详细的英文版解释。）

### **1. 介绍了几种datasets自带的经典数据集**

**Demo 1：波士顿房价数据（适用于回归任务）**

这个数据集包含了506处波士顿不同地理位置的房产的房价数据（因变量），和与之对应的包含房屋以及房屋周围的详细信息（自量），其中包含城镇犯罪率、一氧化氮浓度、住宅平均房间数、到中心区域的加权距离以及自住房平均房价等13个维度的数据，因此，波士顿房价数据集能够应用到回归问题上，这里使用load_boston(return_X_y=False)方法来导出数据，其中参数return_X_y控制输出数据的结构，若选为True，则将因变量和自变量独立导出；

```text
from sklearn import datasets
'''清空sklearn环境下所有数据'''
datasets.clear_data_home()
'''载入波士顿房价数据'''
X,y = datasets.load_boston(return_X_y=True)
'''获取自变量数据的形状'''
print(X.shape)
'''获取因变量数据的形状'''
print(y.shape)
```

![](https://pic2.zhimg.com/80/v2-a271cf38c647c5f4021e345cb8105bf5_720w.jpg)

自变量X：

![](https://pic2.zhimg.com/80/v2-f4e303b9461fc4bb56916b7575cbb995_720w.webp)

因变量y：

![](https://pic3.zhimg.com/80/v2-ef58ac2d2806f54652a415a7967f1baa_720w.webp)

**Demo 2：威斯康辛州乳腺癌数据（适用于分类问题）**

这个数据集包含了威斯康辛州记录的569个病人的乳腺癌恶性/良性（1/0）类别型数据（训练目标），以及与之对应的30个维度的生理指标数据；因此这是个非常标准的二类判别数据集，在这里使用load_breast_cancer(return_X_y)来导出数据：

```text
from sklearn import datasets
'''载入威斯康辛州乳腺癌数据'''
X,y = datasets.load_breast_cancer(return_X_y=True)
'''获取自变量数据的形状'''
print(X.shape)
'''获取因变量数据的形状'''
print(y.shape)
```

![](https://pic2.zhimg.com/80/v2-a73da76169a6506d68ad8d1a0aa4cec9_720w.webp)

自变量X：

![](https://pic1.zhimg.com/80/v2-9ab396aa52f66d5ce4a3b24465cc0dc8_720w.webp)

因变量y：

![](https://pic2.zhimg.com/80/v2-d5dc082423d4801effb69a8d3869b6f5_720w.webp)

![](https://pic2.zhimg.com/80/v2-d5dc082423d4801effb69a8d3869b6f5_720w.webp)

**Demo 3：糖尿病数据（适用于回归任务）**

这是一个糖尿病的数据集，主要包括442行数据，10个属性值，分别是：Age(年龄)、性别(Sex)、Body mass index(体质指数)、Average Blood Pressure(平均血压)、S1~S6一年后疾病级数指标。Target为一年后患疾病的定量指标，因此适合与回归任务；这里使用load_diabetes(return_X_y)来导出数据：

```text
from sklearn import datasets
'''载入糖尿病数据'''
X,y = datasets.load_diabetes(return_X_y=True)
'''获取自变量数据的形状'''
print(X.shape)
'''获取因变量数据的形状'''
print(y.shape)
```

![](https://pic1.zhimg.com/80/v2-316a5581d41041ef7752f0ea319b0774_720w.webp)

自变量X：

![](https://pic2.zhimg.com/80/v2-7b919911ca8f6a383d5c8dcc8f74b259_720w.webp)

因变量y：

![](https://pic3.zhimg.com/80/v2-3274d5bf7e74c102c561560e4f49865a_720w.webp)

**Demo 4 手写数字数据集（适用于分类任务）**

这个数据集是结构化数据的经典数据，共有1797个样本，每个样本有64的元素，对应到一个8x8像素点组成的矩阵，每一个值是其灰度值，我们都知道图片在计算机的底层实际是矩阵，每个位置对应一个像素点，有二值图，灰度图，1600万色图等类型，在这个样本中对应的是灰度图，控制每一个像素的黑白浓淡，所以每个样本还原到矩阵后代表一个手写体数字，这与我们之前接触的数据有很大区别；在这里我们使用load_digits(return_X_y)来导出数据：

```text
from sklearn import datasets
'''载入手写数字数据'''
data,target = datasets.load_digits(return_X_y=True)
print(data.shape)
print(target.shape)
```

![](https://pic2.zhimg.com/80/v2-70acb79a8502e27778775738904b9ad5_720w.webp)

这里我们利用matshow()来绘制这种矩阵形式的数据示意图：

```text
import matplotlib.pyplot as plt
import numpy as np
 
'''绘制数字0'''
num = np.array(data[0]).reshape((8,8))
plt.matshow(num)
print(target[0])
 
'''绘制数字5'''
num = np.array(data[15]).reshape((8,8))
plt.matshow(num)
print(target[15])
 
'''绘制数字9'''
num = np.array(data[9]).reshape((8,8))
plt.matshow(num)
print(target[9])
```

![](https://pic4.zhimg.com/80/v2-e53a3ec79a0b754b94a139def7c72897_720w.webp)

**Demo 5 Fisher的鸢尾花数据（适用于分类问题）**

著名的统计学家Fisher在研究判别分析问题时收集了关于鸢尾花的一些数据，这是个非常经典的数据集，datasets中自然也带有这个数据集；这个数据集包含了150个鸢尾花样本，对应3种鸢尾花，各50个样本（target），以及它们各自对应的4种关于花外形的数据（自变量）；这里我们使用`load_iris(return_X_y)`来导出数据：

```python
from sklearn import datasets

'''载入Fisher的鸢尾花数据'''
data,target = datasets.load_iris(return_X_y=True)

'''显示自变量的形状'''
print(data.shape)

'''显示训练目标的形状'''
print(target.shape)
```

![](https://pic2.zhimg.com/80/v2-1cd796d7c6a1a2a07c44e7d06ce6b9e1_720w.webp)

自变量：

![](https://pic2.zhimg.com/80/v2-43b2caa6c95f2e1b6015c7f9a8db9ce1_720w.webp)

训练目标：

![](https://pic1.zhimg.com/80/v2-96d1705a8850da0bbf34f5c0154cf958_720w.webp)

**Demo 6 红酒数据（适用于分类问题）**

这是一个共178个样本，代表了红酒的三个档次（分别有59,71,48个样本），以及与之对应的13维的属性数据，非常适合用来练习各种分类算法；在这里我们使用load_wine(return_X_y)来导出数据：

```text
from sklearn import datasets
'''载入wine数据'''
data,target = datasets.load_wine(return_X_y=True)
'''显示自变量的形状'''
print(data.shape)
'''显示训练目标的形状'''
print(target.shape)
```

![](https://pic4.zhimg.com/80/v2-db52eeb3010be9be2f7250c73cf6d287_720w.webp)

### **2 自定义数据集**

前面我们介绍了几种datasets自带的经典数据集，但有些时候我们需要自定义生成服从某些分布或者某些形状的数据集，而datasets中就提供了这样的一些方法：

**2.1 产生服从正态分布的聚类用数据**

datasets.make_blobs(n_samples=100, n_features=2, centers=3, cluster_std=1.0, center_box=(-10.0, 10.0), shuffle=True, random_state=None)，

其中:

n_samples：控制随机样本点的个数

n_features：控制产生样本点的维度（对应n维正态分布）

centers：控制产生的聚类簇的个数：

```text
from sklearn import datasets
import matplotlib.pyplot as plt
X,y = datasets.make_blobs(n_samples=1000, n_features=2, centers=4, cluster_std=1.0, center_box=(-10.0, 10.0), shuffle=True, random_state=None)
plt.scatter(X[:,0],X[:,1],c=y,s=8)
```

![](https://pic3.zhimg.com/80/v2-31471812b676c56f73e76d2d2834a702_720w.webp)

**2.2 产生同心圆样本点**

datasets.make_circles(n_samples=100, shuffle=True, noise=0.04, random_state=None, factor=0.8)

n_samples：控制样本点总数

noise：控制属于同一个圈的样本点附加的漂移程度

factor：控制内外圈的接近程度，越大越接近，上限为1

```text
from sklearn import datasets
import matplotlib.pyplot as plt
X,y = datasets.make_circles(n_samples=10000, shuffle=True, noise=0.04, random_state=None, factor=0.8)
plt.scatter(X[:,0],X[:,1],c=y,s=8)
```

![](https://pic4.zhimg.com/80/v2-b03dc6c7a3cc8793e6f339143ba973c7_720w.webp)

**2.3 生成模拟分类数据集**

datasets.make_classification(n_samples=100, n_features=20, n_informative=2, n_redundant=2, n_repeated=0, n_classes=2, n_clusters_per_class=2, weights=None, flip_y=0.01, class_sep=1.0, hypercube=True, shift=0.0, scale=1.0, shuffle=True, random_state=None)

n_samples：控制生成的样本点的个数

n_features：控制与类别有关的自变量的维数

n_classes：控制生成的分类数据类别的数量

```text
from sklearn import datasets
X,y = datasets.make_classification(n_samples=100, n_features=20, n_informative=2, n_redundant=2, n_repeated=0, n_classes=2, n_clusters_per_class=2, weights=None, flip_y=0.01, class_sep=1.0, hypercube=True, shift=0.0, scale=1.0, shuffle=True, random_state=None)
print(X.shape)
print(y.shape)
set(y)
```

![](https://pic3.zhimg.com/80/v2-687ca629dfa37d346f157746ee0ae326_720w.webp)

**2.4 生成太极型非凸集样本点**

datasets.make_moons(n_samples,shuffle,noise,random_state)

```text
from sklearn import datasets
import matplotlib.pyplot as plt
X,y = datasets.make_moons(n_samples=1000, shuffle=True, noise=0.05, random_state=None)
plt.scatter(X[:,0],X[:,1],c=y,s=8)
```

![](https://pic1.zhimg.com/80/v2-cd4cd1a67b8f6937702643cc06588908_720w.webp)

**参考：**sklearn.datasets常用功能详解_不二的博客-CSDN博客_sklearn的datasets




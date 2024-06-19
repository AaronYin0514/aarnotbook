# matplotlib散点图

## scatter函数

```python
scatter(x, y, s=None, c=None, marker=None, cmap=None, norm=None, vmin=None, vmax=None, alpha=None, linewidths=None, verts=None, edgecolors=None, hold=None, data=None, **kwargs)
```

- x,y：x轴 和 y 轴对应的数据。
- s：散点标记面积，以平方磅为单位的标记面积。标量或数组。
- c：颜色
- marker：散点标记类型，默认为圆圈。
- aplha：透明度
- label：标题

## 基本使用

```python
import numpy as np
import matplotlib.pyplot as plt

x = [0,2,4,6,8,10]
y1 = np.random.randn(len(x))
y2 = np.random.randn(len(x))

# 绘制散点图
plt.scatter(x, y1, label='Image1', c='red')
plt.scatter(x, y2, label='Image2', c='green')

plt.legend(loc="upper right")
plt.show()
```

![](Figure_8.png)

## 设置散点标记大小样式

```python
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(8,6))

mu_vec1 = np.array([0,0])
cov_mat1 = np.array([[1,0],[0,1]])
X = np.random.multivariate_normal(mu_vec1,cov_mat1,500)
R = X**2
R_sum = R.sum(axis=1)

# 绘制散点图
# color：散点颜色
# marker：散点样式
# s：散点大小
# alpha：透明度
plt.scatter(X[:,0], X[:,1], color='green', marker='o',  s=32.*R_sum, edgecolor='black', alpha=0.5)

plt.show()
```

![](Figure_9.png)

## 设置散点形状

```python
from matplotlib import pyplot as plt
import numpy as np

mu_vecl = np.array([0, 0])
cov_matl = np.array([[2,0],[0,2]])
x1_samples = np.random.multivariate_normal(mu_vecl, cov_matl,100)
x2_samples = np.random.multivariate_normal(mu_vecl+0.2, cov_matl +0.2, 100)
x3_samples = np.random.multivariate_normal(mu_vecl+0.4, cov_matl +0.4, 100)

plt.figure(figsize = (8, 6))

# marker：设置散点形状
plt.scatter(x1_samples[:,0], x1_samples[:, 1], marker='x', color = 'blue', alpha=0.7, label = 'x1 samples')
plt.scatter(x2_samples[:,0], x1_samples[:,1], marker='o', color ='green', alpha=0.7, label = 'x2 samples')
plt.scatter(x3_samples[:,0], x1_samples[:,1], marker='^', color ='red', alpha=0.7, label = 'x3 samples')

plt.title('Basic scatter plot')
plt.ylabel('variable X')
plt.xlabel('Variable Y')
plt.legend(loc = 'upper right')

plt.show()
```

![](Figure_10.png)

## 设置散点标记大小

```python
import numpy as np
import matplotlib.pyplot as plt

x1 = np.random.randn(20)
x2 = np.random.randn(20)
plt.figure(1)

# 两种方式设置大小
# blue circle with size 20
plt.plot(x1, 'bo', markersize=20)  
# ms是markersize的别名
plt.plot(x2, 'ro', ms=10)  

plt.show()
```

![](Figure_11.png)

## 带标签点绘制

```python
import matplotlib.pyplot as plt

x_coords = [0.13, 0.22, 0.39, 0.59, 0.68, 0.74,0.93]
y_coords = [0.75, 0.34, 0.44, 0.52, 0.80, 0.25,0.55]

fig = plt.figure(figsize = (8,5))

plt.scatter(x_coords, y_coords, marker = 's', s = 50)

for x, y in zip(x_coords, y_coords):
    plt.annotate('(%s,%s)'%(x,y), xy=(x,y), xytext = (0, -10), textcoords = 'offset points', ha = 'center', va = 'top')

plt.xlim([0,1])
plt.ylim([0,1])
plt.show()
```

![](Figure_12.png)

## 用曲线把样本分成两类

```python
# 2-category classfication with random 2D-sample data
# from a multivariate normal distribution

import numpy as np
from matplotlib import pyplot as plt

def decision_boundary(x_1):
    """Calculates the x_2 value for plotting the decision boundary."""
    return 4 - np.sqrt(-x_1**2 + 4*x_1 + 6 + np.log(16))

# Generating a gaussion dataset:
# creating random vectors from the multivariate normal distribution
# given mean and covariance

mu_vec1 = np.array([0,0])
cov_mat1 = np.array([[2,0],[0,2]])
x1_samples = np.random.multivariate_normal(mu_vec1, cov_mat1,100)
mu_vec1 = mu_vec1.reshape(1,2).T # TO 1-COL VECTOR

mu_vec2 = np.array([1,2])
cov_mat2 = np.array([[1,0],[0,1]])
x2_samples = np.random.multivariate_normal(mu_vec2, cov_mat2, 100)
mu_vec2 = mu_vec2.reshape(1,2).T # to 2-col vector

# Main scatter plot and plot annotation

f, ax = plt.subplots(figsize = (7, 7))
ax.scatter(x1_samples[:, 0], x1_samples[:,1], marker = 'o',color = 'green', s=40)
ax.scatter(x2_samples[:, 0], x2_samples[:,1], marker = '^',color = 'blue', s =40)
plt.legend(['Class1 (w1)', 'Class2 (w2)'], loc = 'upper right')
plt.title('Densities of 2 classes with 25 bivariate random patterns each')
plt.ylabel('x2')
plt.xlabel('x1')
ftext = 'p(x|w1) -N(mu1=(0,0)^t, cov1 = I)\np.(x|w2) -N(mu2 = (1, 1)^t), cov2 =I'
plt.figtext(.15,.8, ftext, fontsize = 11, ha ='left')

#Adding decision boundary to plot

x_1 = np.arange(-5, 5, 0.1)
bound = decision_boundary(x_1)
plt.plot(x_1, bound, 'r--', lw = 3)

x_vec = np.linspace(*ax.get_xlim())
x_1 = np.arange(0, 100, 0.05)

plt.show()
```

![](Figure_13.png)



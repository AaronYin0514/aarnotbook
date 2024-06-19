# matplotlib柱状图

## 案例1

```python
from matplotlib import pyplot as plt
from matplotlib import font_manager
my_font = font_manager.FontProperties(fname="/System/Library/Fonts/Hiragino Sans GB.ttc")

a = ["战狼2","速度与激情8","功夫瑜伽","西游伏妖篇"]
b = [56.01,26.94,17.53,16.49]

# 绘制条形图
# width：柱宽度
plt.bar(range(len(a)), b , width=0.3)

plt.xticks(range(len(a)), a, fontproperties=my_font, rotation=90)
plt.show()
```

![](Figure_14.png)

## 案例2

```python
from matplotlib import pyplot as plt
from matplotlib import font_manager
my_font = font_manager.FontProperties(fname="/System/Library/Fonts/Hiragino Sans GB.ttc")

a = ["战狼2","速度与激情8","功夫瑜伽","西游伏妖篇"]
b=[56.01,26.94,17.53,16.49]

#绘制条形图
plt.barh(range(len(a)), b, height=0.3, color="orange")
#设置字符串到x轴
plt.yticks(range(len(a)),a,fontproperties=my_font)

plt.show()
```

![](Figure_15.png)

## 案例3

```python
from matplotlib import pyplot as plt
from matplotlib import font_manager

my_font = font_manager.FontProperties(fname="/System/Library/Fonts/Hiragino Sans GB.ttc")
plt.figure(figsize=(20,8),dpi=80)

a = ["猩球崛起3：终极之战","敦刻尔克","蜘蛛侠：英雄归来","战狼2"]
b_16 = [15746,312,4497,319]
b_15 = [12357,156,2045,168]
b_14 = [2358,399,2358,362]

bar_width = 0.2

x_14 = list(range(len(a)))
x_15 =  [i+bar_width for i in x_14]
x_16 = [i+bar_width*2 for i in x_14]

plt.bar(range(len(a)),b_14,width=bar_width,label="9月14日")
plt.bar(x_15,b_15,width=bar_width,label="9月15日")
plt.bar(x_16,b_16,width=bar_width,label="9月16日")

#设置x轴的刻度
plt.xticks(x_15,a,fontproperties=my_font)

plt.legend(prop=my_font)
plt.show()
```

![](Figure_16.png)





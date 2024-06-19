# 随机数
## 随机数函数

生成`[0,1)`随机数（包含0，小于1）

```javascript
Math.random()
```

生成`[m,n)`随机数（包含m，小于n）

```javascript
Math.random() * (n - m) + m
```

## 取整函数

向上取整

```javascript
Math.ceil(x)
```

向下取整

```javascript
Math.floor(x)
```

四舍五入

```javascript
Math.round(x)
```

## 实例

```javascript
// 表示结果为1-10之间的一个随机数
Math.floor(Math.random()*10+1)

// 表示结果为0-23间的随机数
Math.floor(Math.random()*24)
```


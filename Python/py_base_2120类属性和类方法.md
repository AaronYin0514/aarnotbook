---
tags: [python, class]
---
# 类属性 和 类方法 和 静态方法

## 类属性

### 定义

```python
class Game(object):
  # 类属性
  max_score = 100

  # 私有类属性
  __company = '任地堂'
```

### 赋值 & 访问

```python
print(Game.max_score) # 100

Game.max_score = 120
print(Game.max_score) # 120
```

### 通过实例对象访问类属性

```python
game = Game()

print(Game.max_score) # 100
print(game.max_score) # 100 通过实例对象可以获取到类属性

Game.max_score = 120
print(Game.max_score) # 120

game.max_score = 130 # game实例对象有了一个实例属性max_score
print(Game.max_score) # 120
print(game.max_score) # 130 返回实例属性

del game.max_score
print(game.max_score) # 120 又能获取到类属性了
```

## 类方法

```python
import random

class Game(object):
  # 类属性
  __max_score = 0

  @classmethod
  def set_max_score(cls, score):
    if score > cls.__max_score:
      cls.__max_score = score
    else:
      print('你的分数不够')

  def play(self):
    print('开始游戏...')
    print('游戏结束.')
    score = random.randint(0, 1000)
    
    # Game.set_max_score(score) # 可以通过类调用类方法
    self.set_max_score(score) # 也可以通过实例对象调用类方法

    print('你的得分是: %d'%score)
    print('游戏的最高得分是: %d'%Game.__max_score)
```

方法前面加上`@classmethod`（装饰器），就可以定义一个类方法。注意第一个参数必须是类。即可以通过类调用类方法，也可以通过实例对象调用类方法。

```python
  @classmethod
  def set_max_score(cls, score):
    if score > cls.__max_score:
      cls.__max_score = score
    else:
      print('你的分数不够')
```

测试

```python
game = Game()
game.play()

'''
开始游戏...
游戏结束.
你的得分是: 987
游戏的最高得分是: 987
'''
```

### 静态方法

```python
class Game(object):
  @staticmethod
  def menu():
    print('静态方法')

# 测试
game = Game()
Game.menu()
game.menu()
```

通过在方法前加`@staticmethod`的方式，就定义了一个静态方法。

即可以通过类调用静态方法，也可以通过实例对象调用静态方法。与类方法和实例方法不同的是，静态方法没有参数上的限制，它与函数的定义方式相同。
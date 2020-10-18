---
title: Python入门教程-3-数据类型
date: 2020-06-12 17:52:02 +0800
categories: [Python入门教程]
tags: []
---

# 数字：int, float

Python 中的整数和数学上的整数一样，但是跟 Python2 和 C 语言相比不同的是，Python3 中的整数没有 `短整型(int)` 和 `长整型(long)` 的区别，即 `int` 直接相当于 Python2 和 C 语言中的 `长整型(long)` 类型。  

Python 中的 `浮点数(float)` 则相当于数学上的小数。叫做浮点数是由于在计算机中小数的表示方法有 `浮点表示` 和 `定点表示` 的区别，浮点表示即小数点的位置可变，比如 `1.23` 也可以表示为 `0.123e1`。关于浮点数可以参考《[浮点数的二进制表示](https://www.ruanyifeng.com/blog/2010/06/ieee_floating-point_representation.html)》。

四则运算：

```python
# 加减乘除
>>> 2 + 2
4
>>> 50 - 5*6
20
>>> (50 - 5*6) / 4  # 除法运算结果始终为浮点数
5.0
```

取余和幂运算：

```python
>>> 17 % 3  # 取余
2
>>> 2 ** 3  # 幂运算
8
>>> pow(2, 3)  # 也可以适用内置函数 pow 进行幂运算
8
```

地板除法（floor division）：

```python
>>> 11 // 4  # 地板除运算符(//)的结果将会向负无穷的方向取整
2
>>> -11 // 4  # -2.75 向负无穷的方向取整结果为 -3
-3

# math.floor 方法和 // 运算符结果一样
>>> import math
>>> math.floor(2.75)
2
>>> math.floor(-2.75)
-3

# math.ceil 则是向正无穷方向取整
>>> math.ceil(2.75)
3
>>> math.ceil(-2.75)
-2
```

int 和 float 混合运算时的规则：在数值范围表示上 int < float，所以 int 相对于 float 来说是窄类型（narrower），float 相对 int 来说是宽类型，在混合运算或比较时，窄类型会转换为宽类型进行计算，所以最终结果也是宽类型：

```python
# 窄类型 int 转换为宽类型 float 进行计算
>>> 2 + 2.0
4.0
>>> 2.0 - 1
1.0
>>> 2.0 * 3.0
6.0
>>> 2 ** 3.0
8.0
>>> 17 % 3.0
2.0

# 窄类型 int 转换为宽类型 float 进行比较
>>> 1 == 1.0
True
>>> [1, 2] == [1.0, 2.0]
True
```

# 字符串：str

字符串的 `单引号` 表示形式：

```python
>>> print('I am a string')
I am a string
>>> print('I\'m a string')  # 单引号中如果要包含单引号，需要用反斜杠(\)转义
I'm a string
>>> print('I am a "string"')  # 单引号中可以包含双引号(")
I am a "string"
```

字符串的 `双引号` 表示形式：

```python
>>> print("I am a string")
I am a string
>>> print("I am a \"string\"")  # 双引号中如果要包含双引号，需要用反斜杠(\)转义
I am a "string"
>>> print("I'm a string")  # 双引号中可以包含单引号(')
I'm a string
```

字符串的 `三引号` 表示形式：

```python
>>> print('''I am a string''')  # 三个单引号形式
I am a string
>>> print("""I am a string""")  # 三个双引号形式
I am a string
>>> print('''I'm a "string"''')  # 三引号中既可以包含单引号，也可以包含双引号
I'm a "string"

# 三引号中可以包含多行
print('''I am first line
I am second line''')
I am first line
I am second line

# 三引号中可以在行尾添加反斜杠(\)表示不换行
print('''I am first line, \
I am still first line''')
I am first line, I am still first line
```

字符串的 `raw` 表示形式：

```python
>>> print(' first line \n second line')  # \n表示换行
 first line
 second line

# 添加r前缀的字符串表示把反斜杠(\)当作普通字符，而不是转义符
>>> print(r' first line \n still first line')  
 first line \n still first line
```

字符串可以用 `+` 进行拼接：

```python
>>> print('Py' + 'thon')
Python
```

字符串也可以用 `空格` 进行拼接：

```python
>>> print('Py' 'thon')
Python
>>> s = 'thon'
>>> print('Py' s)  # 不能用空格连接变量字符串
  File "<stdin>", line 1
    print('Py' s)
               ^
SyntaxError: invalid syntax
```

字符串可以用 `*` 进行重复：

```python
>>> print('w' * 3)
www
```

字符串可以用列表索引截取：

```python
>>> s = 'Python'
>>> s[0]
'P'
>>> s[-1]
'n'
>>> s[0:2]  # 切片(slice)，切片将在后续章节进一步讲解
'Py'
```

# 布尔：bool

Python 中的布尔值使用 `True` 和 `False` 表示(注意区分大小写)，布尔运算符分别为 `and`、`or` 和 `not` ：

```python
# and 运算符
>>> True and True
True
>>> True and False
False
>>> False and False
False
>>> 5 > 3 and 3 > 1
True

# or 运算符
>>> True or True
True
>>> True or False
True
>>> False or False
False
>>> 5 > 3 or 1 > 3
True

# not 运算符
>>> not True
False
>>> not False
True
>>> not 1 > 2
True
```

Python 中的 `bool` 类其实是从 `int` 类继承实现的，`True` 和 `False` 分别是 `1` 和 `0` ：

```python
>>> ['a', 'b'][True]
'b'
>>> ['a', 'b'][False]
'a'
```

# 空：None

空值(`None`)在 Python 中是一个特殊值，与 JAVA 中的 `null` 相同，`None` 和 `0` 是不相同的，`0` 是一个 `int`类型的，是有值的。

如果一个函数没有 `return` 语句，那么默认就会返回 `None`。

# 引用资料

- [Numbers] : https://docs.python.org/3.5/tutorial/introduction.html#numbers
- [数据类型和变量] ：https://www.liaoxuefeng.com/wiki/1016959663602400/1017063826246112
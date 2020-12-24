---
title: Python入门教程-8-函数定义
date: 2020-11-24 10:45:50 +0800
categories: [Python入门教程]
tags: []
---

# 函数定义

先看一个简单的函数定义的例子，以 `def` 语句开头，定义一个名为 `add` 的函数，接收两个参数 `a` 和 `b`，然后返回这两个参数的 `和`：

```python
>>> def add(a, b):
...     return a + b
...
>>> add(1, 2)
3
>>>
```

在 Python 中，一个定义好的函数可以把函数名赋值给其他变量，然后通过其他变量也可以调用该函数：

```python
>>> def add(a, b):
...     return a + b
...
>>> plus = add
>>> plus(1, 2)
3
>>>
```

如果定义的函数体中没有明确使用 `return` 语句返回值，那么默认返回值是 `None`：

```python
>>> def nothing():
...     pass
...
>>> r = nothing()
>>> r is None
True
>>>
```

# 函数参数

## 参数默认值

定义函数时可以指定参数的默认值，调用函数时可以不给默认参数传值，不传值则会使用默认值：

```python
# 定义一个有两个参数有默认值的函数
>>> def introduce(name, age=18, city='beijing'):
...     print('My name is %s, I am %d years old and I am from %s' % (name, age, city))
...
>>> introduce('tom')  # age 和 city 都使用默认值
My name is tom, I am 18 years old and I am from beijing
>>> introduce('liubei', city='chengdu')  # city 不使用默认值
My name is liubei, I am 18 years old and I am from chengdu
>>> introduce('sanmao', 30, 'chongqin' )  # age 和 city 都不使用默认值
My name is sanmao, I am 30 years old and I am from chongqin
>>>
```

需要注意的是如果参数默认值使用变量的话，那在函数定义时就值已经确定了，即使后面再改变变量的值不会影响参数的默认值：

```python
>>> i = 5
>>> def f(arg=i):
...     print(arg)
...
>>> f()
5
>>> i = 6
>>> f()  # i 已经变为 6，但是 arg 参数的默认值仍然是 5
5
>>>
```

如果参数默认值是一个可变类型的话，可能会有一些意想不到的结果：

```python
>>> def f(a, L=[]):
...     L.append(a)
...     return L
...
>>> print(f(1))
[1]
>>> print(f(2))
[1, 2]
>>> print(f(3))  # 因为 L 是对一个列表的引用，多次调用都是操作的同一个列表
[1, 2, 3]
>>>

# 如果想要避免上面的情况，应该像下面这样写
>>> def f(a, L=None):
...     if L is None:
...         L = []
...     L.append(a)
...     return L
...
>>> print(f(1))
[1]
>>> print(f(2))
[2]
>>> print(f(3))
[3]
>>>
```

## *args 和 **kwargs

有两种比较特殊的形式 `*args` 和 `**kwargs` 在函数定义和其他情况下使用时有着不同的作用，在函数定义时使用，相当于定义了个数不确定的参数：

```python
# 此函数的 nums 相当于是一个序列
>>> def sum(*nums):
...     s = 0
...     for i in nums:
...         s += i
...     return s
...
# 调用时可以传入任意多个位置参数（按位置传入的参数即为位置参数）
>>> sum(1) 
1
>>> sum(1, 2)
3
>>> sum(1, 2, 3)
6
>>> sum() 
0
>>>
```

```python
# 此函数的 kwargs 相当于一个字典
>>> def fun(**kwargs):
...     for k, v in kwargs.items():
...         print('%s: %s' % (k, v))
...
# 调用时可以传入任意多个关键字参数（以 name=value 形式传入的参数即为关键字参数）
>>> fun(name='tom', age=18, city='beijing')
name: tom
age: 18
city: beijing
>>> fun()
>>>
```

当 `*args` 和 `**kwargs` 在其他地方使用时，其作用相当于对一个序列和字典进行 `解包` 操作：

```python
>>> def add(a, b):
...     return a + b
...
>>> nums = [1, 2]
>>> add(*nums)  # 函数调用时在 nums 前面加 * 相当于解包 nums
3
>>>
```

```python
>>> def introduce(name, age):
...     print('My name is %s, I am %d years old.' % (name, age))
...
>>> d = {'name': 'tom', 'age': 18}
>>> introduce(**d)  # 函数调用时在字典 d 前面加 ** 相当于解包 d
My name is tom, I am 18 years old.
>>>
```

# 匿名函数（lambda）

Python 中运行使用 `lambda` 语句定义一个匿名函数，通常用在只需要一行代码即可完成定义的函数：

```python
# 以下使用 lambda 定义一个匿名函数，函数有两个参数 a 和 b，函数体返回 a + b
>>> add = lambda a, b: a + b
>>> add(1, 2)
3
>>>
```

# 函数文档字符串（docstr）

定义函数时可以在文档字符串中对函数进行简介或详细说明，同时文档字符串还可以方便其他文档生成工具自动生成代码的函数说明文档。文档字符串在函数定义 `def` 语句的下一行开始写，以三个双引号（`"""`）进行包裹，可以是一行或多行：

```python
# 只有一行文档字符串进行简要介绍
def add(a, b):
    """This is an add function."""
    return a + b

# 多行文档字符串对函数进行详细说明
def add(a, b):
    """This is an add function.

    :param a: number a
    :type a: int
    :param b: number b
    :type b: int
    :return: The sum of a and b.
    """
    return a + b
```

# 函数注解（Function Annotations）

从 Python3 开始支持定义函数时使用 `注解` 方式说明参数和返回值类型：

```python
# 使用注解说明参数 a, b 和返回值类型都是 int
>>> def add(a: int, b: int) -> int:
...     return a + b
...
>>> add(1, 2)
3
>>>
```

更多关于函数注解说明可以参考 [PEP 484](https://www.python.org/dev/peps/pep-0484/)

# 引用资料

[Defining Functions] : https://docs.python.org/3.5/tutorial/controlflow.html#defining-functions  
[More on Defining Functions] : https://docs.python.org/3.5/tutorial/controlflow.html#more-on-defining-functions  
[Function definitions] : https://docs.python.org/3.5/reference/compound_stmts.html#function-definitions  
[PEP 484] : https://www.python.org/dev/peps/pep-0484/

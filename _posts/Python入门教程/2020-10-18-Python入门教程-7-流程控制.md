---
title: Python入门教程-7-流程控制
date: 2020-10-18 15:58:25 +0800
categories: [Python入门教程]
tags: []
---

# if

if 是条件控制语句，其语法定义如下：

```
if_stmt ::=  "if" expression ":" suite
             ( "elif" expression ":" suite )*
             ["else" ":" suite]
```

`if` 是固定格式，当 `if` 后面的 `expression` 为 `真` 时执行其后面的 `suite`，然后可以在后面接 0 或任意多个 `elif` 语句，最后一条 `else` 语句为 `可选`。  

```python
>>> x = 8
>>> if x < 0:
...     print('negative')
... elif x == 0:
...     print('zero')
... else:
...     print('positive')
... 
positive
>>> 
```

Python 中没有像 C 语言的 `switch...case` 这样的结构，因为使用 `elif` 就能达到这样的效果。

## 真值计算

对于 `if` 语句后面的表达式 `expression` 的计算结果，除了以下几种情况为 `假` 之外，其他情况均为 `真`：

* `None`
* `False`
* 数字类型零：`0`, `0.0`, `0j`
* 空序列：`''`, `()`, `[]`
* 空字典：`{}`
* 如果是用户自定义类，然后实现了 `__bool__` 或 `__len__` 方法，并且其返回结果为 `False` 或者 `0`，那么也被当作 `假`

```python
# None
>>> if None:
...     print('should not print')
... else:
...     print('should print')
... 
should print
>>> 

# 整数0
>>> if 0:
...     print('should not print')
... else:
...     print('should print')
... 
should print
>>>

# 空字符串
>>> if '':
...     print('should not print')
... else:
...     print('should print')
... 
should print
>>> 

# 空字典
>>> if {}:
...     print('should not print')
... else:
...     print('should print')
... 
should print
>>> 
```

## 布尔运算符

有时在 `if` 后面的 `expression` 中会是一个由 `布尔运算符` 连接多个部分所组成的表达式，比如：

```python
>>> p = '/home/user1/'
>>> if p.startswith('/') and p.endswith('/'):
...     print('The absolute path of a directory')
... 
The absolute path of a directory
>>> 
```

对于包含 `布尔运算符` 的表达式的真假值判断遵循以下规范：

| 操作符 | 结果 |
| ----- | ---- |
| x `or` y | 如果 x 为假，则结果为 y，否则为 x |
| x `and` y | 如果 x 为假，则结果为 x，否则为 y |
| `not` x | 如果 x 为假，则结果为 `True`，否则为 `False` |

(1) `or` 是一个 `短路运算符`，只有当 x 为 `假` 时才会计算 y。  
(2) `and` 也是一个 `短路运算符`，只有当 x 为 `真` 时才会计算 y。  
(3) 这 3 个布尔运算符的优先级关系为：not > and > or  

```python
>>> 1 or 2
1
>>> 0 or 2
2
>>> 

>>> 1 and 2
2
>>> 0 and 1
0
```

## 比较运算符

比较运算符也常用于 `if` 后面的 `expression` 表达式，比较运算符总是返回 `bool` 类型的结果，即 `True` 或 `Flase`，所有比较运算符如下：

| 运算符 | 说明 |
| ----- | ---- |
| < | 小于 |
| <= | 小于等于 |
| > | 大于 |
| >= | 大于等于 |
| == | 等于 |
| != | 不等于 |
| is | 判断是否是某个对象 |
| is not | 判断是否不是某个对象 |

# for

for 语句的定义如下：

```
for_stmt ::=  "for" target_list "in" expression_list ":" suite
              ["else" ":" suite]
```

Python 中的 for 循环只有 `for...in` 的形式，它遍历序列 `expression_list`，每次遍历取出的元素赋值给 `target_list`，第一个 `suite` 是循环体，然后还可以使用一个 `else` 子句，else 只有在 for 循环正常结束之后才会执行，如果在 for 循环中被 `break` 打断了，则会跳过 `else` 子句。

```python
# 遍历一个 list
>>> for c in ['a', 'b', 'c']:
...     print(c)
... 
a
b
c
>>> 

# 如果想要类似 C 语言中的 for 循环，可以使用 range
# 关于 range 的详细说明可参考前面《列表与元组》章节
>>> for i in range(5):
...     print(i)
... 
0
1
2
3
4
>>> 

# 如果使用了 else 子句，那么会在循环正常结束后再执行 else 子句
>>> sum = 0
>>> for i in (1, 2, 3):
...     sum += i
... else:
...     print('sum = %d' % sum)
... 
sum = 6
>>> 

# 如果 for 循环体中使用了 break，那么则不会执行 else 子句
>>> sum = 0
>>> for i in (1, 2, 3):
...     sum += i
...     if i == 2:
...         break
... else:
...     print('sum = %d' % sum)
... 
>>> print(sum)
3
>>> 
```

如果需要在 for 循环时同时取序列中元素的下标和值，可以使用 `enumerate` 函数：

```python
>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print(i, v)
...
0 tic
1 tac
2 toe
```

如果需要在 for 循环遍历一个序列的同时修改该序列，建议是对该序列做一个拷贝然后再遍历该拷贝，如果不用拷贝的方式，直接在遍历该序列的同时修改该序列，可能会出现一些不可意料的情况：

```python
# 想要去除 nums 中的偶数
>>> nums = [1, 2, 4, 3]
>>> for n in nums:
...     if n % 2 == 0:
...         nums.remove(n)
... 
>>> nums
# python 在循环时内部会有一个计数器记录当前循环下标(从 0 开始)，并且每次自动加 1
# 所以当循环到元素 2 时，记录的当前下标为 1，然后判断 2 为偶数并移除了它，然后后面
# 的元素依次往前挪一位，当进入下一次循环时自动取下标为 2 的元素，则取到了元素 3，这
# 样就把元素 4 给漏掉了。
[1, 4, 3]
>>> 

# 这种情况一般建议拷贝一份序列后再对其进行遍历
>>> nums = [1, 2, 4, 3]
>>> for n in nums.copy():
...     if n % 2 == 0:
...         nums.remove(n)
... 
>>> nums
[1, 3]
>>>

# 还可以使用切片方式 nums[:] 创建一个拷贝，比 copy 的方式更简洁
>>> nums = [1, 2, 4, 3]
>>> for n in nums[:]:
...     if n % 2 == 0:
...         nums.remove(n)
... 
>>> nums
[1, 3]
>>> 
```

使用 for 循环对字典遍历时默认遍历其 `key`，但是也可以使用字典的 `.items()` 方法同时遍历其 `key` 和 `value`，或者使用字典的 `.values()` 方法只遍历其 `value`：

```python
# 遍历字典的 key
>>> d = {'name': 'tom', 'age': 18}
>>> for k in d:
...     print(k)
... 
name
age
>>> 

# 同时遍历 key 和 value
>>> for k, v in d.items():
...     print(k, v)
... 
name tom
age 18
>>> 

# 只遍历 value
>>> for v in d.values():
...     print(v)
... 
tom
18
>>> 
```

如果需要使用 for 循环同时遍历多个长度相同的序列时可以使用 `zip` 函数：

```python
# 同时遍历 2 个序列
>>> keys = ['name', 'age']
>>> values = ['tom', 18]
>>> for k, v in zip(keys, values):
...     print(k, v)
... 
name tom
age 18
>>> 

# 同时遍历 3 个序列
>>> for i, j, k in zip([1, 2], [3, 4], [5, 6]):
...     print(i, j, k)
... 
1 3 5
2 4 6
>>> 
```

如果需要使用 for 循环反向遍历一个序列时，可以使用 `reversed` 函数：

```python
>>> for i in reversed([1, 2, 3]):
...     print(i)
... 
3
2
1
>>> 
```

# while

`while` 语句的定义如下：

```
while_stmt ::=  "while" expression ":" suite
                ["else" ":" suite]
```

当表达式 `expression` 为 `真` 时执行 `suite`，后边还可以跟一个可选的 `else` 子句，当表达式 `expression` 为 `假` 时执行 `else` 子句中的 `suite`，如果在第一个 `suite` 中执行了 `break`，则退出循环，并且不会执行 `else` 子句。

```python
# 使用 while 循环计算 1 ~ 10 的和
>>> i = 1
>>> sum = 0
>>> while i <= 10:
...     sum += i
...     i += 1
... 
>>> sum
55
>>> 

# else 子句在 while 的 expression 为假时执行
>>> i = 1
>>> sum = 0
>>> while i <= 10:
...     sum += i
...     i += 1
... else:
...     print('loops over')
... 
loops over
>>> sum
55

# while 的循环体中执行了 break，所以子句 else 被跳过不执行
>>> i = 1
>>> sum = 0
>>> while i <= 10:
...     sum += i
...     i += 1
...     if i == 9:
...         break
... else:
...     print('loop over')
... 
>>> sum
36
>>> 
```

# break 

`break` 语句用于 `for` 或者 `while` 循环体中，执行后直接退出循环，并且不会执行循环的 `else` 子句，具体示例请见上面的 for 和 while 小节内容，这里不再重复举例。

# continue

`continue` 语句用在 `for` 或者 `while` 循环体中，跳过本次循环中后续的内容，然后进入下一次循环，它不会结束整个循环，并且不影响循环的 else 子句。

```python
# 使用 continue 跳过 for 循环中偶数的打印步骤，并且不会影响到 else 子句
>>> for i in range(1, 11):
...     if i % 2 == 0:
...         continue
...     print(i)
... else:
...     print('loops over')
... 
1
3
5
7
9
loops over
>>> 

# 使用 continue 跳过 while 循环中偶数的打印步骤，并且不会影响到 else 子句
>>> i = 0
>>> while i <= 10:
...     i += 1
...     if i % 2 == 0:
...         continue
...     print(i)
... else:
...     print('loops over')
... 
1
3
5
7
9
11
loops over
>>> 
```

# pass

`pass` 语句不做任何事情，通常用于一些在语法上需要写内容，但是实际没有内容可写的情况下进行占位。

```python
# 自定义异常类
class MyException(Exception):
    pass

# 先定好函数名，函数体还需进一步思考后编写
def myfun():
    pass
```

# 引用资料

[More Control Flow Tools] : https://docs.python.org/3.5/tutorial/controlflow.html  
[Truth Value Testing] : https://docs.python.org/3.5/library/stdtypes.html#truth-value-testing  
[Boolean Operations] : https://docs.python.org/3.5/library/stdtypes.html#boolean-operations-and-or-not  
[Comparisons] : https://docs.python.org/3.5/library/stdtypes.html#comparisons  
[The if statement] : https://docs.python.org/3.5/reference/compound_stmts.html#the-if-statement  
[The while statement] : https://docs.python.org/3.5/reference/compound_stmts.html#the-while-statement  
[The for statement] : https://docs.python.org/3.5/reference/compound_stmts.html#the-for-statement
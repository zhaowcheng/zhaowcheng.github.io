---
title: Python入门教程-6-字典与集合
date: 2020-08-06 10:31:58 +0800
categories: [Python入门教程]
tags: []
---

# 字典（dict）

字典通过 `键值对(key: value)` 的方式，把 key 映射到 value。key 必须是 `可 hash 的`([hashable](https://docs.python.org/3.5/glossary.html#term-hashable))，由于 Python 中所有内置的 `不可变类型`([immutable](https://docs.python.org/3.5/glossary.html#term-immutable)) 都是可 hash 的，所以都可用作字典的 key，比如 `字符串(str)`、`数字(int, float)` 和 `只包含不可变类型元素的元组(tuple)`，如果元组直接或间接的包含了 `可变类型` 的元素，也不能作为字典的 key。value 则可以是任意类型。

字典中元素是无序的，并不会按照插入的顺序排列，可能会是任意的顺序，如果想要有序的字典，可以使用 [collections.OrderedDict](https://docs.python.org/3.5/library/collections.html#collections.OrderedDict)。但是从 `Python 3.7` 开始，字典默认是按照插入顺序排序的了。

## 字典创建

创建一个字典可以通过以下 3 种方式：

- 大括号 {} 方式：

    ```python
    >>> d = {'name': 'lilei', 'age': 18}
    >>> d
    {'age': 18, 'name': 'lilei'}
    ```

- dict 关键字方式：

    dict 的定义如下：

    ```python
    class dict(**kwarg)
    class dict(mapping, **kwarg)
    class dict(iterable, **kwarg)
    ```

    根据定义 dict 接受如下形式的参数进行初始化：

    ```python
    >>> dict(one=1, two=2)   # 关键字参数形式(**kwargs)
    {'one': 1, 'two': 2}
    >>> dict({'one': 1, 'two': 2})  # 字典形式(mapping)
    {'one': 1, 'two': 2}
    >>> dict([('one', 1), ['two', 2]])  # 可迭代类型中内嵌2个元素的可迭代类型(iterable)
    {'one': 1, 'two': 2}

    >>> dict({'one': 1}, two=2)  # 字典+关键字参数形式(mapping + **kwargs)
    {'one': 1, 'two': 2}
    >>> dict([('one', 1)], two=2)  # 可迭代类型+关键字参数形式(iterable + **kwargs)
    {'one': 1, 'two': 2}
    ```

- 字典推导式(dict comprehension)：

    ```python
    >>> {x: x**2 for x in (2, 4, 6)}
    {2: 4, 4: 16, 6: 36}
    ```

## 字典操作

- len(_d_)

    返回字典 _d_ 的长度，即包含多少个键值对

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> len(d)
    2
    ```

- d[_key_]

    返回 _key_ 对应的 value，如果 _key_ 不存在，则抛出 `KeyError` 异常

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d['one']
    1
    >>> d['three']
    Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
    KeyError: 'three'
    ```

- d[_key_] = value

    设置 _key_ 对应的 value，如果 _key_ 不存在则自动添加

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d['two'] = 2.0
    >>> d['three'] = 3
    >>> d
    {'one': 1, 'two': 2.0, 'three': 3}
    ```

- del d[_key_]

    删除 _key_ 对应的项，如果 _key_ 不存在则抛出 `KeyError` 异常

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> del d['two']
    >>> d
    {'one': 1}
    ```

- pop(_key_[, _default_])

    删除 _key_ 对应的项并且返回对应的 value，如果 _key_ 不存在并且传入了 _default_ 参数，那么返回 _default_，否则抛出 `KeyError` 异常

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d.pop('two')
    2
    >>> d.pop('three', 0)  # 不存在 key 为 'three' 的项则返回 0
    0
    >>> d
    {'one': 1}
    ```

- popitem()

    任意删除并返回一个项，由于字典是无序的，所以删除的项是不确定的

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d.popitem()
    ('one', 1)
    >>> d
    {'two': 2}
    ```

    从 `Python 3.7` 开始字典是有序的（按照插入顺序排序），所以 popitem() 会按照 `LIFO` 规则删除，即相当于删除最后一个项

- key in d

    如果 key 存在于字典 d 中，则返回 `True`，否则返回 `False`

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> 'one' in d
    True
    >>> 'three' in d
    False
    ```

- key not in d

    如果 key 不存在于字典 d 中，则返回 `True`，否则返回 `False`

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> 'one' not in d
    False
    >>> 'three' not in d
    True
    ```

- iter(_d_)

    返回一个包含字典 _d_ 的所有 key 的迭代器（iterator）

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> for k in iter(d):
    ...     print(k)
    ...
    one
    two
    ```

- clear()

    清空字典

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d.clear()
    >>> d
    {}
    ```

- copy()

    返回字典的 `浅拷贝`，关于 `浅拷贝` 与 `深拷贝` 可以参考 `《列表与元组》` 章节

    ```python
    >>> d1 = {'one': 1, 'two': 2}
    >>> d2 = d1.copy()
    >>> d2
    {'one': 1, 'two': 2}
    ```

- classmethod fromkeys(_seq_[, _value_])

    返回一个新的字典，字典的 keys 来自参数 _seq_，并且所有 key 的 value 为参数 _value_，_value_ 参数默认值为 `None`。  
    这是一个 `类方法`，用于以其他形式创建字典。

    ```python
    >>> dict.fromkeys(['three', 'four'])
    {'four': None, 'three': None}
    >>> dict.fromkeys(['three', 'four'], 0)  # 初始值为 0
    {'four': 0, 'three': 0}
    >>>
    ```

- get(_key_[, _default_])

    返回 _key_ 对应的 value，如果 _key_ 不存在则返回 _default_，_default_ 默认为 `None`

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d.get('one')
    1
    >>> d.get('three', 0)  # 不存在则返回 0
    0
    ```

- items()

    返回一个包含所有项的 `字典视图(view)`，每一项是一个 2 个元素的 tuple，分别为 key 和 value

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> for k, v in d.items():
    ...     print('%s = %s' % (k, v))
    ...
    one = 1
    two = 2
    ```

- keys()

    返回一个包含所有 key 的 `字典视图(view)`

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> for k in d.keys():
    ...     print(k)
    ...
    one
    two
    ```

- values()

    返回一个包含所有 value 的 `字典视图(view)`

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> for v in d.values():
    ...     print(v)
    ...
    1
    2
    ```

- setdefault(_key_[, _default_])

    如果 _key_ 存在于字典中，则返回其对应的 value，否则插入 _key_ ，并且将其 value 设置为 _default_，_default_ 默认为 `None`

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d.setdefault('two')
    2
    >>> d.setdefault('three', 3)
    3
    >>> d
    {'one': 1, 'two': 2, 'three': 3}
    ```

- update([_other_])

    更新字典中的项，可接受参数类型与 dict 关键字一样

    ```python
    >>> d = {'one': 1, 'two': 2}
    >>> d.update({'two': 2.0, 'three': 3})  # mapping 形式
    >>> d
    {'one': 1, 'two': 2.0, 'three': 3}
    >>> d.update([('three', 3.0), ('four', 4)])  # iterable 形式
    >>> d
    {'one': 1, 'two': 2.0, 'three': 3.0, 'four': 4}
    >>> d.update(five=5, six=6)  # **kwargs 形式
    >>> d
    {'four': 4, 'two': 2.0, 'three': 3.0, 'six': 6, 'one': 1, 'five': 5}
    >>> d.update({'six': 6.0}, seven=7)  # mapping + **kwargs 形式
    >>> d
    {'four': 4, 'two': 2.0, 'three': 3.0, 'six': 6.0, 'one': 1, 'seven': 7, 'five': 5}
    >>> d.update([('one', 1.0), ('four', 4.0)], five=5.0, seven=7.0)  # iterable + **kwargs 形式
    >>> d
    {'four': 4.0, 'two': 2.0, 'three': 3.0, 'six': 6.0, 'one': 1.0, 'seven': 7.0, 'five': 5.0}
    ```

## 字典视图

上面提到的 d.keys()、d.values() 和 d.items() 等方法返回的都是一个 `视图类型(view object)`，通过这些视图可以访问字典 d 的 keys、values 或 items，并且视图会随着字典的变化而自动更新。

```python
>>> d = {'one': 1, 'two': 2}
>>> keys = d.keys()
>>> values = d.values()
>>> items = d.items()
>>> keys
dict_keys(['one', 'two'])
>>> values
dict_values([1, 2])
>>> items
dict_items([('one', 1), ('two', 2)])

# 视图随着字典变化而动态更新
>>> d.update(three=3)
>>> keys
dict_keys(['one', 'two', 'three'])
>>> values
dict_values([1, 2, 3])
>>> items
dict_items([('one', 1), ('two', 2), ('three', 3)])

# 可以对视图进行迭代(iteration)
>>> for k in keys:
...     print(k)
...
one
two
three
```

## 字典比较

字典只支持 `==` 比较操作符，当且仅当所有项都相等时才相等。对于 `<`、`>`、`<=` 和 `>=` 比较操作符都不支持，如果使用会抛 `TypeError` 错误。

# 集合（set）

`集合(set)` 是一些 `无序(unordered)`、`无重复(no duplicate)` 并且 `可hash(hashable)` 元素的组合。通常用于 `是否包含某个成员的检测`、`去除重复元素` 等操作。同时也支持数学上的集合运算，比如 `交集(intersection)`、`并集(union)`、`差集(difference)` 和 `对称差集(symmetric difference)`。

Python 内置两种类型集合，`set` 和 `frozenset`，set 是可变类型(mutable)，frozenset 是不可变类型(immutable)。

## 集合创建

集合创建可以通过以下 3 种方式：

- 大括号 {} 方式：

    ```python
    >>> s = {'a', 'b', 'c', 'a'}
    >>> s
    {'c', 'b', 'a'}  # 相等元素只保留第一个
    ```

- set 或 frozenset 关键字方式：

    set 和 frozenset 的定义如下：

    ```python
    class set([iterable])
    class frozenset([iterable])
    ```

    根据以上定义可以通过下面的方式创建集合：

    ```python
    >>> s = set(['a', 'b', 'c'])
    >>> fs = frozenset(['a', 'b', 'c'])
    >>> s
    {'c', 'b', 'a'}
    >>> fs
    frozenset({'c', 'b', 'a'})
    >>> s.add('d')  # set 是可变类型
    >>> s
    {'c', 'b', 'd', 'a'}
    >>> fs.add('d')  # frozenset 是不可变类型
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    AttributeError: 'frozenset' object has no attribute 'add'
    >>>
    ```

- 集合推导式(set comprehension):

    ```python
    >>> {x for x in range(1, 11)}
    {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    ```

集合中的元素必须是 `可hash的`([hashable](https://docs.python.org/3.5/glossary.html#term-hashable))

```python
# str、numeric 和 tuple 类型都是可hash的
>>> {'one', 2, (3, 4)}
{'one', 2, (3, 4)}

# list 是 unhashable 类型
>>> {1, [2]}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'

# 间接包含 unhashable 类型也不行
>>> {1, (2, [3])}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

如果要创建空集合，必须使用 set 或 frozenset 关键字，不能使用大括号，因为 {} 创建的是一个空字典。

```python
>>> s = set()
>>> s
set()
>>> fs = frozenset()
>>> fs
frozenset()
>>> d = {}
>>> d
{}
>>> type(d)
<class 'dict'>
```

## 集合操作

以下是 `set` 和 `frozenset` 都支持的操作：

- len(s)

    返回结合中元素的个数

    ```python
    >>> s = {1, 2}
    >>> len(s)
    2
    ```

- x in s

    判断 x 是否被包含于集合 s 中

    ```python
    >>> s = {1, 2}
    >>> 1 in s
    True
    >>> 3 in s
    False
    ```

- x not in s

    判断 x 是否不被包含于集合 s 中

    ```python
    >>> s = {1, 2}
    >>> 1 not in s
    False
    >>> 3 not in s
    True
    ```

- isdisjoint(_other_)

    判断当前集合是否与其他集合 _other_ `没有交集`

    ```python
    >>> s = {1, 2}
    >>> s.isdisjoint({3, 4})
    True
    >>> s.isdisjoint({2, 3})
    False
    ```

- issubset(_other_)  
  set <= other

    判断当前集合是否为其他集合 _other_ 的 `子集`

    ```python
    >>> s = {1, 2}
    >>> other = {1, 2, 3}
    >>> s.issubset(other)
    True
    >>> s <= other
    True
    >>> other = {1, 4, 3}
    >>> s.issubset(other)
    False
    >>> s <= other
    False
    ```

- set < other

    判断当前集合是否为其他集合 other 的 `真子集`

    ```python
    >>> {1, 2} < {1, 2, 3}
    True
    >>> {1, 2} < {1, 2}
    False
    ```

- issuperset(_other_)  
  set >= other

    判断当前集合是否为其他集合 _other_ 的 `超集`

    ```python
    >>> s = {1, 2, 3}
    >>> other = {1, 2}
    >>> s.issuperset(other)
    True
    >>> s >= other
    True
    ```

- set > other

    判断当前集合是否其他集合 other 的 `真超集`

    ```python
    >>> {1, 2, 3} > {1, 2}
    True
    >>> {1, 2} > {1, 2}
    False
    ```

- union(*others)  
  set | other | ...

    当前集合与其他一个或多个集合求 `并集`

    ```python
    >>> s = {1, 2}
    >>> other = {2, 3}
    >>> s.union(other)
    {1, 2, 3}
    >>> s | other
    {1, 2, 3}
    ```

- intersection(*others)  
  set & other & ...

    当前集合与其他一个或多个集合求 `交集`

    ```python
    >>> s = {1, 2}
    >>> other = {2, 3}
    >>> s.intersection(other)
    {2}
    >>> s & other
    {2}
    ```

- difference(*others)  
  set - other - ...

    当前集合与其他一个或多个集合求 `差集`

    ```python
    >>> s = {1, 2}
    >>> other = {2, 3}
    >>> s.difference(other)
    {1}
    >>> s - other
    {1}
    ```

- symmetric_difference(_other_)  
  set ^ other

    当前集合与其他集合 _other_ 求 `对称差集`

    ```python
    >>> s = {1, 2}
    >>> other = {2, 3}
    >>> s ^ other
    {1, 3}
    >>> s.symmetric_difference(other)
    {1, 3}
    ```

- copy()

    返回当前集合的一个 `浅拷贝`

    ```python
    >>> s = {1, 2}
    >>> s.copy()
    {1, 2}
    ```

需要注意的是以上的 `比较方法` union()、intersection()、difference()、symmetric_difference()、issubset() 和 issuperset()，接受的参数除了集合类型外，还可以是 `可迭代类型(iterable)`。相比之下，对应的 `比较运算符` 则只能接受集合类型参数。

```python
>>> {1, 2}.union([3, 4])  # union 方法可以接受 iterable 类型
{1, 2, 3, 4}
>>> {1, 2} | [3, 4]  # | 运算符不能接受 iterable 类型
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for |: 'set' and 'list'
```

以下是只有 `set` 支持的操作：

- update(*others)  
  set |= other | ...

    将当前集合与其他一个或多个集合求 `并集` 然后更新到当前集合

    ```python
    >>> s = {1, 2}
    >>> other = {2, 3}
    >>> s.update(other)
    >>> s
    {1, 2, 3}
    >>> s |= {3, 4}
    >>> s
    {1, 2, 3, 4}
    ```

- intersection_update(*others)  
  set &= other & ...

    将当前集合与其他一个或多个集合求 `交集` 然后更新到当前集合

    ```python
    >>> s = {1, 2}
    >>> other = {2, 3}
    >>> s.intersection_update(other)
    >>> s
    {2}
    >>> s &= {3, 4}
    >>> s
    set()
    ```

- difference_update(*others)  
  set -= other | ...

    将当前集合与其他一个或多个集合求 `差集` 然后更新到当前集合

    ```python
    >>> s = {1, 2}
    >>> other = {2, 3}
    >>> s.difference_update(other)
    >>> s
    {1}
    >>> s -= {3, 4}
    >>> s
    {1}
    ```

- symmetric_difference_update(_other_)  
  set ^= other

    将当前集合与其他集合 _other_ 求 `对称差集` 然后更新到当前集合

    ```python
    >>> other = {2, 3}
    >>> s.symmetric_difference_update(other)
    >>> s
    {1, 3}
    >>> s ^= {3, 4}
    >>> s
    {1, 4}
    ```

- add(_elem_)

    添加一个元素 _elem_ 到当前集合

    ```python
    >>> s = {1, 2}
    >>> s.add(3)
    >>> s
    {1, 2, 3}
    ```

- remove(_elem_)

    从当前集合删除元素 _elem_ ，如果 _elem_ 不存在则抛 `KeyError` 异常

    ```python
    >>> s = {1, 2}
    >>> s.remove(2)
    >>> s
    {1}
    >>> s.remove(2)
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    KeyError: 2
    ```

- discard(elem)

    从当前集合删除元素 _elem_ ，如果 _elem_ 不存在也不抛异常

    ```python
    >>> s = {1, 2}
    >>> s.discard(2)
    >>> s
    {1}
    >>> s.discard(2)
    >>> s
    {1}
    ```

- pop()

    从当前集合任意删除一个元素并返回，由于集合是无序的，所以删除哪个元素不确定，如果当前集合为空，则抛 `KeyError` 异常

    ```python
    >>> s = {1, 2}
    >>> s.pop()
    1
    >>> s.pop()
    2
    >>> s.pop()
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    KeyError: 'pop from an empty set'
    ```

- clear()

    清空当前集合

    ```python
    >>> s = {1, 2}
    >>> s.clear()
    >>> s
    set()
    ```

同样的以上的 update()、intersection_update()、difference_update() 和 symmetric_difference_update() 也接受可迭代类型参数，但是对应的 `运算符` 则只能接受集合类型参数。

# immutable 与 hashable

Python 中不可变类型 `immutable` 都是 `hashable` 类型的，但是 hashable 类型并不一定都是 immutable 的，因为默认所有 `自定义类的实例` 都是 hashable 类型的，其 hash 值通常就是 `id()` 函数的计算结果，由于用户自定义类不一定是不可变类型的，所以 hashable 类型不一定都是 immutable 类型。

关于 hashable 可参考：https://docs.python.org/3.5/glossary.html#term-hashable  
关于 immutable 可参考：https://docs.python.org/3.5/glossary.html#term-immutable

# 引用资料

[Sets] : https://docs.python.org/3.5/tutorial/datastructures.html#sets  
[Dictionaries] : https://docs.python.org/3.5/tutorial/datastructures.html#dictionaries  
[Set Types — set, frozenset] : https://docs.python.org/3.5/library/stdtypes.html#set-types-set-frozenset  
[Mapping Types — dict] : https://docs.python.org/3.5/library/stdtypes.html#mapping-types-dict

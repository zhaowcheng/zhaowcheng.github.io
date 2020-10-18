---
title: Python入门教程-4-字符串
date: 2020-07-10 17:39:07 +0800
categories: [Python入门教程]
tags: []
---

在上一节课（数据类型）中对 `字符串(str)` 进行了一个简单的介绍，由于字符串在 Python 中使用频率较高，所以这一节课中专门对字符串进行一个深入的讲解。

# 字符串方法

`字符串(str)` 在 Python 中是一个 `对象(object)`，并且包含很多的 `方法(method)`，这些方法可以给我们操作字符串带来很大的方便，下面列举一些常用的方法及其使用示例：

- str.capitalize()  

    把字符串的首字母变成大写。

    ```python
    >>> 'hello'.capitalize()
    'Hello'
    ```

- str.lower()

    把字符串转换为小写。

    ```python
    >>> 'Hello'.lower()
    'hello'
    ```

- str.upper()

    把字符串转换为大写。

    ```python
    >>> 'Hello'.upper()
    'HELLO'
    ```

- str.center(_width_[, _fillchar_])

    返回一个长度为 _width_，原始字符串居中，两边多余长度使用 _fillchar_ 进行填充的新字符串。

    ```python
    >>> 'abc'.center(10, '=')
    '===abc===='
    >>> 'abc'.center(10)  # fillchar 默认为空格
    '   abc    '
    >>> 'abc'.center(3)  # 如果 width 小于等于字符串长度，则返回原始字符串
    'abc'
    ```

- str.count(_sub_[, _start_[, _end_]])

    在字符串的 _start_ 到 _end_ 范围内查找子字符串 _sub_ 出现的次数。  
    _start_ 默认为0，_end_ 默认为字符串长度，即默认查找整个字符串，这2个参数在其他方法中类似，不再重复说明。

    ```python
    >>> 'abcad'.count('a')
    2
    >>> 'abcad'.count('a', 0, 3)
    1
    >>> 'abcad'.count('a', 0, 4)
    2
    ```

- str.encode(_encoding_="utf-8", _errors_="strict")

    对字符串按照 _encoding_ 进行编码。

    ```python
    >>> '中文'.encode()
    b'\xe4\xb8\xad\xe6\x96\x87'
    ```

- str.startswith(_prefix_[, _start_[, _end_]])

    判断字符串在 _start_ 到 _end_ 范围内是否是以 _prefix_ 开头，是返回 True，否则返回 False。  
    _prefix_ 可以是多个，以 tuple 格式传入。  
    str.endswith 方法与此方法类似。

    ```python
    >>> 'abc'.startswith('a')
    True
    >>> 'abc'.startswith('b')
    False
    >>> 'abc'.startswith(('a', 'b'))  # 是否以 'a' 或 'b' 开头
    True
    ```

- str.find(_sub_[, _start_[, _end_]])

    在字符串的 _start_ 到 _end_ 范围内查找子字符串 _sub_，如果查到到则返回 _sub_ 出现的最小起始索引号，否则返回 -1 。

    ```python
    >>> 'abcdbf'.find('b')
    1
    >>> 'abcdbf'.find('x')
    -1
    ```

    如果只需要判断是否包含子字符串，可以使用 in 关键字进行判断，无需使用 find 方法。

    ```python
    >>> 'b' in 'abc'
    True
    >>> 'd' in 'abc'
    False
    ```

- str.index(_sub_[, _start_[, _end_]])

    与 str.find 方法类似，不同的是如果没有查找到子字符串 _sub_ ，会抛出 ValueError 异常。

    ```python
    >>> 'abcdbf'.index('b')
    1
    >>> 'abcdbf'.index('x')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: substring not found
    >>>
    ```

- str.format(_*args_, _**kwargs_)

    格式化字符串，此方法在下面会进行详细讲解，这里只演示一个简单示例。

    ```python
    >>> "The sum of 1 + 2 is {0}".format(1+2)
    'The sum of 1 + 2 is 3'
    ```

- str.join(_iterable_)

    把 _iterable_ 中的字符串元素用 _str_ 进行连接。  
    _iterable_ 可以是 list、tuple 等可迭代对象，但是里边的元素必须是 str 类型。

    ```python
    >>> '_'.join(['a', 'b', 'c'])
    'a_b_c'
    >>> '_'.join([1, 2, 3])  # 不能是非 str 类型
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: sequence item 0: expected str instance, int found
    >>>
    ```

- str.strip([_chars_])

    去除字符串首尾的在 _chars_ 中出现的字符。  
    如果 _chars_ 不指定，则默认去除空白字符。

    ```python
    >>> '   spacious   '.strip()
    'spacious'
    >>> 'www.example.com'.strip('cmowz.')
    'example'
    ```

    str.lstrip([_chars_]) 方法则只去除左边(首部)的字符。

    ```python
    >>> '   spacious   '.lstrip()
    'spacious   '
    >>> 'www.example.com'.lstrip('cmowz.')
    'example.com'
    ```

    str.rstrip([_chars_]) 与 lstrip 类似，只是去除右边(尾部)字符。

- str.split(_sep_=None, _maxsplit_=-1)

    用分隔符 _sep_ 对字符串进行最多 _maxsplit_ 次切割，返回切割后的 list 。  
    _sep_ 默认为空白字符，即空格、制表符等。  
    _maxsplit_ 如果不指定或指定为 -1，则会进行最大次数的切割。

    ```python
    # 指定 sep
    >>> '1,2,3'.split(',')
    ['1', '2', '3']
    >>> '1,2,3'.split(',', maxsplit=1)
    ['1', '2,3']
    >>> '1,2,,3,'.split(',')
    ['1', '2', '', '3', '']

    # 不指定 sep
    >>> '1 2 3'.split()
    ['1', '2', '3']
    >>> '1 2 3'.split(maxsplit=1)
    ['1', '2 3']
    >>> '   1   2   3   '.split()
    ['1', '2', '3']
    ```

- str.splitlines([_keepends_])

    把字符串按照行边界符(line boundaries)切割成多行，并以 list 形式返回。  
    如果 _keepends_ 为True，则返回的每行末尾仍然包含换行符，_keepends_ 默认为False。  
    符合行边界符(line boundaries)定义的字符如下：

    | Representation | Description |
    | -------------- | ----------- |
    | \n | Line Feed |
    | \r | Carriage Return | 
    | \r\n | Carriage Return + Line Feed |
    | \v or \x0b | Line Tabulation |
    | \f or \x0c | Form Feed |
    | \x1c | File Separator |
    | \x1d | Group Separator |
    | \x1e | Record Separator |
    | \x85 | Next Line (C1 Control Code) |
    | \u2028 | Line Separator |
    | \u2029 | Paragraph Separator |

    ```python
    >>> 'first line \nsecond line \r\nthird line'.splitlines()
    ['first line ', 'second line ', 'third line']
    >>> 'first line \nsecond line \r\nthird line'.splitlines(True)
    ['first line \n', 'second line \r\n', 'third line']
    ```

# 使用 % 格式化字符串

Python 中使用 % 格式化字符串的格式是 `format % values` ：

```python
>>> 'I am %s' % 'python'
'I am python'
>>>
```

如果 values 是多个值，那么需要使用表示 tuple 的圆括号括起来：

```python
>>> 'This is %s, that is %s' % ('python', 'java')
'This is python, that is java'
>>>
```

values 也可以是字典，如果是字典那么 format 里需要用 `映射键(mapping key)` 的方式取值：

```python
>>> 'I am %(lang)s' % {'lang': 'python'}
'I am python'
```

以上示例中的 `%s` 里的 s 表示转换类型为 `str`，Python 也支持很多其他类型，以下列出常见的几种：

- d/i : 有符号的十进制整数

    ```python
    >>> '1 + 1 = %d' % 2
    '1 + 1 = 2'
    >>> '1 + 1 = %i' % 2
    '1 + 1 = 2'
    ```

- o : 有符号的八进制整数

    ```python
    >>> 'decimal 9 convert to octal is %o' % 9
    'decimal 9 convert to octal is 11'
    ```

- x/X : 有符号16进制数

    ```python
    >>> 'decimal 17 convert to hex is %x' % 17
    'decimal 17 convert to hex is 11'
    ```

- f/F : 浮点数十进制格式

    ```python
    >>> '%f' % 3.14
    '3.140000'
    ```

- e/E : 浮点数指数格式

    ```python
    >>> '%e' % 31.4
    '3.140000e+01'
    ```

- c : 单个字符

    ```python
    >>> '%c' % 97
    'a'
    >>> '%c' % 'a'
    'a'
    ```

- r : 字符串（使用 repr() 转换任何 Python 对象）

    ```python
    >>> 'hello %r' % 'world'
    "hello 'world'"
    ```

- s : 字符串（使用 str() 转换任何 Python 对象）

    ```python
    >>> 'hello %s' % 'world'
    'hello world'
    ```

- a : 字符串（使用 ascii() 转换任何 Python 对象）

    ```python
    >>> 'hello %a' % 'world'
    "hello 'world'"
    ```

如果需要原样表示 % ，只需双写 % 即可：

```python
>>> '3 %% 2 = %d' % 1
'3 % 2 = 1'
```

可以指定占位宽度，如果实际内容小于宽度，默认右对齐，左边补空格：

```python
>>> '%3s' % 'a'
'  a'
>>> '%3d' % 10
' 10'
```

对于浮点数，可以使用 `.precision` 的格式指定指定精度：

```python
>>> '%.3f' % 3.1415
'3.142'
>>> '%.3f' % 3.14
'3.140'
```

可以使用 `转换标志(Conversion flags)` 对转换行为进行一些控制，支持的转换标志如下：

- '#' : 替代模式(alternate form)

    ```python
    # 八进制和十进制转换在替代模式下会显示 0o 和 0x/0X 前缀
    >>> '%o' % 9
    '11'
    >>> '%#o' % 9
    '0o11'
    >>> '%#x' % 17
    '0x11'
    >>> '%#X' % 17
    '0X11'

    # 浮点数转换在替代模式下会始终保留一个小数点在末尾，即使精度为0的整数
    >>> '%.0f' % 3.0
    '3'
    >>> '%#.0f' % 3.0
    '3.'
    ```

- '0' : 数值类型转换时多余位置补0

    ```python
    >>> '%03d' % 1
    '001'
    ```

- '-' : 转换值将靠左对齐（会覆盖 '0' 转换）

    ```python
    >>> '%-3d' % 1
    '1  '
    >>> '%0-3d' % 1  # '-' 转换会覆盖 '0' 转换
    '1  '
    ```

- ' ' : (空格) 正数前面保留一个空格

    ```python
    >>> '% d' % 1
    ' 1'
    >>> '% d' % -1
    '-1'
    ```

- '+' : 数值类型前面显示符号位（会覆盖 "空格" 转换）

    ```python
    >>> '%+d' % 1
    '+1'
    >>> '%+d' % -1
    '-1'
    ```

以上这些符号组合使用时需要遵守以下规则和顺序：

```
%[M][F][W][P][L]T

% : 固定格式。
M : 映射键(mapping key)（可选），由加圆括号的字符序列组成 (例如 (somename))，values 是字典的情况下使用。
F : 转换标志(conversion flags)（可选），用于影响某些转换类型的结果。
W : 最小字段宽度(width)（可选）。 如果指定为 '*' (星号)，则实际宽度会从 values 元组的下一元素中读取，要转换的对象则为最小字段宽度和可选的精度之后的元素。
P : 精度(precision)（可选），以在 '.' (点号) 之后加精度值的形式给出。 如果指定为 '*' (星号)，则实际精度会从 values 元组的下一元素中读取，要转换的对象则为精度之后的元素。
L : 长度修饰符(length modifier)（可选）。
T : 转换类型(conversion type)。

注：长度修饰符（h, l 或 L）可以使用，但是会被忽略，因为在 Python3 中只有长整型（L），没有短整型（h），所有 %ld 和 %d 是一样的。
```

下面是一些组合使用的示例：

```python
# 十进制浮点数转换类型(f)，带符号位(+)，左对齐(-)，最小宽度5(5)，精度1位(.1)
>>> '%+-5.1f' % 3.14
'+3.1 '
# 十进制整数转换类型(d)，带符号位(+)，最小宽度4(4)，不足位补0(0)
>>> '%+04d' % 16
'+016'
# 字符串转换类型(s)，左对齐(-)，最小宽度6(6)，精度2(.2)
>>> '%-6.2s' % 'Python'
'Py    '
```


# 使用 str.format 格式化字符串

从 Python3 开始官方推荐使用字符串自带的 format 方法对字符串进行格式化，该方法能够完全兼容 % 的形式且更强大，比如像下面这样：

```python
>>> 'hello {}!'.format('world')
'hello world!'
```

format 的语法定义如下：

```
replacement_field ::=  "{" [field_name] ["!" conversion] [":" format_spec] "}"
field_name        ::=  arg_name ("." attribute_name | "[" element_index "]")*
arg_name          ::=  [identifier | integer]
attribute_name    ::=  identifier
element_index     ::=  integer | index_string
index_string      ::=  <any source character except "]"> +
conversion        ::=  "r" | "s" | "a"
format_spec       ::=  <described in the next section>
```

下面通过举例的方式逐一对 format 语法定义中的字段进行解释：

- field_name/arg_name : 参数名（可选），可以是位置（integer）或名称（identifier）。

    ```python
    # arg_name 为空则按顺序依次引用后面的位置参数
    >>> '{} {} {}'.format('first', 'second', 'third')
    'first second third'

    # arg_name 为位置（integer），则按照索引号引用后面的位置参数
    >>> '{0} {1} {2}'.format('first', 'second', 'third')
    'first second third'
    >>> '{2} {1} {0}'.format('first', 'second', 'third')
    'third second first'
    >>> '{0} {0} {1}'.format('first', 'second')
    'first first second'

    # 但是空和位置引用不能混用
    >>> '{} {1} {2}'.format('first', 'second', 'third')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: cannot switch from automatic field numbering to manual field specification

    # arg_name 为名称（identifier），则按照名称引用后面的关键字参数
    >>> '{f1} {f2} {f3}'.format(f1='first', f2='second', f3='third')
    'first second third'

    # 空引用和名称引用混用，或者数字引用和名称引用混用是可以的
    >>> '{} {f2} {}'.format('first', 'third', f2='second')
    'first second third'
    >>> '{0} {f2} {1}'.format('first', 'third', f2='second')
    'first second third'
    ```

- field_name/arg_name.attribute_name : 属性名（可选），是对参数的属性进行引用。

    ```python
    >>> c = 3-5j
    >>> 'realpart = {0.real}, imagpart = {0.imag}'.format(c)
    'realpart = 3.0, imagpart = -5.0'
    >>> 'realpart = {cnum.real}, imagpart = {cnum.imag}'.format(cnum=c)
    'realpart = 3.0, imagpart = -5.0'
    ```

- field_name/arg_name[element_index] : 元素索引（可选），可以是数字或名称，通过索引的方式引用参数的元素。

    ```python
    # 数字索引
    >>> arr = [1, 2]
    >>> 'e1 = {0[0]}, e2 = {0[1]}'.format(arr)
    'e1 = 1, e2 = 2'

    # 名称索引
    >>> d = {'id': 1, 'name': 'tom'}
    >>> 'id = {0[id]}, name = {0[name]}'.format(d)
    'id = 1, name = tom'
    ```

- conversion : 转换标志，'!s' 会对值调用 str()，'!r' 调用 repr() 而 '!a' 则调用 ascii() 。

    ```python
    >>> 'hello {0!r}'.format('world')
    "hello 'world'"
    >>> 'hello {0!s}'.format('world')
    'hello world'
    >>> 'hello {0!a}'.format('world')
    "hello 'world'"
    ```

format_spec 的语法定义如下：

```
format_spec ::=  [[fill]align][sign][#][0][width][,][.precision][type]
fill        ::=  <any character>
align       ::=  "<" | ">" | "=" | "^"
sign        ::=  "+" | "-" | " "
width       ::=  integer
precision   ::=  integer
type        ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%"
```

下面通过举例的方式逐一对 format_spec 语法定义中的字段进行解释：

- width : 最小宽度

    ```python
    # 指定最小宽度为 10 位
    >>> '{:10}'.format('Python')
    'Python    '
    ```

- align : 对齐方式，支持 '<', '>', '^' 三种方式。

    ```python
    >>> '{:<30}'.format('left aligned')  # 最小宽度 30，并且左对齐
    'left aligned                  '
    >>> '{:>30}'.format('right aligned')  # 最小宽度 30，并且又对齐
    '                 right aligned'
    >>> '{:^30}'.format('centered')  # 最小宽度 30，并且居中对齐
    '           centered           '
    ```

- fill : 填充符

    ```python
    # 最小宽度 30，并且居中对齐，多余部分使用 * 填充
    >>> '{:*^30}'.format('centered')
    '***********centered***********'
    ```

- sign : 符号位，仅对数学类型有效，支持 '+', '-', ' ' 三种类型。

    ```python
    >>> '{:+}, {:+}'.format(1, -1)  # 始终保留符号位
    '+1, -1'
    >>> '{:-}, {:-}'.format(1, -1)  # 负数才有符号位
    '1, -1'
    >>> '{: }, {: }'.format(1, -1)  # 正数前保留一个空格
    ' 1, -1'
    ```

- precision : 精度

    ```python
    >>> '{:.2}'.format(3.14)
    '3.1'
    >>> '{:.2}'.format('Python')  # 作用在字符串上则会按照进行截断
    'Py'
    ```

- , : 千分位

    ```python
    >>> '{:,}'.format(12345678)
    '12,345,678'
    ```

- type : 转换类型

    转换类型大部分与 % 格式化的方式相同，下面只对不一样的类型进行示例说明：

    ```python
    >>> '{:b}'.format(7)  # 二进制类型
    '111'
    >>> '{:.2%}'.format(23/89)  # 百分比类型
    '25.84%'
    ```

- #/0 : # 和 0 的作用与 % 格式化的方式相同

# 引用资料

- [Strings] : https://docs.python.org/3.5/tutorial/introduction.html#strings
- [String Methods] : https://docs.python.org/3.5/library/stdtypes.html#string-methods
- [Format String Syntax] : https://docs.python.org/3.5/library/string.html#formatstrings
- [printf-style String Formatting] : https://docs.python.org/3.5/library/stdtypes.html#old-string-formatting

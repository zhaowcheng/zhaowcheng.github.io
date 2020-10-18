---
title: PostgreSQL入门教程-n-过程语言PLpgSQL
date: 2020-08-24 18:09:03 +0800
categories: [PostgreSQL入门教程]
tags: []
---

# 1 简介

PL/pgSQL 专为 PostgreSQL 设计的一种可加载 `过程语言`，它是设计目标包含以下几点：

- 能被用来创建函数(function)和触发器(trigger)
- 增加 `控制结构` 到 SQL 语言中
- 能够执行复杂计算
- 继承所有用户自定义的类型、函数和操作符
- 能被定义为受服务器信任的(trusted)
- 易于使用

使用 PL/pgSQL 定义的函数能够在任何能调用内置函数的地方被调用。比如可以先创建一个包含复杂条件计算的函数，然后用于操作符定义或者索引表达式中。在 PostgreSQL 9.0 及以后的版本中默认安装 PL/pgSQL，但是它仍然是一个可加载模块，所以管理员可以基于安全考虑而移除它。

## 1.1 PL/pgSQL 的优点

SQL 是 PostgreSQL 和其他大部分关系型数据库的查询语言，它可移植并且易于使用，但是每一条语句都必须被数据库服务器单独执行。这就意味着每一个查询都必须被先发送到服务器，然后等待被处理，然后接收返回结果，然后进行下一个查询。所有这些都需要通过网络进行通信。如果是用 PL/pgSQL 则可以把一些计算的步骤和查询语句组合成函数或存储过程并存储在数据库服务器上，这样既简化了 SQL 语句，更重要的是减少了客户端和服务器之间的通信：

- 客户端和服务器之间的额外往返通信被消除了
- 客户端不需要的中间结果不必被整理或者在服务器与客户端之间传送了
- 多次的往返查询被避免

这样相比不使用函数的应用，性能上得到了提升。  
PL/pgSQL 同样能使用所有 SQL 的数据类型、操作符和函数。

## 1.2 PL/pgSQL 支持的参数和返回类型

使用 PL/pgSQL 写的函数能接受任意服务端支持的标量或数组类型作为参数，也可以把这些类型作为返回结果。它也能接受和返回任何命名了的组合类型(行类型)，它也可以定义一个 PL/pgSQL 函数作为接受 record，这就意味着组合类型都将作为输入或返回 record，这也意味着返回结果是一个行类型，并且是在调用时指定返回哪些列。就像这里讨论的一样：[Table Functions](https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-TABLEFUNCTIONS)。

PL/pgSQL 函数可以使用 `VARIDIC` 标记定义可变数量的参数，它的工作方式和 SQL 函数一样，详情见 [SQL Functions with Variable Numbers of Arguments](https://www.postgresql.org/docs/current/xfunc-sql.html#XFUNC-SQL-VARIADIC-FUNCTIONS)。

PL/pgSQL函数也可以被声明为接受并返回多态类型anyelement、anyarray、anynonarray、anyenum以及anyrange。如第 [ Polymorphic Types](https://www.postgresql.org/docs/current/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC) 节中所讨论的，由一个多态函数处理的实际数据类型会随着调用改变。在第 [Declaring Function Parameters](https://www.postgresql.org/docs/current/plpgsql-declarations.html#PLPGSQL-DECLARATION-PARAMETERS) 节中展示了一个例子。

PL/pgSQL函数还能够被声明为返回一个任意（可作为一个单一实例返回的）数据类型的“集合”（或表）。这样的一个函数通过为结果集的每个期望元素执行RETURN NEXT来产生输出，或者通过使用RETURN QUERY来输出一个查询计算的结果。

最后，如果一个PL/pgSQL函数没有可用的返回值，它可以被声明为返回void（另外一种选择是，在那种情况下它可以被写作一个过程）。

PL/pgSQL函数也能够被声明为用输出参数代替返回类型的一个显式说明。这没有为该语言增加任何基础功能，但是它常常很方便，特别是对于要返回多个值的情况。RETURNS TABLE符号也可以被用来替代RETURNS SETOF。

在第 `42.3.1` 节和第 `42.6.1` 节中有详细的例子。
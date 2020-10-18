---
title: PostgreSQL入门教程-n-过程语言简介
date: 2020-08-21 15:58:12 +0800
categories: [PostgreSQL入门教程]
tags: []
---

# 简介

PostgreSQL 允许用户使用 SQL 和 C 以外的语言来自定义 `函数(function)`。通常把这些其他语言统称为 `过程语言(procedural languages(PLs))`。对于使用过程语言编写的函数，数据库服务器并没有内置的方法来解释它，而是把它传递给一个能够解释这种过程语言的 `handler` 进行处理。handler 可以自己对使用过程语言编写的函数进行解析、语法分析和执行等，也可以只是作为一个中介，把过程语言函数交给数据库服务器上已安装的过程语言解释器进行处理。handler 是使用 C 语言编写的并且编译为共享库供数据库服务器加载使用。

PostgreSQL 原生支持 4 种过程语言：PL/pgSQL、PL/Tcl、PL/Perl 和 PL/Python。另外也有一些非 PG 社区开发的对其他过程语言的支持的扩展包，详情见 [Procedural Languages](https://www.postgresql.org/docs/current/external-pl.html)。除此之外，用户还可以自定义过程语言，如何自定义过程语言可以参考 [Writing a Procedural Language Handler](https://www.postgresql.org/docs/current/plhandler.html)

# 安装

数据库服务器中的每个 `数据库` 要使用过程语言都需要先安装。如果模板数据库 `template1` 中安装了过程语言，那么所有使用 `CREATE DATABASE` 语句创建的新数据库都会自动支持过程语言，因为 `CREATE DATABASE` 会自动拷贝 `template1` 作为新数据库。通过这个特性，数据库管理员可以灵活配置哪些数据库支持指定类型的过程语言。

对于 PostgreSQL 原生支持的过程语言，只需要在对应数据库上使用 `CREATE EXTENSION language_name` 语句安装后即可使用，所以就不再赘述，下面主要讲解一下如何安装非原生支持的过程语言扩展包。

## 扩展过程语言安装步骤

过程语言扩展包的安装包含 5 个步骤，安装过程必须使用 `超级用户`，并且应该将安装过程中所需用到的 SQL 语句打包成脚本以便 `CREATE EXTENSION` 语句调用。

1. 首先应该安装过程语言的 handler 共享库，安装方式与安装用户使用 C 语言编写的自定义函数一样，详情见 [Compiling and Linking Dynamically-Loaded Functions](https://www.postgresql.org/docs/current/xfunc-c.html#DFUNC)。另外还应该安装好过程语言解释器供 handler 调用。

2. 使用以下语句格式声明 hanlder：

    ```sql
    CREATE FUNCTION handler_function_name()
        RETURNS language_handler
        AS 'path-to-shared-object'
        LANGUAGE C;
    ```
    返回类型 `language_handler` 表示这不是普通的数据类型，所以不能被用于普通的 SQL 语句中。

3. [可选]：如果安装了 `内联(inline)` handler 的共享库，可以创建内联 handler 函数用于执行匿名代码块([DO 命令](https://www.postgresql.org/docs/current/sql-do.html))，声明语句如下：

    ```sql
    CREATE FUNCTION inline_function_name(internal)
        RETURNS void
        AS 'path-to-shared-object'
        LANGUAGE C;
    ```

4. [可选]：如果安装了 `验证器(validator)` handler 的共享库，可以创建验证器函数用于在不执行过程语言函数的方式下检查过程语言的定义是否正确，声明语句如下：

    ```sql
    CREATE FUNCTION validator_function_name(oid)
        RETURNS void
        AS 'path-to-shared-object'
        LANGUAGE C STRICT;
    ```

5. 最后使用如下语句声明过程语言(PL)：

    ```sql
    CREATE [TRUSTED] LANGUAGE language_name
        HANDLER handler_function_name
        [INLINE inline_function_name]
        [VALIDATOR validator_function_name] ;
    ```
    可选参数 `TRUSTED` 表示不允许用户访问它本身不拥有的数据。受信任的过程语言是设计来允许数据库 `普通用户(非超级用户)` 安全地创建存储过程和函数的。由于存储过程函数是在数据库服务器内部被执行的，所以 `TRUSTED` 应该只被用于不允许访问数据库内部和文件系统的过程语言。PL/pgSQL、PL/Tcl 和 PL/Perl 可以是 TRUSTED，但是 PL/TclU、PL/PerlU 和 PL/PythonU 是被设计来提供无限制功能的，所以不应该被标记为 TRUSTED。

## PL/Perl 安装示例

首先使用如下语句声明 PL/Perl 的调用处理器(handler)函数：

```sql
CREATE FUNCTION plperl_call_handler()
    RETURNS language_handler
    AS '$libdir/plperl'
    LANGUAGE C;
```

由于 PL/Perl 提供了 `内联处理器函数(inline handler function)` 和 `验证器函数(validator function)`，所以还可以作如下声明：

```sql
CREATE FUNCTION plperl_inline_handler(internal)
    RETURNS void
    AS '$libdir/plperl'
    LANGUAGE C;

CREATE FUNCTION plperl_validator(oid)
    RETURN void
    AS '$libdir/plperl'
    LANGUAGE C STRICT;
```

最后声明 plperl 语言

```sql
CREATE TRUSTED LANGUAGE plperl
    HANDLER plperl_call_handler
    INLINE plperl_inline_handler
    VALIDATOR plperl_validator;
```

PostgreSQL 默认编译 PL/pgSQL 并且安装到 `'library'` 目录下，另外，PL/pgSQL 还会默认安装到所有的数据库。如果在编译的 configure 阶段添加了支持 Tcl 语言的参数，那么会编译安装 PL/Tcl 和 PL/TclU 的处理器(handler)共享库到 library 目录下，但是这些过程语言默认不会安装到任意一个数据库，只有使用 `CREATE EXTENSION` 安装。同样的，如果编译时添加了支持 Perl 语言的参数，则会编译安装 PL/Perl 和 PL/PerlU 的处理器共享库到 library 目录下。如果编译时添加了支持 Python 语言的参数，则会编译安装 PL/PythonU 的处理器共享库到 library 目录下。
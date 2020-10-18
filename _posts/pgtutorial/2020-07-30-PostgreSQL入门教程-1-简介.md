---
title: PostgreSQL入门教程-1-简介
date: 2020-07-30 16:27:49 +0800
categories: [PostgreSQL入门教程]
tags: []
---

# 简介

PostgreSQL 是以美国加州大学伯克利分校计算机系开发的 POSTGRES 4.2 为基础的 `对象关系型数据库管理系统(ORDBMS: object-releational database management system)`。  
POSTGRES 提出了很多领先的概念，这些概念直到很多年后才在商业数据库系统中出现。

PostgreSQL 是最初的伯克利代码的开源继承者，它除了支持大部分的 SQL 标准以外，还支持以下这些的现代特性：

- 复杂查询
- 外键
- 触发器
- 可更新视图
- 事务完整性
- 多版本并发控制

PostgreSQL 还支持让用户通过增加以下类型进行扩展：

- 数据类型
- 函数
- 操作符
- 汇聚函数
- 索引方法
- 过程语言

PostgreSQL 的 license 允许任何人居于任何目的自由使用、修改和分发，并且在修改后还可以不用再次开源。

# 简史

现在为人们所熟知的对象关系型数据库管理系统 PostgreSQL 是派生自加州大学伯克利分校的包。经过二十多年的发展，在此期间主要有 2 个阶段，分别是 Postgres95 和 PostgreSQL，目前 PostgreSQL 是世界上可获得的最先进的开源数据库。

## POSTGRES

POSTGRES 项目由 `Michael Stonebraker` 教授主导，受到了 `美国国防部高级研究计划局(DARPA: Defense Advanced Research Projects Agency)`、`美国陆军研究办公室(ARO: Army Research Office)`、`美国国家科学基金会(NSF: National Science Foundation)` 和 `ESL, Inc` 的资助，开始于 1986 年。该系统最初的概念见 [The design of POSTGRES](http://db.cs.berkeley.edu/papers/ERL-M85-95.pdf)，最初的数据模型定义见 [The POSTGRES data model](http://db.cs.berkeley.edu/papers/ERL-M87-13.pdf)，当时的系统规则设计见 [The design of the POSTGRES rules system(已废弃)](https://www.postgresql.org/docs/12/biblio.html#STON87A)，存储管理器的理论基础和架构见 [The design of the POSTGRES storage system](http://db.cs.berkeley.edu/papers/ERL-M87-06.pdf)。

POSTGRES 的开发经历了几个大的阶段。第一个 demo 程序是在 1987 年实现，然后在 1988 年 ACM-SIGMOD 大会上展示。然后在 1989 年 6 月实现了第一个版本并且对外释放给了少量用户使用，第一个版本的实现详情见 [The implementation of POSTGRES](http://db.cs.berkeley.edu/papers/ERL-M90-34.pdf)。为了回应用户对规则系统的[批评](https://www.postgresql.org/docs/current/biblio.html#STON89)，规则系统被重新进行了[设计](https://www.postgresql.org/docs/current/biblio.html#STON90B)，然后在 1990 年 6 月发布了第二个版本，并且在该版本中使用了新设计的规则系统。然后在 1991 年发布了第三个版本，并且在该版本中新增了支持多存储管理器的功能，并且改善了查询执行器、重写了规则系统。然后在接下来就是主要专注于可移植性和可靠性的提升，一直到 Postgres95。

POSTGRES 已被用于多种研究项目和产品。包括财务数据分析系统、喷气式引擎性能监控、小行星跟踪数据库、医药信息数据库和地理信息系统等。POSTGRES 也在几所大学被用作教学软件。Illustra Information Technologies(后来合并到 Informix，现在 Infomix 被合并到 IBM) 使用 POSTGRES 源码进行了商业化。到 1992 年末，POSTGRES 成为了 [Sequoia 2000 scientific computing project](http://meteora.ucsd.edu/s2k/s2k_home.html) 的主要数据管理器。

社区用户的数量在 1993 年几乎增加到了原来的 2 倍。由于在源码维护上面所需的时间日益增加而减少了数据库研究的时间。为了减少支持的负担，伯克利大学在 4.2 版本时正式停止了 POSTGRES 项目。

## Postgres95

在 1994 年，`Andrew Yu` 和 `Jolly Chen` 在 POSTGRES 基础上增加了 `SQL 语言解释器`，并且重新命名为 `Postgres95`，然后以开源的方式发布到网上。

Postgres95 的代码全部是 `ANSI C`，并且体积减少了 25%。对内部进行了许多修改，提高了性能和可维护性。使用 `Wisconsin Benchmark` 工具对比测试发现，Postgres95 1.0.x 的速度比 POSTGRES 4.2 要快 30%~50%。除了 bug 修复外，Postgres95 主要做了以下方面的增强：

- 把 PostQUEL 查询语言替换为 SQL 查询语言（在服务端实现）。（接口库 libpq 就是取 PostQUEL 的缩写 pq 来命名的）。在 PostgreSQL 之前还不支持子查询，但是在 Postgres95 中可以通过用户自定义 SQL 函数实现模拟子查询的效果。汇聚函数进行了重写。同时还新增了对 GROUP BY 查询子句的支持。

- 新增了一个客户端程序 psql 用于交互式 SQL 查询，psql 使用了 GNU Readline，psql 在很大程度上取代了老的 monitor 程序。

- 新增了一个前端库 libpgtcl 用于支持基于 Tcl 的客户端。新增了一个 pgtclsh，这个 shell 新增了一些 Tcl 命令，用于支持 Tcl 程序和 Postgres95 服务端进行交互。

- 彻底重写了大对象接口，保留了大对象倒转（inversion）作为存储大对象的唯一机制（移除了倒转(inversion)文件系统）。

- 移除了实例级规则系统，但是保留了重写规则。

- 随源码一起发布一个介绍常规 SQL 特性和 Postgres95 的简短的教程。

- 使用 GNU make 替代 BSD make 用于编译。Postgres95 也可以使用不打补丁的 GCC 进行编译。（修正了双精度数据对齐问题）

## PostgreSQL

到了 1996 年，Postgres95 这个名字就显得有些不合时宜了。所以社区就取了一个新的名字，PostgreSQL，这个名字可以反映出在 POSTGRES 的基础上添加了 SQL 的情况。同时延续了伯克利 POSTGRES 项目的版本号，设置 PostgreSQL 的版本号从 6.0 开始。

人们习惯于把 PostgreSQL 叫做 Postgres，可能是由于历史原因或者比较容易发音，所以 Postgres 这个名字也被当作昵称或别名而被广泛接受。

在 Postgres95 开发期间的重点工作还是在解决服务端代码的问题。到了 PostgreSQL 开发期间，虽然各个方面都在做，但是最主要的工作还是 `开发新功能` 和 `增加容量`。

想要了解从这以后 PostgreSQL 发生了什么，可以查看 [Release Notes](https://www.postgresql.org/docs/12/release.html)

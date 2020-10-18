---
title: PostgreSQL入门教程-2-安装
date: 2020-08-16 16:54:09 +0800
categories: [pgtutorial]
tags: []
---

# 源码安装

本章讲解如何使用源码进行编译安装，包括在 `Unix-like` 和 `Windows` 平台上。如果下载的是 `rpm` 或者 `deb` 之类的包，则使用对应类型的包的安装方式安装即可。

## Unix-like 平台

在 Unix-like 平台，如果依赖环境满足，并且只需按照默认方式安装的话(比如在一个之前已经编译安装过的系统上)，可以参以下步骤进行快速安装：

```sh
./configure
make
su
make install
adduser postgres
mkdir /usr/local/pgsql/data
chown postgres /usr/local/pgsql/data
su - postgres
/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data
/usr/local/pgsql/bin/pg_ctl -D /usr/local/pgsql/data -l logfile start
/usr/local/pgsql/bin/createdb test
/usr/local/pgsql/bin/psql test
```

但是如果是一个新安装的系统，或者一个之前没有编译安装过 PostgreSQL 的系统上，则请参考后面的章节步骤进行安装。

### 依赖检查

以下为必须软件：

- GNU make 3.80 及以上版本，可以使用 `make --version` 命令查看make 版本号，有的平台上 make 的程序名为 `gmake`。
- 

## Windows 平台

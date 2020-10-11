---
title: pg_ctl 重启相对路径启动的数据库报 'No such file or directory' 错误
date: 2020-07-30 12:00:00 +0800
categories: []
tags: []
---

# 环境信息

```
操作系统版本：CentOS Linux release 7.6.1810 (Core)
操作系统用户：postgres
PG版本: PostgreSQL 13devel
PG安装路径：/home/postgres/pgdb/latest
PGHOME环境变量：/home/postgres/pgdb/latest
PGDATA环境变量：/home/postgres/pgdb/latest/data
```

# 问题复现

使用 postgres 用户切换到 PG 安装路径，并且使用 -D 加相对路径启动数据库

```sh
[postgres@LAPTOP-H448I3TO latest]$ cd /home/postgres/pgdb/latest/
[postgres@LAPTOP-H448I3TO latest]$ pg_ctl start -D ./data/
waiting for server to start....2020-07-30 16:57:04.896 CST [6624] LOG:  starting PostgreSQL 13devel on x86_64-pc-linux-gnu, compiled by gcc (GCC) 4.8.5 20150623 (Red Hat 4.8.5-39), 64-bit
2020-07-30 16:57:04.897 CST [6624] LOG:  listening on IPv4 address "127.0.0.1", port 5432
2020-07-30 16:57:04.905 CST [6624] LOG:  listening on Unix socket "/tmp/.s.PGSQL.5432"
2020-07-30 16:57:04.911 CST [6625] LOG:  database system was shut down at 2020-07-30 16:56:46 CST
2020-07-30 16:57:04.915 CST [6624] LOG:  database system is ready to accept connections
 done
server started
[postgres@LAPTOP-H448I3TO latest]$
```

然后切换到 postgres 的 HOME 目录，使用 `pg_ctl restart` 不加参数重启数据库（不加参数默认将使用 $PGDATA 环境变量）

```sh
[postgres@LAPTOP-H448I3TO latest]$ cd ~
[postgres@LAPTOP-H448I3TO ~]$ pg_ctl restart
waiting for server to shut down....2020-07-30 17:00:17.714 CST [6624] LOG:  received fast shutdown request
2020-07-30 17:00:17.717 CST [6624] LOG:  aborting any active transactions
2020-07-30 17:00:17.720 CST [6624] LOG:  background worker "logical replication launcher" (PID 6631) exited with exit code 1
2020-07-30 17:00:17.720 CST [6626] LOG:  shutting down
2020-07-30 17:00:17.738 CST [6624] LOG:  database system is shut down
 done
server stopped
waiting for server to start....postgres: could not access directory "/home/postgres/./data": No such file or directory
Run initdb or pg_basebackup to initialize a PostgreSQL data directory.
 stopped waiting
pg_ctl: could not start server
Examine the log output.
[postgres@LAPTOP-H448I3TO ~]$
```

然后就出现了 `could not access directory "/home/postgres/./data": No such file or directory` 的错误。为什么为出现这个错误呢，按照理解不是应该默认使用 `PGDATA` 作为数据库目录去启动吗，难道是 PGDATA 配置错了？查看 PGDATA 环境变量如下：

```sh
[postgres@LAPTOP-H448I3TO ~]$ echo $PGDATA
/home/postgres/pgdb/latest/data
```

可以看到环境变量 PGDATA 配置是正确。出现这个问题是由于 pg_ctl restart 的机制导致的，下面将详细分析原因。

# 原因分析

之所以会出现以上问题，是由于 `pg_ctl restart` 的重启的两个阶段 `stop` 和 `start` 使用的 data 目录不一致所导致的问题。可以看到上面重启的 stop 阶段是成功的，只是到了 start 阶段报错了，这是由于在 stop 时用 PGDATA 环境变量当作 data 目录，PGDATA 的配置也是正确的，所以 stop 能够成功。但是在 start 阶段时，会使用 data 目录（其实也是 PGDATA）下的 `postmaster.opts` 文件中记录的上一次启动数据库时的参数去 start。我们可以查看 postmaster.opts 内容如下：

```sh
[postgres@LAPTOP-H448I3TO ~]$ cd $PGDATA
[postgres@LAPTOP-H448I3TO data]$ cat postmaster.opts
/home/postgres/pgdb/latest/bin/postgres "-D" "./data"
[postgres@LAPTOP-H448I3TO data]$
```

可以看到 postmaster.opts 中记录了上一次使用的 -D 参数，restart 中的 start 阶段使用了这个参数，而在执行 restart 命令时的当前目录为 `/home/postgres` 目录，使用这个相对路径就会导致找不到真正的 data 目录，所以出现了 'No such file or directory' 的错误。

# 问题规避

这个问题早在 2011 年就有人在 PG 社区中提出，一开始的解决办法是 `Bruce Momjian` 在 PG 手册中 `pg_ctl` 那一章添加了说明：

> If relative paths were used on the command line during server start, restart might fail unless pg_ctl is executed in the same current directory as it was during server start.

后来这个问题的提出者还自己提供了修改补丁，并且有人对代码进行了 review，但是最终却没有应用这个补丁，并且最后一次关于那个补丁的邮件是 Momjian 问了一下 `Where are we on this patch?`，一直也没人回复，所以这个补丁的事情也有可能是无人跟踪就遗漏了。

讨论邮件：https://www.postgresql.org/message-id/flat/CAK3UJRFK8-izAU1SMpNZr5Muc%2B6CRWBk0_7ByJnwUoapj3esFQ%40mail.gmail.com

所以为了规避这个问题，还是建议启动时不要使用相对路径。
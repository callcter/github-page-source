---
title: MacOS 升级 MySQL
---

今天青岛大雾！

#### 报错

一早来到公司，启动mysql服务，结果给我来了这么一句

> ERROR! The server quit without updating PID file (/usr/local/var/mysql/liuhangdeMacBook-Pro.local.pid).

额，O__O "…

<!-- more -->

执行

```
mysqld start
```

返回了详细的报错：

> 2019-06-20T01:41:59.982830Z 0 [System] [MY-010116] [Server] /usr/local/Cellar/mysql/8.0.15/bin/mysqld (mysqld 8.0.15) starting as process 4707
> 2019-06-20T01:41:59.986135Z 0 [Warning] [MY-010159] [Server] Setting lower_case_table_names=2 because file system for /usr/local/var/mysql/ is case insensitive
> 2019-06-20T01:42:00.069561Z 1 [ERROR] [MY-012526] [InnoDB] Upgrade after a crash is not supported. This redo log was created with MySQL 5.7.18. Please follow the instructions at http://dev.mysql.com/doc/refman/8.0/en/upgrading.html
> 2019-06-20T01:42:00.069583Z 1 [ERROR] [MY-012930] [InnoDB] Plugin initialization aborted with error Generic error.
> 2019-06-20T01:42:00.379422Z 1 [ERROR] [MY-011013] [Server] Failed to initialize DD Storage Engine.
> 2019-06-20T01:42:00.380010Z 0 [ERROR] [MY-010020] [Server] Data Dictionary initialization failed.
> 2019-06-20T01:42:00.380249Z 0 [ERROR] [MY-010119] [Server] Aborting
> 2019-06-20T01:42:00.381714Z 0 [System] [MY-010910] [Server] /usr/local/Cellar/mysql/8.0.15/bin/mysqld: Shutdown complete (mysqld 8.0.15)  Homebrew.

意思是说之前 mysql 升级没处理好，遗留文件导致新版本的启动不了。

查了网上的各种方法，最后只能选择重装，然后手动恢复数据。

/(ㄒoㄒ)/~~

#### 重装

# ！！！一定要先备份数据，重装会自动删除原来目录下的所有文件

```
brew uninstall --force mysql // 删除原来的MySQL
brew services list
brew doctor // 确认删除并修复
mv /usr/local/var/mysql /usr/local/var/old.mysql // 将数据文件备份，数据库文件以.frm .idb的格式在/usr/local/var/mysql下
brew install mysql // 重新安装

mysql.server start // 启动
mysql_secure_installation // 安全安装，mysql8版本更改了安装规则，需要配置很复杂的密码
```
然后根据提示配置就可以了

#### 数据恢复

在终端 mysql -uroot -p 可以正常登录，但是用 sequel pro 连接却报错：

> Authentication plugin 'caching_sha2_password' cannot be loaded: dlopen(/usr/local/mysql/lib/plugin/caching_sha2_password.so, 2): image not found

？？？

在 mysql 下执行

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

然后就可以使用 sequel pro 连接了。

最痛苦的是恢复数据，以后一定要记得 dump 数据 /(ㄒoㄒ)/~~

网上更有多种数据恢复的方式，需要根据所能提供的文件选择不同的方式。

开始我选择用 mysql-utilities 的方式，这个工具集提供 mysqlfrm 指令，可以提取表结构。表结构提取没问题，但是后续的操作始终有问题。还有一种恢复方式，新建同名表，然后旧的 frm 文件覆盖新的，使用恢复模式强行恢复表结构。这种方式我在一开始就否定了，我这里的数据库跟表太多了，这不知要忙活到何年何月。

再回过头去看看数据库打开失败的原因，是旧版本的遗留文件导致打不开，那如果我恢复到旧的版本，是否就可以了呢。我之前的版本是 5.7.18，使用 brew 重新安装 5.7 版本。

```
brew info mysql // 查看 brew 上面 mysql 的信息，可以看到 mysql@5.7
brew uninstall --force mysql // 卸载当前版本
brew install mysql@5.7 // 下载安装 5.7 版本，当前是 5.7.25
```

安装完毕后，直接将 old.mysql 目录 覆盖掉 /usr/local/var/mysql，启动服务，连接 mysql，OK！

O(∩_∩)O~~

然后赶紧 mysqldump 导出数据，卸载 mysql@5.7，安装最新版本。

#### 总结

还是缺乏经验，开始看到报错就如无头苍蝇到处查各种试，花费了很多时间，如果开始就静下心来去看看mysql的数据管理方式，也许就不会这样了，以此为戒！

#### 参考文章

- [Cannot start mysql: InnoDB: Upgrade after a crash is not supported](https://superuser.com/questions/1332974/cannot-start-mysql-innodb-upgrade-after-a-crash-is-not-supported)
- [（解决方法）MySQL 8 + macOS 错误：Authentication plugin 'caching_sha2_password' cannot be loaded](https://1c7.me/mysql-8-connect-error-2018/)
title: MySQL配置远程访问
date: 2015-11-01 17:16:56
categories: 数据库
tags: MySQL
---

> 默认安装的MySQL只允许本地连接(localhost 或 127.0.0.1)，因此如果需要远程操作数据库，例如用数据库工具操作数据库的时候就应该授权。

授权有几种方法，把下面password换成自己的密码, 0.0.0.0换成自己的IP

<!--more-->

* 允许root用户在任何IP进行远程登录，并具有所有库的所有操作权限:

	`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'IDENTIFIED BY 'password' WITH GRANT OPTION;`
	
* 允许root用户在一个特定的IP进行远程登录，并具有所有库的所有操作权限:

	`GRANT ALL PRIVILEGES ON *.* TO root@"0.0.0.0" IDENTIFIED BY "password" WITH GRANT OPTION;`

* 允许root用户在一个特定的IP进行远程登录，并具有所有库的特定操作权限:

	`GRANT select，insert，update，delete ON *.* TO root@"0.0.0.0" IDENTIFIED BY "password";`
	
最后刷新一下: `flush privileges;`
	
另外，在Ubuntu下还要修改一下配置文件/etc/mysql/my.cnf，找到bind-address = 127.0.0.1注释掉。







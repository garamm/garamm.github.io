---
layout: post
title: "[MySQL] MySQL Error"
category: mysql
date: "2019-06-05"
---

ERROR 2002 (HY000): Can’t connect to local MySQL server through socket ‘/var/run/mysqld/mysqld.sock’ (2)
-------------
> ln -s /var/lib/mysql/mysql.sock /tmp/mysql.sock
> service mysql start or service mysql start

ERROR 1045(28000): Access denied for user 'root'@'localhost'(using password: YES)
-------------
$ sudo service mysql start
$ cd /var/run
$ sudo cp -rp ./mysqld ./mysqld.bak
$ sudo service mysql stop
$ sudo mv ./mysqld.bak ./mysqld
$ sudo mysqld_safe --skip-grant-tables --skip-networking &
$ mysql -u root
 -- mysql 접속 후 --
> FLUSH PRIVILEGES;
> SET PASSWORD FOR root@'localhost' = PASSWORD('new_pwd');
[출처](https://stackoverflow.com/questions/41984956/cant-reset-root-password-with-skip-grant-tables-on-ubuntu-16/41987711)
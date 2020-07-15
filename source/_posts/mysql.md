---
title: mysql
date: 2020-07-15 17:08:22
tags: mysql
---

---

title: mysql
date: 2020-07-15 15:24:36
tags: mysql

---

安装 mysql

```
yum install mysql-server
```

查看版本

```
mysql -V
```

运行 mysql

```
service mysqld start
```

查看是否启动

```
service mysqld status
```

获取初始密码

```
grep "password" /var/log/mysqld.log
```

登陆 mysql

```
mysql -uroot -p'********'
```

设置密码（“需要带数字，大写字母，小写字母，特殊符号”）如我设置密码为 Qc123456!

```
SET PASSWORD = PASSWORD('你的新密码');
```

设置密码永不过期

```
ALTER USER 'root'@'localhost' PASSWORD EXPIRE NEVER;
```

刷新系统权限相关表

```
flush privileges;
```

设置数据库用户在所有 ip 下以及在本地可访问,以下用 root 用户做演示

```
grant all privileges on *.* to root@"%" identified by "你的密码";
grant all privileges on *.* to root@"localhost" identified by "你的密码";
flush privileges;
```

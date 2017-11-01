title: database_mysql
date: 2015-12-18 00:17:35
tags: database mysql
---

mysql 用户管理和授权
创建user
CREATE USER 'username'@'localhost'IDENTIFIED BY 'password';
以上是创建一个用户　用户名为　username 密码为　password ;

授权给用户
GRANT ALL PRIVILEGES ON dbname.* TO 'username'@'localhost' WITH GRANT OPTION;
授权用户名为 username 的用户，拥有对数据库名为dbname的数据库下的所有table的增删改查的权限
FLUSH PRIVILEGES;
刷新　授权以使授权生效

删除已经授权的用户
DROP USER 'username'@'localhost';
FLUSH PRIVILEGES

注意在mysql的命令行中‘；’不能省略



mysql_common_command
1.创建数据库　

CREATE DATABASE dbname;

2.创建表格

CREATE TABLE tablename ( id INTEGER 0 AUTO_INCREMENT, clsid INTEGER 0 REFERENCE tablename1(id), PRIMARY KEY(id) );

3.查询

SELECT * FROM dbname.tablename WHERE columnname LIKE '%s%';

SELECT * FROM dbname.tablename WHERE colunname REGEXP '^s*';

SELECT * FROM dbname.tablename WHERE name=‘徐建华’ AND age>24;

SELECT * FROM dbname.tablename WHERE age>23 ORDER BY age DESC；

4.描述表格

DESCRIBE tablename；

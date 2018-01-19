title: mysql_study_2
date: 2016-12-20 16:45:53
tags: database_mysql
---
```
1. 数据类型
a. 字符类型
  char(8) 8个字节 ‘a’： 8，’ab'：8
  varchar 可变的字节数根据内容‘a’：2，’ab':3
b. 时间类型
  year: year（2)17,16； year（4)2017,1917；
  date datetime timestamp time 是有关联的，它们的区别联系如下：
  1>date “YYYY-MM-DD” 1000-01-01 to 9999-12-31
    datetime "YYYY-MM-DD HH:MM:SS"
    time: 'HH:MM:SS' 23:59:59
    timestamp:可以表示日期和时刻 范围为 1970-01-01 00:00:00 2038-01-19 03:14：07 （UTC)
  2>timestamp 会把当前时区的时间转成UTC 格林时间进行存储。
```

```
2.mysql 常用命令
1> create  table if not exists tablename (
  id int not null auto_increment ,
  name char (18) not null ,
  age tinyint default 0,
  insert_time timestamp default current_timestamp on update current_timestamp
  primary key (id)
  );
  2>
  drop table tablename;
```

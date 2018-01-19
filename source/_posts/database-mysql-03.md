title: database-mysql-study-03
date: 2017-01-16 11:20:18
tags: database_mysql_table
---
```
table
1. create table
 create table if not exists t_name (
    id tinyint not null auto_increment,
    name char(18) not null,
    part_id int not null default 1 references t_name_2(column),
    priamry key (id)
    ) engine = innodb default charset =utf8 select colum_1, colum_2 from t_name_1;

2.modify table 
    1> alert table t_name add coumn;
    2> alert table t_name change column_old_name column_new_name tinyint not null;
    3> alert table t_name add primary key (column_name);
    4> alert table t_name add foreign key (column_name) references t_name_1 (column_name);
    4>alert table t_name drop column_name;
    5>alert table t_name drop primary key (column_name);
    6>alert table t_name drop references foreign key (fk_symbol);
3.drop table
    drop table if exists t_name;

4. query table
    1>select * from t_name;
    2>select column_name ,column_name1 from t_name;
    3>select top 3 * from t_name join t_name_1 on t1.column=t2.column.column;
    4>select distinct column_name from t_name ;
5 create index
    create index index_name on t_name (column_name);
```

```
 partition
1> create table if not exists  t_name(
    id int not null auto_increment,
    name char(18) not null,
    age tinyint not null,
    gender enum ('man','woman') not null uique,
    part_id int constraint fk_symbol references t_name_1 (column_name1,column_name_2),
    primary key (id),
    check(id>0),
    )engine= innodb default charater set utf8 
    partition by range columns(id)(
        partition p0 values less than (10),
        partition p1 values less than (20)
    );
    

2> partitions type
    a)range 
        partition by range columns(id)(
            partition p0 values less than (10),
            partition p1 values less than (20)
        );
    b)key
        partition by key ()
        partitions 2;
    c)hash
    d)list
        partition by list columns (id)(
            partition p0 values in (1,2,3,4,5),
            partition p1 values in (6,7,8)
        );


```

```
database
 1>create database if not exists db_name default character set utf8;
 2> drop database if exists db_name;
```

---
title: mysql-orm
date: 2018-04-08 10:43:05
tags: mysql_beego_orm
---
```
 1. type Student struct{
        id  int64 
        teacher []*Teacher `orm:"rel(m2m)"`
        father *Father `orm:"rel(fk)"`
        wife *Wife `orm:"rel(one)"`
    }
    type Teacher struct{
        id int64
        student []*Student `orm:"reverse(many)"`
    }
    type Father struct{
        id int64
        student []*Student `orm:"reverse(many)"`
    }
    type Wife struct{
        id int64
        studnet *Student `orm:"reverse(one)"`
    }
 2. 一对多 rel(fk) reverse(many)
    一对一 rel(one) reverse(one)
    多对多 rel(m2m) reverse(many)
```

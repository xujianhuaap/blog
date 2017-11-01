title: database_mysql_04
date: 2017-08-13 20:24:39
tags: mysql
---
```
1. 联表查询
select c_name_1,c_name_2 from t_name_1 left join t_name_2 on [join_condition]
显性连接
1>内部连接　inner join
  必须两张表中，记录都存在
2>外部连接　
  left join ; 左表记录都显示
  right join；右表记录都显示

select * from t_name_1,t_name_2 where condition;
这个属于隐形连接查询，不建议再使用，更建议使用显性连接查询。

2. limit
select Salary As SecondHighestSalary from Employee order by Salary DESC limit 1,1;
limit ６ 表示记录的前６行
limit ５，３　表示记录第六行开始，往后３行，

３．　ifnull 函数
ifnull(exp1,exp2); 如果　exp1　不为NULL return exp1;否则,return　exp2;

SELECT
    IFNULL(
      (SELECT DISTINCT Salary
       FROM Employee
       ORDER BY Salary DESC
        LIMIT 1 OFFSET 1),
    NULL)
 AS SecondHighestSalary

```

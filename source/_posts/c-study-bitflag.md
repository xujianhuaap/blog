---
title: c_study_bitflag
date: 2017-11-03 18:58:01
tags: c_bitflag
---
```
1. bitflag 
	基于位运算，达到我们的目的，
2. example
	int flag = 0;
	int flag_1 = 1;
	int flag_2 = 1 << 1;
	int flag_3 = 1 << 2;
	int flag_4 = 1 << 3;
	1> 判断flag是否具有flag_1 属性
		flag & flag_1 != 0 表示具有flag_1;
	2> 判读flag是否具有flag_1 和floag_2 的属性
		flag & (flag_1 | flag_2) != 0 表示具有二者的属性；
	3> 赋值给flag
		flag | =( flag_2 | flag_1)
	4> 关闭某个属性
		flag &= ~flag_1;
	5> 翻转某个属性
		flag^= flag_1;
```

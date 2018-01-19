---
title: java-study-class
date: 2018-01-15 09:42:20
tags: java jvm class_struct
---
```
1. class的结构组成
	1> class 是字节流.可以是class 文件,也可以是从网络直接获取的字节流(jsp).
	jvm 支持的class字节流,可以是java 编译而来,也可以是Groovy的动态语言,
	编译而来.
	2> class字节流由无符号数和表组成.表是由若干无符号数和若干表组成.
	无符号数具体表示数字,索引值,数值. 表以_info结尾.
	3> class 结构
	u4 (四个字节） magic(魔法数）  区别语言标记
	u2 minior_version (次版本号）
	u2 major_version (主版本号）jdk 版本号必须大于等于主版本号
	u2 constant_pool_count
	cp_info constant_info
	u2 access_flag 
	u2 this_class (类索引） //确定类的全名
	u2 super_class (父类索引） //确定父类的全名
	u2 interface_count 
	u2 interface
	u2 fields_count
	field_info  fields
	u2  method_count
	method_info
	u2 attribute_count
	attribute_info
	
2. 版本号
	jdk        class
	1.1        45
	1.7        51
	
3. 常量池
	1> 常量池中的元素结构
		a> contant_utf-8_info
		b> constant_Integer_info
		c> constant_float_info
		d> constant_long_info
		e> constant_double_info
		f> constant_class_info
		g> constant_string_info
		h> constant_field_ref_info
		i> constant_method_ref_info
		j> constant_interface_ref_info
4. 访问权限修饰符
	1> public private
	2> class interface annotation enm 
	3> abstract 
	4> final 
	5> super 
	6> synthetic (合成的）
	access_flag 包含了class的以上信息,是由一系列的布尔值表示的
5. 方法和字段描述符号
	int a ; I 
	String s; Ljava/lang/String
	int[] arr; [I
	int[][] arr_1; [[I
	
	void() add; ()V
	String getName(); ()Ljava/lang/String
	int getAge(String student); (Ljava/lang/String)I
6. 方法表

	u2	access_flags
	u2      name_index
	u2      descriptor_index
	u2      atrribute_count
	attribute_info attributes
6. 属性表
	a> code 的属性
	java方法中的代码经过javac编译器处理后,最终变为字节码指令存储在Code 属性内
```


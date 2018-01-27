---
title: ECMAScript-Study
date: 2017-12-02 20:27:46
tags: javascript_basic
---
```
1. 基本数据类型
null undefine bool string number function object symbol(ES6)
2.Es6 function
	参数默认值只有在最后一个，才能省略，不然会报错
	function printAddress (country='中国',province,city){
		console.log(country+province+city);
	}
	
	析构赋值
	function printAddress ({country='中国‘，province,city}){
		console.log(country+province+city);
	}
	
	length 
	 对于默认值参数和rest(...)参数，不计入其中
3. Object
	Object.assign(target,source1...); 实行的是浅拷贝
	target 获得的只是source的引用
	
	Object 是由一系列的属性构成的可以是string 数字 bool
	甚至是function,每一个属性都有一个描述对象。
	
	Object.protoType 原型有三个重要的属性
	1> enumerable 
	2> writable
	3> configturable

	Object.getOwenPropertyDescriptor(obj,prop);
	返回一个对象{value,enumerable,configureable,writable}
	
	Object.getOwnPropertyDescripters(obj);
	
	Object.create(Object.getProtoType(obj),Object
	  .getOwnPropertyDescripters(obj));
	
	Object.create(prototype,propObject);
	propObject 的属性的enumerable 必须显示声明，默认为false
4. Object的属性遍历
	for..in 循环遍历自身和继承可枚举属性
	Object.keys（）返回自身可枚举属性的键名集合
	Object.getOwnPropertyNames(obj)获取自身所有属性(
		不包括Symbol属性）的键名集合
	Reflect.ownKeys(obj)返回自身所有键名的集合
5. == === Object.is()
	== 会自动转换数据类型
	=== 但Nan === NaN false ; -0 === +0 false;

6. Class 类
	类的使用必须用new 
	类里面声明的方法持有对象是类的原型，类里面的
	属性持有对象是this(即类的实例）；类里面的
	静态方法，实例无法使用，只能通过类直接
	引用,（ES6并没有静态属性）。原型定义的方法，
	类的实例都会继承。
	
	new.target 用于Constructor的方法中，如果构造
	函数不是通过new 命令调用new.target 返回的是
	undefined ,如果子类继承父类，那么父类构造方法，
	new.target 返回的是子类
	class Shape{
		constructor(){
			if (new.target === undefine){
				throws error;
			}
		}
	}		
```


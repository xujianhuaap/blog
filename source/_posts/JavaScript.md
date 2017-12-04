---
title: JavaScript
date: 2017-12-03 23:46:51
tags: JavaScript
---
```
1. Object_Oriented Programming (oop)
	every thing is Object;
2. create object
	a> function Person(name,age){
		return {name,age}
	} 
	
	var person = Person('xujianhua',27);
	person.name = 'xujianhua';
	
	b> function Person (name,age){
		this.name = name;
		this.age = age;
	}
	//must used new 
	var person = new Person('xujianhua',27);
	
	c> var person = {name: 'xujianhua',age:27);
	d> var person = new Object;
	person.name = 'xujianhua';
	person.age = 27;
	
	d> var person1 = Object.create(person);
3. javascript 是基于原型的语言 prototype-based
	原型链
4. Function 方法也是一种Object;

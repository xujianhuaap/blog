---
title: javascript-function
date: 2018-01-27 21:01:41
tags: javascript_function
---
1. function 
```
1> function Person(name,age){
	this.name = name;
	this.age = age;
    }
    //必须用new 
    var person = new Person('xu',27);
   var student = { name: 'li',speakEnlish: function(rank){} };
```
2. Fuction prototype
```
 a> property 
	Function.arguments 是一个数组
	Function.dispalyName 
 b> method
	call
	    func.call(thisArg, arg1,arg2,...) 作用类似于apply 只是参数不一样 
	bind
	   func.bind(thisArg,arg1,arg2...)
	apply
	    func.apply(thisArg, arguments) arguments是可选项,是一个参数数组
	    thisArg 可以是null
```
3. example
```
function printStr(){
        console.log("printStr", this.str);
}
var student = {str: "123"}
printStr.bind(student)();
console.log ("printStr prototype",Object.getPrototypeOf(printStr));

function Person(name,age){
        this.name = name;
        this.age = age;
}
var person = new Person("xujianhua",23);
console.log("person",person.name);
console.log("person prototype", Object.getPrototypeOf(person));
console.log("person constructor", person.constructor);

// print result 

printStr 123
printStr prototype function () { [native code] }
person xujianhua
person prototype Person {}
person constructor function Person(name,age){
	this.name = name;
        this.age = age;
}

```

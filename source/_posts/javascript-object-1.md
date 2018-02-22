---
title: javascript-object
date: 2018-01-29 20:07:49
tags: javascipt_object
---
1. Object prototype
```
a> method
   Object.getPrototypeOf(obj)
   Object.getOwnPropertyNames(obj)
b> property
   obj._proto_ // 不建议使用可以使用Object.getPrototypeOf(obj)
   obj.constructor 
```
2. example
```
var student = {name: "xujianhua",age: 27}
var student1 = Object.create(student,{
speak:{
        value: "English",
        enumerable:true,
        configuruable:true,
}

});
var student2 = Object.create(student1,{
joy:{
        value: function(){
                console.log("student2 joy","joy");
               },
        enumerable: true,

}
});
var student3 = Object.create(student2);
console.log("student1 prototype",Object.getPrototypeOf(student1));
console.log("student2 prototype",Object.getPrototypeOf(student2));
console.log("student3 prototype",Object.getPrototypeOf(student3));
// print result
student1 prototype { name: 'xujianhua', age: 27 }
student2 prototype { speak: 'English' }
student3 prototype { joy: [Function: value] }

```

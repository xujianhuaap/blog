---
title: javascript-typescript
date: 2018-02-01 11:23:42
tags: javascript_typescript
---
1. typescript
```
TypeScript是应用程序级JavaScript的一种语言.TypeScript
将可选的类型,类和模块添加到JavaScript中.TypeScript支持适
用于任何浏览器的大型JavaScript应用程序的工具,适用于任何操
作系统上的任何主机. TypeScript编译为可读,基于标准的JavaScript.
```
2. 逆变和协变
```
方法的参数检查 strict 模式,
HerbDog <: Dog <: Animal
caffeCat  <: Cat <: Animal

f1: (x: Dog)
f2: (x: Cat)
f3: (x: Animal)

f1 = f2; // err
f3 = f2; // strict 模式 err  

f2 = f3; // ok 

```
3. example
```
let age : int =23;
let isMan : boolean = true;
let some : any = "xu";

function(x:String)void {
}
```

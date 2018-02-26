---
title: javascript-typescript-1
date: 2018-02-22 20:58:21
tags: javascript_typescript
---
```
1. 基本数据类型定义
   let name:string = "xujianhua";
   let title:string = `welcome to school ,${name}`;
   let isMan: boolean = true;
   let age:number = 27;
   let arr: number[] = [23,23,34];
   enum Color {red,green,white}
   let red : Color = Color.red;
   let someThing:any = "xujianhua";
2. 变量的作用域
   var vs let
   var的缺点：
   a> 一个方法内部可以重复声明同一个变量
      for(var i= 0;i<4;i++){
          for(var i=1; i<5; i++){
          }
      }
      容易出现逻辑错误
   b> var 声明的作用域是方法域
      function print(){
         if(true){
            var str = "xujianhua";
         }
         returb str;
      }
      str 可以在print的任何位置声明，str的访问也是整个方法内部。
   c>  for(var i =0; i<5;i++){
		setTimeout(2s,console.log(i));
        }
        结果是 4 ，4 ，4 ，4 ,4
3. 接口
   interface person {
      sports ?:string[];//可选属性
      score ?: number;
      name:string;
      age: number;
   }
4. 模块
   内部模块（namespace)
   外部模块（module)
        任何包含import 或export的文件都是一个模块
        任何一个模块都有一个默认导出,
        模块导出的是方法 变量 类 接口
	    student.ts//js 文件
            export default class Student{}
            score.ts // 成绩js文件 
            import student from "./student"
            var s1= new student();
        模块导出的形式
	    1> export class Student{}
            2>
              class Student{}
              export = Student; 
5. example
   shapes.ts//形状文件
   export class Circle{}
   export class Rectangle{}
   shapeDrawer.ts//绘制文件
   import * as shape from "shape"
   var circle = new shape.Circle();
   
   type Task = (taskData:any)=>{
        }
   export= Task;
```

title: reflect_study
date: 2015-12-18 00:09:49
tags: reflect
---
```
public class ReflectDemo {
   public static void main(String[]args){
       System.out.println("---hello-----");

       /***
        * 不难看出
        * getGenericInterface（）的区别 包含泛型类信息
        */
       Student student=null;
       Class[] classes= Student.class.getInterfaces();
       if(classes.length>0){
           for(int i=0;i<classes.length;i++){
               System.out.println("Class getInterfaces() ["+i+"]"+classes[i]);
           }
       }
       Type[] types= Student.class.getGenericInterfaces();
       if(types.length>0){
           for(int i=0;i<types.length;i++){
               System.out.println("Class getGenericInterfaces() ["+i+"]"+types[i]);
               System.out.println("Class getGenericInterfaces() ["+i+"]"+(types[i] instanceof ParameterizedType));
           }
       }
   }
 ```

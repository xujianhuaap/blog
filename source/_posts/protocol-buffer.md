---
title: protocol_buffer
date: 2018-02-05 16:03:46
tags: proto_buf
---
1. proto_buf 简介
```
  proto_buf 是一种类似于json xml 的结构化数据的序列化协议,但比两者效率更高.
```
2. example
```
  syntax="proto3";
  message Person {
     String name=1;
     int age=2;
     boolean gender = 3;
  }
  
  1 2 3 称为tag,一旦分配完毕,就不能修改.
```  

title: go_beego_study
date: 2016-06-06 17:22:54
tags: go beego
---
```
1. beego 基于MVC的Web后端开发框架---路由正则注册
  1> "/user/:id" 可以匹配 "/user/register" 不能匹配 "/user"
  2> "/user/:?id" 可以匹配 "/user/register" 也可以匹配"/user"
  3> "/user/*" 可以匹配"/user/register/uid"
  4> "/user/*.*"可以匹配"/user/register/hi.html"
  5> "/user/:id[1-9]+"
  6> "/user/:id[\w]+"
```
```
2. beego 支持注解
```

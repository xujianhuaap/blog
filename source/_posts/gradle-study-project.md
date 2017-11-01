title: gradle-study-project
date: 2016-04-15 17:41:34
tags: gradle Project
---
1. gradle 是一个强大的构建工具，其中主要立足于project这个对象进行构建。
project在这里不仅指工程本身，而还包括编译，打包，签名，上传，等多个任务。
project对应一个project_level的build.gradle文件，在这个文件中，有它的
任务相关所需要的配置。project的相关介绍如下：
```
1> build生命周期
首先，如果存在setting.gradle，那么对工程结构进行设计。再者，如果存
在子工程那么在子工程评估之前完成对该工程的评估。
2> project 有5个属性范围。当project查询一个属性时按照以下顺序进行：
project本身；extra;extensions；conventions;tasks;它们的特点如下：
extra 是可读可写的；extensions 由plugins产生 ，只能读。
3>project 有5个方法区间。当project调用方法时按照以下顺序进行:project
本身；build文件；extensions;conventions;taks;其中需要特别注意的是
如果在task中调用某方法，那么事实上并且该方法只有一个参数并且该参数是
闭包或者Action，那么
```

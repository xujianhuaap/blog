title: gradle_summary
date: 2016-01-05 17:11:04
tags: gradle
---

1. gradle 是一种脚本语言。gradle 脚本主要是配置脚本，这是说当脚本执行时，它配置成为特定的类型对象。
例如 如果是build脚本执行，那么它配置的对象是Project。常用的脚本类型有如下：
```
类型  	                   配置对象
Build script	            Project
Init script                 Gradle
Settings script	         Settings
```

  gradle 脚本实现了script的接口，script中定义了我们所用的方法和属性。

2. build script 的结构

  ```
  sourceSet{}
  repositories{}
  dependencies{}
  allProjects{}
  artificats{}
  configurations{}
  ```

3. 核心类型
```
Project
Task
Settings
script
SourceSet
...
```

title: gradle_task
date: 2016-01-05 17:33:03
tags: gradle_task
---

1.Task

task 是Project的基本功能单元。每一个task都有一个名字和全路径，名字在所在的Project内指向该任务，而全路径在所有任务和工程内是独一无二的，它是由工程路径和任务名组合而成，路径用：隔开。

task的定义可以通过TaskContainer的TaskContainer.create();或者在Build的文件中使用task关键字，如下：

```
    task myTask
    task myTask { configure closure }
    task myType << { task action }
    task myTask(type: SomeType)
    task myTask(type: SomeType) { configure closure }
```
2. task 的执行
一个task 是由一系列的Action组成，当task在执行时通过调用Action.execute(),action依次进行，你也可以通过Task.doFirst()或者Task.doLast()添加Actions.当然闭包也可以作为Action.

Task.dependOn()
Task.shouldRunAfter()
Task.

3. 在build的文件中使用task

 动态属性

 你可以通过属性名字获得task的属性，也可以通过Task.Property().也可以通过Task.setProperty()改变对应的属性值。

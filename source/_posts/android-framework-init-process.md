title: android_framework_init_process
date: 2016-01-12 13:18:05
tags: android init_process
---
所涉及到的文件路径
```
system/core/rootdir/init.rc
system/core/init/init.cpp
```


1.作为android系统的第一个进程编号为1

2.该进程文件位于system/core/init/init.cpp其main函数主要作了如下工作
```
1> 创建文件夹 挂载设备
2>解析init.rc 和init.xiaomi.rc配置文件获得一系列的Action.
这些Action处于不同的阶段(Session).
3>执行各个阶段的Action
4>首先执行early_init 阶段的Action,创建Uevnet与Linux内核交互
的Socket,初始化相关的属性资源,初始化keychord的设备(与调试有关),
加载开机动画文件.
5>其次执行init阶段的动画,启动属性服务调用socketpair函数创建两个
已经连接好的socket
6>继续执行early_boot阶段的Action
7>再执行booot阶段的Action
8>监听来自四个方面的事情,来自内核的Uevent事件,来自属性服务器的事件
,由socketpair创建的来自另一个socket的事件,以及keychord初始化成功
情况下,来自该设备的事件.
9> 开启无限循环,重启死去的子进程

```

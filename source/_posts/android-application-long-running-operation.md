title: android-application-long-running-operation
date: 2016-02-20 07:45:07
tags: android_application_long_running_operation
---
```
1. 耗时操作在android你会想起什么，Service Thread Asynctask,它们的区别又是什么？
2. 异步任务，是android设计的初衷是合理利用UI线程，避免开启新的线程和使用hanlder实
现线程间的通信来进行耗时操作．异步任务运行在一个后台的线程里，最多几秒的耗时任务，再长，
推荐使用并发的ThreadPoolExcutor FutureTask．异步任务只能被执行一次，第二次执行时
会报异常．在android3.0之后excute()无法并发，要并发需要使用excuteonPoolExcutor().
Service 是不与用户进行交互的这时候可以选用．其他时候可以选用Thread.  
3. 一般来说，一个应用中的组件都运行在同一个进程中．这些进程可以根据生命周期可以划分为，前台
进程，可见进程，服务进程，后台进程，空进程．进程被杀的可能性依序增大．进程的生命周期阶段取决
与这个应用所在的组件的状态．例如：如果一个应用Activity还处于交互状态，或者bound service
还在与绑定它的组件在交互．正在执行onRecive()的Receiver,那么此时该进程处于前台进程．如果
一个Activity(onPauser()被调用)或者被绑定的组件进入可见状态的bound service ,那么该进程
处于可见进程．如果一个service 启动了startService(),拥有这样组件的进程是服务进程．对于
Activity已经调用onStop()后，就不可见了，它即使被杀死也不影响用户体验．这样的进程成为后台
进程．
4. android将线程分为主线程（UI线程）或工作线程．在一个应用启动后，开启一个Linux进程后会
有运行一个线程这个线程成为主线程．在这个线程主要是访问UI工具库，因此又称为UI线程．一下是两
个记住的规则：UI线程不能堵塞，不能在UI线程外访问UI工具库．
5. 线程安全
某些情况下，一个方法可能在不同的线程中执行，例如在bound service中的方法，

```

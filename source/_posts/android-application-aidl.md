title: android-application-aidl
date: 2016-01-27 09:29:56
tags: aidl
---
1.  参考文件
```
```
2.  aidl
```
aidl(Android Interface Descriptor Language)是一种为跨进程通信的接口描述语言，
作用类似于ContentProviderNativie类一样的功能，aidl的内部类Stub一方面描述了业务所
需的成员变量和方法，另一方面继承了Binder;Stub的调用是在服务端调用，对于客户端进程在
在与服务端进程建立链接后，通过Stub.asInterface()获得服务端进程的业务类的副本，并调
用相应的方法．当客户端调用某个方法时，在服务端就会调用相应的方法．
在Eclipse下．aidl文件由aidl工具生成位于工程gen包下的aidl包下java文件．它只能引用
其他aidl中的接口，不能直接引用java类中的接口．
```

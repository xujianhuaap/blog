title: android-application-contentprovider
date: 2016-01-25 20:27:59
tags: content_provider
---
1.  参考文件
```
core/java/android/content/pm/ProviderInfo.java
services/java/com/android/server/am/ContentProviderRecord.java
core/java/android/app/IActivityManager.java
```
2.  ContentProvider的简介
```
1>  实现数据在应用间共享.这是对SQliteDataBase的互补.
2>  管理数据主要是通过ContentResolver
3>  contentprovider 是安卓本地数据库(repository)管理者,数据一般是关系型数据.
4>  contentprovider与client必须提供标准的接口
```
3. contentprovider的主要方法
```
和Activity一样,需要在manifest.xml中配置<provider></provider>,contentprider的实例有系
统创建,因此应用一般不需要直接实例化ContentProvider.在实例化后并不能直接使用他的属性和方法,必
须由他的实现类在onCreate()进行初始化.
contentprovider作为抽象类,我们必须实现它的抽象方法:onCreate()(),增删查改.
Transport内部类的作用,就是跨进程的binder机制通信．Transport继承于ContentProviderNativie
而ContentProviderNativie作为一个抽象类继承于Binder并且实现了Ｉcontentprovider.而且其内部
类ContentProviderProxy实现了IContentProvider.IContentProvider
```
４．  抽象类ContentProviderNativie
```
//根据obj查询出IContentProvider 若为空创建ContentProviderProxy,ContentProviderProxy
//实现了IContentProvider
static IContentProvider asInterface(IBinder obj)
//返回ContentProviderNativie的实例
static IBinder asBinder()
```
5.  ContentProviderHolder的介绍
```
ＩActivityManager的内部类
ContentProviderHolder实现了Parcelable
主要成员变量为IContentProvider ProviderInfo
构造器为ContentProvider(ProviderInfo info)
值得研究的方法：



```
6. 

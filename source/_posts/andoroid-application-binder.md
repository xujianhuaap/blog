title: andoroid-application-binder
date: 2016-01-26 16:49:27
tags: binder
---
1.  参考文件
```
core/java/android/os/Binder.java
```
2.  Binder(java)中的主要方法
```
//owner业务层类　 descriptor业务层类的标签（通过该标签查询出业务层类）
//该方法一般在创建业务层native类实例时，进行调用．
public void attachInterface(IInterface owner, String descriptor);
//通过该标签查descriptor询出业务层类
public IInterface queryLocalInterface(String descriptor);
//与内核binder的驱动交互的地方
public final boolean transact(int code, Parcel data, Parcel reply,int flags);
```
3.  binder机制下的业务层native类
```
//根据obj的descriptor查询业务层类的实例，如果为空创建新的实例
static T　asInterface(IBinder obj)
```

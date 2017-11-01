title: android-application-parcelable
date: 2016-01-26 20:18:46
tags: Parcelable
---
1.  参考文件
```
core/java/android/os/Parcel.java
```
2. Parcel
```
Parcel 是信息（可以通过IBinder传递）的容器,这些信息可以是数据或者引用，它里面包含的是扁平的
数据，这些数据在目标进程再去扁平化．一般是通过多种方法写特定的类型信息，或者实现Parcelable接
口，进而若发送ＩBinder的进程还在存活，该Ibinder的引用将会为目标进程带来一个BinderProxy,与
发送进程进行通信．
要特别注意，Parcel 不是一般意义上的数据的序列化．这个类，以及Parcelable是为进程间通信高效实
现设计的，在这样的情况下，对Parcel数据进行持久化储存是不合适的，因为任何Parcel中的底层数据的
变化可能使老数据不可读．
Parcel 中可以储存基本数据类型，基本数据的数组，实现了Parcleable的实体类，Bundle,还有一些特
殊的例如：
Active Objects
  writeStrongBinder(IBinder)              readStrongBinder()
  writeStrongInterface(IInterface)        readBinderArray(IBinder[])
  writeBinderArray(IBinder[])             createBinderArray()
  writeBinderList(List)                   readBinderList(List)
  createBinderArrayList()
没类型的容器
  writeValue(Object)       readValue(ClassLoader)
  writeArray(Object[])     readArray(ClassLoader)
  writeList(List)          readList(List, ClassLoader)
  readArrayList(ClassLoader)
  writeMap(Map)            readMap(Map, ClassLoader)
```
3.  Parcel 工作机制
```
//ParcelPool POOL_SIXE=6(默认情况下)　
private static final Parcel[] sOwnedPool = new Parcel[POOL_SIZE];
private static final Parcel[] sHolderPool = new Parcel[POOL_SIZE]
//先判断sOwnedPool中是否有不为空的若有则返回对应的Ｐarcele，若都为空就创建一个Parcle
//实例　new Parcel(mOwnObject)
public static Parcel obtain()
//回收机制
public final void recycle() ;
```
4.  Parcel.cpp(暂不分析)
```
struct flat_binder_object {
         /* 8 bytes for large_flat_header. */
         __u32           type;
         __u32           flags;

         /* 8 bytes of data. */
         union {
                 binder_uintptr_t        binder; /* local object */
                 __u32                   handle; /* remote object */
         };

       /* extra data associated with local object */
       binder_uintptr_t        cookie;
};
   void acquire_object(const sp<ProcessState>& proc,const flat_binder_object& obj,
   const void* who);
   void acquire_object(const sp<ProcessState>& proc,
                       const flat_binder_object& obj, const void* who);
   void release_object(const sp<ProcessState>& proc,
                       const flat_binder_object& obj, const void* who);
   void flatten_binder(const sp<ProcessState>& proc,
                       const sp<IBinder>& binder, flat_binder_object* out);
   void flatten_binder(const sp<ProcessState>& proc,
                       const wp<IBinder>& binder, flat_binder_object* out);
   status_t unflatten_binder(const sp<ProcessState>& proc,
                             const flat_binder_object& flat, sp<IBinder>* out);
   status_t unflatten_binder(const sp<ProcessState>& proc,
                             const flat_binder_object& flat, wp<IBinder>* out);

```

title: android-application-handler
date: 2016-01-27 13:01:32
tags: handler
---
1. 参考文件
```
core/java/android/os/Looper.java
core/java/android/os/Handler.java
java/lang/Thread.java
java/lang/ThreadLocal.java
```
2.Thread
```
１＞在一个线程中启动一个新的线程，我们称为线程为原线程的子线程，因此一个Thread可能会有Parent
Thread,也可能有自己的Child Thread．当我们每创建新线程的时候，都会调用Create()方法，在
这个方法中，主要执行一下逻辑，获得当前的线程（Thread.currentThread()native），给当前线
程设置相关变量，threadGroup（线程属于那一组），线程的target(runnable),线程的名字(thre
adName),线程的优先级（priority),若当前线程的ｉnheritableValues不为空，则这样情况下创建
ThreadLocal.Values(currentThread.inheritableValues)，并赋予当前线程的
inheritableValues,把Thread实例添加到当前组中．
２＞两个重要的成员变量，ThreadLocal.Values inheritableValues(继承而来) localValues
３＞每一个线程进行线程间handler通信，必须要有Looper,每一个线程只有一个Looper．
4>每一个线程都对应一个ThreadLocal.Values,但是所有的线程，只有一个ThreadLocal.
```
2.  ThreadLocal
```
这个类实现了线程本地化存储，所有的线程都拥有一个ThreadLocal的变量，也有对应的Values
保存在ThreadLocal中.
ThreadLocal<T> T 就是线程的范型
//返回的是当前线程
<T> get()｛
  <1>获得当前线程
  <2>获得当前的线程的localValues并赋予局部变量Values values
  <3>若values不为空，根据ｖalues的成员变量table,以及mask(可以求得index)
      Object[] table = values.table;
                int index = hash & values.mask;
                if (this.reference == table[index]) {
                    return (T) table[index + 1];
                }
      ｝
  <4>若为空则创建新的localValues实例，调用（T）values.getAfterMiss(this)
｝
//
Object getAfterMiss(ThreadLocal<?> key) {
}
静态内部类
static class Values{
  １>成员变量介绍
  //必须是２的幂值
  private static final int INITIAL_SIZE = 16;
  //移除的实体holder
  private static final Object TOMBSTONE = new Object();
  //Map实体类，key为ThreadLocaL 长度为２的幂值
  //table　是一个序列key,value，key,value．．．
  //key为WeakReference<ThreadLocaL<T>>
  private Object[] table;
  //用于将Hash值转成index
  private int mask;
  //活者的实体类的数目
  private int size;
  //被移除掉的实体类的数目
  private int tombstones;
  //存活的实体类和被移除掉的实体类最大数目
  private int maximumLoad;
  /** Points to the next cell to clean up. */
  private int clean;
  2> 构造函数
  Values(){
    //创建新的空的table,长度为INITIAL_SIZE*2,并且mask为table．lengh-1
    //maximumLoad为INITIAL_SIZE 的２／３，clean=0;
    initializeTable(INITIAL_SIZE);
  }

  Values(Values fromParent)｛
    <1>//将fromParent的相关属性设置到新的实例上
    <2>继承Table
    inheritValues（fromParent）;
  ｝
  ３>重要的方法
    //在GC可能回收的情况下，对Ｖalues的table中某项，该项是reference（key），对其进
    //行判断,若放在其中的Ｔ为空说明GC已经回收了其中的值，则置该项为TOMBSTONE(墓碑)，
    //它的下一项为null,size--,tombstones++m,并将该项的index赋予Values的clear
    cleanUp();
    //table 可能是　null,tombstones,也可能是reference
    //对于table的每一项进行轮询，若某项既不为空也不为TOMBSTONE(移除的实体),
    //那么该项为reference，取出引用中的Raw type (InheritableThreadLocal)
    //若InheritableThreadLocal不为空（没有被ＧＣ机制回收），那么 该项的下一
    //项为fromParent的对应项的值．若为空则该项为TOMBSTONE,下一项的值为空，size--,
    //tombstones++     
    private void inheritValues(Values fromParent);
}
```
3. Looper
```
1>Looper 是一个循环器，检查MessageQuee是否有Message;
2>在创建Looper实例化之前就已经实例化了一个mThreadLocal
  static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();
//主要是创建Looper的实例并且设到类成员变量sThreadLocal上
public static final void prepare();
```
4. handler
```
//CallBack 中的方法是handleMessage(Message msg)
public Handler(Looper looper, Callback callback, boolean async);
//
public Handler(Callback callback, boolean async) {
  ....
  //实质上是从mThreadLocal中获得mThreadLocal.get()
  mLooper = Looper.myLooper();
  ....
}
```
5. 总结
```
ThreadLocal<Looper> reference
ThreadLocal.Values table
Thread重要成员变量　inheritableValues localValues
Looper　threadLocal（类成员变量）
创建线程时，若运行的线程有inheritValues则创建ThreadLocal.Values的实例，并赋值给
Thread的成员变量inheritableValues．
在创建好的线程中加载Looper的同时，创建ThreadLocal<Looper>的实例(同时创建reference实例)
，在实例化Looper的同时将实例设置到ThreadLocal<Looper>的实例上．在这个过程中实例化
localValues,并调用其put()
```

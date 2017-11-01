title: android_frameworks_binder
date: 2016-01-15 09:54:43
tags: binder
---
1.  RefBase 指针管理公共类,提供引用计数的方法,但由StrongPointer和WeakPointer来管理.
```
class RefBase
{
public:
void  incStrong(const void* id) const;增加强引用
void  decStrong(const void* id) const;减少强引用
void  forceIncStrong(const void* id) const;强制增加强引用
int32_t   getStrongCount() const;获得当前强引用的数量
//创建弱引用类型
weakref_type*   createWeak(const void* id) const;
weakref_type*   getWeakRefs() const;//获得弱引用类型
//追踪
inline  void    trackMe(bool enable, bool retain)
{
    getWeakRefs()->trackMe(enable, retain);
}
//若弱类型的处理
class weakref_type
{
public:
  void  incWeak(const void* id);增加弱引用
  void  decWeak(const void* id);减少弱引用
  bool  attemptIncStrong(const void* id);
  bool  attemptIncWeak(const void* id);
  int32_t   getWeakCount() const;获得弱引用的数量
  void  trackMe(bool enable, bool retain);
};
};
```
2.  IInterface的功能
```
1>  继承与RefBase
2>  定义了许多template函数常见的有
interface_cast(sp<IBinder>)
3>  定义了template的类
BpInterface
BnInterface
4>  定义了宏方法
#define DECLARE_META_INTERFACE(INTERFACE)
#define IMPLEMENT_META_INTERFACE(INTERFACE, NAME)  
5>常见方法
sp<IBinder>asBinder
```
3.  IServiceMananer的功能
```
1>继承于Interface
2>addService()
  listService()
  checkService()
  getService()
3> 重要的方法
  sp<IServiceMananger>defaultServiceManager()
  class BnServiceManager : public BnInterface<IServiceManager>
{
public:
    virtual status_t   onTransact()
};  
```
4.  BpServiceManager的功能
```
1>  继承与BpInterface<IServiceMananger>
注意:范型表示BpInterface继承于IServiceMananger\
见模板函数BpInterface 定义
```
2.  BpInterface的定义
```
template<typename INTERFACE>
class BpInterface : public INTERFACE, public BpRefBase
{
public:
    BpInterface(const sp<IBinder>& remote);
protected:
    virtual IBinder*   onAsBinder();
};  +
```

title: android_framework_binder_common_class
date: 2016-01-15 17:09:28
tags: android_framework_binder
---
1.  参考文件
2.  ProcessState的介绍
```
1> 继承于RefBase
2>关于getContextObject()
sp<IBinder> ProcessState::getContextObject(const sp<IBinder>& caller)
{
    //在创建ProcessState实例过程中如果成功打开binder设备supportsProcesses()为true
    //取决与open_driver()返回的参数
    if (supportsProcesses()) {
        return getStrongProxyForHandle(0);
    } else {
        return getContextObject(String16("default"), caller);
    }
}
//让我们来看一下
sp<IBinder> ProcessState::getStrongProxyForHandle(int32_t handle)
{
    sp<IBinder> result;
    AutoMutex _l(mLock);//自动同步机制
    //handle_entry
    handle_entry* e = lookupHandleLocked(handle);
    if (e != NULL) {
        // We need to create a new BpBinder if there isn't currently one, OR we
        // are unable to acquire a weak reference on this current one.  See comment
        // in getWeakProxyForHandle() for more info about this.
        IBinder* b = e->binder;
        if (b == NULL || !e->refs->attemptIncWeak(this)) {
            b = new BpBinder(handle);
            e->binder = b;
            if (b) e->refs = b->getWeakRefs();
            result = b;
        } else {
            // This little bit of nastyness is to allow us to add a primary
            // reference to the remote proxy when this team doesn't have one
            // but another team is sending the handle to us.
            result.force_set(b);
            e->refs->decWeak(this);
        }
    }
    return result;
}
ProcessState::handle_entry* ProcessState::lookupHandleLocked(int32_t handle)
{
    //mHandleToObject是Vector,里面存放的是handle_entry
    const size_t N=mHandleToObject.size();
    if (N <= (size_t)handle) {
        handle_entry e;
        e.binder = NULL;
        e.refs = NULL;
        status_t err = mHandleToObject.insertAt(e, N, handle+1-N);
        if (err < NO_ERROR) return NULL;
    }
    return &mHandleToObject.editItemAt(handle);
}
struct handle_entry {
     IBinder* binder;
     RefBase::weakref_type* refs;
 };
 总结: 在ProcessState的getContextObject()根据打开Binder设备的情况,
 若成功测调用getStrongProxyForHandle(),并且返回handle所对应的handle_entry,
 若handle_entry不为空,并且该handle_entry的binder为空或者其refs尝试
 增加弱引用失败,就重新新建BpBinder,并且把BpBinder*的实例返回.
 getStrongProxyForHandle()逻辑如下:
 从handle_entry的集合中先查找对应的handle_entry,若没有则创建新的handle_entry,
 并且插入到hanle_entry集合的队尾,且返回handle所对应的handle_entry.
 binder为new BpIbinder();
```

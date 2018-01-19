title: android_frameworks_binder_communication
date: 2016-01-15 15:08:22
tags: android_framework_binder
---
1.  参考文件
```
av/media/mediaserver/main_mediaserver.cpp
platform/base/media/libmediaplayerservice/MediaPlayerService.cpp
platform/base/include/binder/IInterface.h
```
2.  main_mediaserver中的main函数逻辑如下
```
int main(int argc __unused, char** argv){
  //获得ProcessState实例单例模式,创建ProcessState实例也是打开binder虚拟设备的过程
  //这其中会挂载文件,映射内存
  sp<ProcessState> proc(ProcessState::self());
  MediaLogService::instantiate();
  ProcessState::self()->startThreadPool();
  //获得sp<IServiceManager>的实例
  sp<IServiceManager> sm = defaultServiceManager();
  sp<IBinder> binder = sm->getService(String16("media.log"));
  //实例化各种相关的服务
  AudioFlinger::instantiate();
  MediaPlayerService::instantiate();
  ResourceManagerService::instantiate();
  CameraService::instantiate();
  AudioPolicyService::instantiate();
  SoundTriggerHwService::instantiate();
  RadioService::instantiate();
  registerExtensions();
  //开启线程池
  ProcessState::self()->startThreadPool();
  IPCThreadState::self()->joinThreadPool();
}
```
3.  defaultServiceManager()功能如下
```
//defaultServiceManager()代码如下
sp<IServiceManager> defaultServiceManager()
{
    gDefaultServiceManager = interface_cast<IServiceManager>(
                ProcessState::self()->getContextObject(NULL));
    return gDefaultServiceManager;
}
//由于IServiceMananer继承与IInterface ,因此interface_cast()为IInterface中的template
//函数,实质是调用template(IServiceMananer)的asInterface方法,这里是宏方法
//#define DECLARE_META_INTERFACE(INTERFACE)      
template<typename INTERFACE>
inline sp<INTERFACE> interface_cast(const sp<IBinder>& obj)
{
    return INTERFACE::asInterface(obj);
}
//宏方法asInterface()事实上defaultServiceManager是BpServiceManager的实例
#define IMPLEMENT_META_INTERFACE(INTERFACE, NAME)                       \                                                                             android::sp<I##INTERFACE> I##INTERFACE::asInterface(                    \
            const android::sp<android::IBinder>& obj)                   \
    {                                                                   \

        return intrintr = new Bp##INTERFACE(obj); ;                     \
    }                                                                   \
    I##INTERFACE::I##INTERFACE() { }                                    \
    I##INTERFACE::~I##INTERFACE() { }  
```
4.  在MediaPlayerService中
```
void MediaPlayerService::instantiate() {
    defaultServiceManager()->addService(
            String16("media.player"), new MediaPlayerService());
}
总结:
1>各种ServiceManager.addService()实质上是defaultServiceManager.addService()
而在有是调用的binder-->transact(),进而是IPCThreadState::self()-->transact()
2>参数流如下:
addService(const String16& name, const sp<IBinder>& service)主要工作如下:
  1.声明两个Parcel实例data reply
  2.将name和service写到data中
  3.调用binder-->transact(ADD_SERVICE_TRANSACTION,data,&reply)
binder::transact()
   IPCThreadState::self()->transact(mHandle, code, data, reply, flags);
```

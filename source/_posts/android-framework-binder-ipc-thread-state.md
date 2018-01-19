title: android-frameworks-binder-ipc_thread_state
date: 2016-01-18 15:48:49
tags: android_framework_binder
---
1.  参考的文件
```
android / platform / external / pthreads / master / . / pthread.h
platfrom/base/libs/binder/IPCThreadState.cpp
```
2.  IPCThreadState的self()
```
//获得IPCState的实例
IPCThreadState* IPCThreadState::self()
{
  if (gHaveTLS) {//是否拥有线程专有存储,默认为false
restart://goto跳转标志
      const pthread_key_t k = gTLS;
      IPCThreadState* st = (IPCThreadState*)pthread_getspecific(k);
      if (st) return st;
      return new IPCThreadState;
  }

  if (gShutdown) return NULL;

  pthread_mutex_lock(&gTLSMutex);
  if (!gHaveTLS) {
      if (pthread_key_create(&gTLS, threadDestructor) != 0) {
          pthread_mutex_unlock(&gTLSMutex);
          return NULL;
      }
      gHaveTLS = true;
  }
  pthread_mutex_unlock(&gTLSMutex);
  goto restart;
}
在 IPCThreadState的实例中主要做了一下工作
1>将该实例设到该线程专有的存储中
2>设置与binder交互的缓存区
```
3.  IPCThreadState的transact()
```
//int32_t cmd
//uint32_t binderFlags
//int32_t handle
//uint32_t code
//const Parcel& data
//status_t* statusBuffer
status_t IPCThreadState::transact()
{
    //将data中的属性封装到binder_transaction_data tr
    //并且将tr写入out缓冲区中
    1>writeTransactionData()
    //
    2>waitForResponse(){
      //
      cmd = mIn.readInt32();
      err=talkWithDriver()
      switch(cmd){
        case default:
        err = executeCommand(cmd);
      }
    }
    return err;
}
//talkWithDriver(bool doReceive)
//跨设备通信,主要是通过ioctl(fmd,cmd,&bwr)完成的,ioctl是Linux内核的方法
//暂不研究,在IPCThreadState的In缓存中,存储着请求信息
tatus_t IPCThreadState::talkWithDriver(bool doReceive)
{
    1>断言是否成功打开Binder设备
    2> binder_write_read bwr
    3>判断是否in缓存有数据需要读
    const bool needRead = mIn.dataPosition() >= mIn.dataSize();
    4>获得out缓存可用的空间
    const size_t outAvail = (!doReceive || needRead) ? mOut.dataSize() : 0;
    5>将out缓存可用的空间以及数据占用情况赋值给binder的读写数据结构 bwr
    bwr.write_size = outAvail;
    bwr.write_buffer = (long unsigned int)mOut.data();
    6>//有读取请求,并且执行的是doRecive,就将请求的数据所占的空间赋值给binder读写数据结构bwr
    if (doReceive && needRead) {
        bwr.read_size = mIn.dataCapacity();
        bwr.read_buffer = (long unsigned int)mIn.data();
    } else {
        bwr.read_size = 0;
    }
    7>//打印相关日志
    IF_LOG_COMMANDS() {
        TextOutput::Bundle _b(alog);
        if (outAvail != 0) {
            alog << "Sending commands to driver: " << indent;
            const void* cmds = (const void*)bwr.write_buffer;
            const void* end = ((const uint8_t*)cmds)+bwr.write_size;
            alog << HexDump(cmds, bwr.write_size) << endl;
            while (cmds < end) cmds = printCommand(alog, cmds);
            alog << dedent;
        }
        alog << "Size of receive buffer: " << bwr.read_size
            << ", needRead: " << needRead << ", doReceive: " << doReceive << endl;
    }

    //没有可以读写的
    if ((bwr.write_size == 0) && (bwr.read_size == 0)) return NO_ERROR;
    //初始化关于读写的
    bwr.write_consumed = 0;
    bwr.read_consumed = 0;
    status_t err;
    do {
        #if defined(HAVE_ANDROID_OS)
                //这是关键
                if (ioctl(mProcess->mDriverFD, BINDER_WRITE_READ, &bwr) >= 0)
                    err = NO_ERROR;
                else
                    err = -errno;
        #else
                err = INVALID_OPERATION;
        #endif

    }
    if (err >= NO_ERROR) {
        if (bwr.write_consumed > 0) {
            if (bwr.write_consumed < (ssize_t)mOut.dataSize())
                mOut.remove(0, bwr.write_consumed);
            else
                mOut.setDataSize(0);
        }
        if (bwr.read_consumed > 0) {
            mIn.setDataSize(bwr.read_consumed);
            mIn.setDataPosition(0);
        }

        return NO_ERROR;
    }

    return err;
}
```

title: android-frameworks-servermanager
date: 2016-01-19 17:44:25
tags: servermanager
---
1.  参考文件
```
platform/base/cmds/servicemanager/service_manager.c
platform/base/cmds/servicemanager/binder.c
```
2.  ServerManager.main()
```
int main(int argc, char **argv)
{
    struct binder_state *bs;
    void *svcmgr = BINDER_SERVICE_MANAGER;
    //打开binder虚拟设备,
    bs = binder_open(128*1024);//binder所占内存大小
    //binder_become_context_manager(bs)与binder设备进行通信将自己变成Context_ServerManager
    if (binder_become_context_manager(bs)) {
        LOGE("cannot become context manager (%s)\n", strerror(errno));
        return -1;
    }
    svcmgr_handle= svcmgr;
    //检查是否有有效请求,若有将会调用svcmgr_handler
    binder_loop(bs, svcmgr_handler);
    return 0;
}


```
3.  在binder.h中相关的方法
```
struct binder_state
{
    int fd;
    void *mapped;//未确定类型的指针
    unsigned mapsize;
};
//打开虚拟binder设备
struct binder_state *binder_open(unsigned mapsize)
{
    struct binder_state *bs;
    bs = malloc(sizeof(*bs));//向系统申请分配内存(低于128k)     
    if (!bs) {
        errno = ENOMEM;
        return 0;
    }
    //挂载'/dev/binder'目录
    bs->fd = open("/dev/binder", O_RDWR);
    if (bs->fd < 0) {
        fprintf(stderr,"binder: cannot open device (%s)\n",
                strerror(errno));
        goto fail_open;
    }
    //设置bs 反映binder设备所占内存大小的变量mapsize
    bs->mapsize = mapsize;
    //映射内存
    bs->mapped = mmap(NULL, mapsize, PROT_READ, MAP_PRIVATE, bs->fd, 0);//用户进程级别申请内存
    if (bs->mapped == MAP_FAILED) {
        fprintf(stderr,"binder: cannot map device (%s)\n",
                strerror(errno));
        goto fail_map;
    }
    return bs;
}
//註冊成爲Context_Manager
int binder_become_context_manager(struct binder_state *bs)
{
    return ioctl(bs->fd, BINDER_SET_CONTEXT_MGR, 0);
}
//binder_handler 是一个回调函数
//不断通过ioctl检查是否有请求
void binder_loop(struct binder_state *bs, binder_handler func)
{
    int res;
    struct binder_write_read bwr;//binder设备读写数据结构
    unsigned readbuf[32];
    //初始化binder设备读写数据结构
    bwr.write_size = 0;
    bwr.write_consumed = 0;
    bwr.write_buffer = 0;
    //设置为readbuf的首变量
    readbuf[0] = BC_ENTER_LOOPER;
    //
    binder_write(bs, readbuf, sizeof(unsigned));
    for (;;) {
        bwr.read_size = sizeof(readbuf);
        bwr.read_consumed = 0;
        bwr.read_buffer = (unsigned) readbuf;
        res = ioctl(bs->fd, BINDER_WRITE_READ, &bwr);
        if (res < 0) {
            LOGE("binder_loop: ioctl failed (%s)\n", strerror(errno));
            break;
        }
        //解析请求
        res = binder_parse(bs, 0, readbuf, bwr.read_consumed, func);
        if (res == 0) {
            LOGE("binder_loop: unexpected reply?!\n");
            break;
        }
        if (res < 0) {
            LOGE("binder_loop: io error %d %s\n", res, strerror(errno));
            break;
        }
    }
}
//ioctl是设备驱动程序中对设备的I/O通道进行管理的函数,函数是switch(cmd)条件判断,执行不同
//的操作.cmd参数在用户程序端由一些宏根据设备类型、序列号、传送方向、数据尺寸等生成,这个整数
//通过系统调用传递到内核中的驱动程序，再由驱动程序使用解码宏从这个整数中得到设备的类型、序列
//号、传送方向、数据尺寸等信息，然后通过switch{case}结构进行相应的操作.
int binder_write(struct binder_state *bs, void *data, unsigned len)
{
    struct binder_write_read bwr;
    int res;
    bwr.write_size = len;
    bwr.write_consumed = 0;
    bwr.write_buffer = (unsigned) data;
    bwr.read_size = 0;
    bwr.read_consumed = 0;
    bwr.read_buffer = 0;
    res = ioctl(bs->fd, BINDER_WRITE_READ, &bwr);
    if (res < 0) {
        fprintf(stderr,"binder_write: ioctl failed (%s)\n",
                strerror(errno));
    }
    return res;
}
```

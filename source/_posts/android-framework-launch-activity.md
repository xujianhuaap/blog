title: Activity 启动
date: 2016-01-12 15:50:54
tags: launch_activity
---

1.  参考文件
```
framework/base/core/java/android/app/ActivityThread.java
framework/base/core/java/android/content/pm/ActivityInfo.java
framework/base/services/java/com/android/server/am/ActivityRecord.java
```
2.  zygote该进程会fork一个子进程即app进程,该进程的入口处是Activity
Thread的main函数,主要进行如下工作:
```
public static void main(String[] args) {
  1.准备工作 开启事件追踪--事件Tag为ActivityThreadMain,
  初始化当前用户,创建Looper对象,创建MainHandler                 
  2.创建ActivityThread的实例thread,并调用attach(boolean)
  3.获得Looper 并启动它looper.loop();
  4.thread.detach();
}
```
3.  ActivityThread中的handleLauchActivity(
ActivityClientRecord r, Intent customIntent)中
的主要逻辑
```
ActivityClientRecord 主要记录一下信息:
1>Intent
2>ActivityInfo
3>LoadApk 主要包括ApplicationInfo ActivityThread packageName(String)
  appDir,libDir,dataDir,Resource,ClassLoader,Application,
  CompatibilityInfo(关于屏幕匹配的信息density,屏幕applicationscaled).
4>Window
5>Activity
6>boolean paused stoped


private void handleLaunchActivity(ActivityClientRecord r,
 Intent customIntent) {
       Activity a = performLaunchActivity(r, customIntent);
       handleResumeActivity(r.token,false,r.isForward,
         !r.activity.mFinished && !r.startsNotResumed);
}
```
4.  performLaunchActivity的逻辑如下:
```
private void Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
  1>获得ActiviytInfo-- r.activityInfo
    activityInfo 主要包含了以下信息:targgetActivity softInput theme ,permission,
    screenOrintion,launchMode.
  2>获得packageInfo --getPackageInfo(applicationInfo,compatInfo,flag)
  3>获得组件的名字--r.intent.getComponent(),如果activityInfo.tatargetActivity 不为空,创建ComponentName的对象
  component = new ComponentName(r.activityInfo.packageName,
                r.activityInfo.targetActivity);
  4>获得Activity的实例
    1>获得ClassLoader---r.packageInfo.getClassLoader();
    2>生成Activity实例---activity = mInstrumentation.newActivity(
            cl, component.getClassName(), r.intent);
  5>activity的属性设置
      1>生成Application 实例---r.packageInfo.makeApplication(false, mInstrumentation)
      2>生成Context---appContext = new ContextIml()                                     ;
      3>生成Configuration
      4>将相关属性设置到activity,生成Window,设置windowmanager-----activity.attach();
  6>activity 生命周期
      1>  if (r.isPersistable()) {
            mInstrumentation.callActivityOnCreate(activity, r.state, r.persistentState);
        } else {
            mInstrumentation.callActivityOnCreate(activity, r.state);
        }
      2> 根据条件启动start restore
}        
```

5.  handleResumeActivity()的具体逻辑
```
final void  handleResumeActivity(IBinder token,
            boolean clearHide, boolean isForward, boolean reallyResume) {

        1>调用GC
        2>执行performResumeActivity(token, clearHide)并 且获得ActivityClientRecord r
        3>设置activity在生命周期变化后一些属性,activity位于栈的顶端时自动设置软件盘
        的显示.
        4>获得Activity的window,将获得DecorView,并添加到WindowManager中
              *window是绘制顶层窗口和标题栏,以及背景
    }
  ```
6. Instrument的作用----控制创建Activity的实例和Activity的生命周期
```
public Activity newActivity(ClassLoader cl, String className,
        Intent intent)throws InstantiationException,IllegalAccessException,
        ClassNotFoundException {
          return (Activity)cl.loadClass(className).newInstance();
}
```

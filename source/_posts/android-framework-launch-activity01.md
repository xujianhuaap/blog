title: android-frameworks-launchactivity01
date: 2016-01-25 11:18:44
tags: launch_activity
---
1. 参考文件
```
frameworks/base/core/java/android/view/WindowManagerImpl.java
```
2.  Activity下的attach()
```
1> 设置有关属性
2>创建Window的实列,并且设置WindowManager
  <1>PolicyManager.makeNewWindow()-->mPolicy.makeNewWindow()--->
    new PhoneWindow()
  <2> window.setWindowmanger(wm),wm为WindowManagerImpl的实例,若其为空,
  测创建一个新的实例.然后为window设置一个LocalWindowManager(wm)
  小结:
  这里是对代理设计模式的应用,以LocalWindowManager 为例 windowManagerImpl已经
  实现了addView()为什么LocalWindowManager又重写该方法?代理模式有什么优势,在框
  架层有如此广泛的应用.首先,代理模式,可以控制实现类的访问权限.再者,代理类可以修改对
  应的operator(),这样维护了实现类的稳定性,当该operator()稳定后在移到实现类.
  一般来说,在一个类创建对象耗费资源较多时,考虑使用代理类,当确实需要调用operator()
  时才会,创建实现类的实例.当一个类功能较复杂,还有些不稳定的,可以使用代理类.需要对实
  现类访问权限限制的,也可以使用代理类.
```
3. WindowManagerImpl的addView(decorView)
```
1> 声明局部变量ViewRoot root
2> 寻找在WindowManagerImpl的成员变量mViews中查找是否有decorView如果有返回index
3> root=roots[index]//roots是ViewRooot的集合,并且设置有关属性LayoutParams,并且
   return 函数堵塞
4> 如果没有相应的decorView的,则创建相应的root,刷新roots,刷新views,root.setView()
```

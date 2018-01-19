title: android-application-customer-view
date: 2016-02-26 18:17:59
tags: android_application_customer_view
---
```
1. View layout有两个过程，一个measure,另一个是layout,其中measure主要是测量View以及子
View的大小，layout主要是控制View以及子View相对于父控件的位置的过程．
2. MotionEvent
运动事件包括鼠标，笔，指尖，轨迹球，MotionEvent所包含的事件既有相对运动也有绝对运动，取决与
设备．对运动的描述主要包括Action和axis值的集合(压力值，x,y,)．
单点触摸：ACTION_UP ACTION_DOWN ACTION_MOVE
多点触摸：ACTION_POINTER_UP ACTION_PINTERDOWN ACTION_MOVE
3. KeyEvent
4. InputEvent 无论是键盘事件还是运动事件都是输入事件．对于输入事件android有不止一种方式
来截取事件，对于输入事件的操作一般是通过接口回调来实现的，例如event listener和event
handler．事实上event listener是android 定义好的event handler,即这个方法被调用的时机，
已经被android框架设置好了，我们所需要做的就是实现这个listener 所要执行的业务逻辑．
5. 接口回调
在声明一个接口，把类的实现类的实例作为调用者的成员变量，在执行调用者的某个方法(功能)时执行
该实现类的定义的方法．在为该实现者设置对应的成员变量的地方实现实现类的方法．
6. android 首先会调用event　handler ,其次才会调用event listener;这也以为如果event
handler 消费了事件那么　event listener就没机会了．


```

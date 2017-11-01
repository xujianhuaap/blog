title: android_framework_setContentView
date: 2016-01-12 21:29:41
tags: set_content_view
---
1.  参考文件
```
framework/base/core/java/android/app/Activity.java
base/phone/com.android.internal.policy.impl.PhoneWindow.java
```
2.  在Activity中setContentView()
```
getWindow().setContentView(layoutResID);//PhoneWindow
initWindowDecorActionBar();
```
3.  在PhoneWindow中setContentView()
```
public void setContentView(View view, ViewGroup.LayoutParams params) {
       1>如果mContentParent为空调用installDecor,不为空移除mContentParent中的子View
       2>将view设置到mContentParent中
       3>mContentParent 事实上是ViewGroup 事实上是ViewGroup
}
private void installDecor() {
          1>判断DecorView 是否为空若为空调用 generateDecor()
          2>判断mContentParent是否为空,若为空,调用generateLayout(mDecor)
          并且设置标题栏的可见否
}
protected ViewGroup generateLayout(DecorView decor) {
   ViewGroup contentParent = (ViewGroup)findViewById(ID_ANDROID_CONTENT);
}
public View findViewById(int id) {
        return getDecorView().findViewById(id);
}
```
4.  DecorView
```
DecorView 本质上是FrameLayout,是window中top_level View;
```

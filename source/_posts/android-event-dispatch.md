---
title: android-event-dispatch
date: 2018-01-17 17:12:47
tags: android_event_dispatch
---
```
1. touch event 事件的分发
	MotionEvent.ACTION_DOWN = 0000 0000;
	MotionEvent.ACTION_UP = 0000 0001;
	MotionEVent.ACTION_MOVE = 0000 0010;
	MotionEvent.ACTION_CANCLE = 0000 0011;
	MotionEvent.ACTION_MASK = 1111 1111;
2. Action 分发流程
	Activity dispatchTouchEvent(){
	       	    if(window.dispatchTouchEvent()){return true};
		    return onTouchEvent();
		} 
		
		onTouchEvent(event){
			//如果activity正在退出返回true,其他默认返回false
			return false;
		}
	
	Window dispatchTouchEvent(){
		mDecorView.dispatchTouchEvent();
	}

	DecorView (extends Framelayout) dispatchTouchEvent(){
		super.dispatchTouchEvent();
	}

       ViewGroup dispatchTouchEvent(){
		若无子view，super.dispatchTouchEvent();
		如果有子view child.dispatchTouchEvent();
	//	如果子view 是ViewGroup 继续走一样的逻辑 否则,调用View.dispatchTouchEvent()
	//     	super.dispatchTouchEvent();实际就是View.dispatchTouchEvent()
	}
	

	View dispatchTouchEvent(){
		
		return onTouchEvent();

	      }

	     onTouchEvent(event){
			if(clickable){
			    // 根据MotionEvent.Action 作不同的业务处理
			    return true;
			}
			return false;
	    }
4. ViewGroup dispatchTouchEvent 具体实现步骤
	1. 若event 的Action是ActionDown,清除touchTarget.
	2. 若event.Action 不是Action_CANCEL 并且不Intercepted,
		在Action是ACTION_MOVE ACTION_DOWN 的情况下,从所有的子View集合中,遍历到
		能够接受到TouchEvent的View,并依据这个View,继续分发事件，若分发事件返回为true
		,创建newTouchTarget,并且将mFirstTouchTarget赋值给newTouchTarget.next,
                并且alreadyDispatchToNewTouchTarget设置为true 
	3. 遍历该ViewGroup的所有的touchTarget
	   若alreadyDispatchToNewTouchTarget 为true 并且 newTouchTarget == touchTarget 
		则 返回 true 事件结束
	   否则 调用dispatchTransformedTouchEvent(touchTarget.child),并且返回该返回值 
		
		dispatchTansformedTouchEvent(child,event){
			if(view != null){
			        child.dispatchTouchEvent(event);
			}else{
				// 即View 的dispatchTouchEvent()
				super.dispatchTouchEvent(event);
			}
		}
5. 小结
	1. TouchTarget数据结构 next=> touchTarget child=> view
	2. 只有存在MotionEvent.ACTION_MOVE 时,ViewGroup 的TouchTarget的链长度为大于1
	当ViewGroup没有child 时 touchTarget链的长度为0
	3. 获取焦点的view,会随着MotionEvent_ACTION_MOVE,不停的变换,因此对应的TouchTarget可能会增加
	4. 当ACTION_DOWN 或者ACTION_MOVE被拦截的情况下,就不会创建新的TouchTarget ,事件也不会继续分发
```

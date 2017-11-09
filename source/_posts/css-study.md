---
title: css-study
date: 2017-11-09 21:17:18
tags: css
---

```
1.弹性盒子模型
	容器
	display: flex;//弹性 none block inline flex
	flex-direction: row;//row row-reverse column colum-reverse 子元素排列顺序
	justify-content: flex-start;//flex-start flex-end space-around center 
	align-items: flex-start;//同上
	flex-wrap: wrap;//wrap nowrap wrap-reverse
	1> nowrap 弹性容器为单行
	2> wrap 在弹性子项溢出的部分会被放置到新行，子项内部会发生断行
	3> wrap-reverse 反转wrap
	子项
	flex: 1;//弹性的比重比
2. 常识
	1> block 元素　div p h
	   inline 元素　a span
	2> 块元素自动对齐　margin-left: auto;margin-right: right;
	3> 元素display属性能够改变<p style='display: inline'></p>
3. 元素的定位属性
	position: static;//static relative absolute fixed
	1> static 情况下没有定位　不会受到top bottom left right
	2> fixed 相对于窗口位置不变
	3> relative 相对其正常位置，受top bottom let right 影响
	4> absolute 相对与最近的绝对定位元素的位置
            受bottom let right 影响

```

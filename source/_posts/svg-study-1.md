---
title: svg_study
date: 2017-12-29 11:45:38
tags: svg
---
```
1. <svg version="1.1" xmlns="http://www.w3.org/svg"
	width="a" height="b" viewbox=" e f  c d">
	<rect width ="r1" height="r2" fill="#00ffee"/>
</svg>

viewbox 定义了显示的区域,分别对应min_x min_y 
width height .其中width 代表将Viewport 的a分成
c份，height代表将viewport的b分成d份
 
viewport 视图定义看到区域的大小
rect 显示的大小width为r1*a/c height 为r2*b/d

preserveAspectRatio 当viewport 宽高比例与viewbox的宽
高比例不一致，meet 用系数低的（Min(a/c,b/d)) slice 
用系数高的Max(a/c,b/d);
xMin 将视图平移-e      //以右为正
xMax 将视图平移(a-e-c)
YMin 将视图下移 -f
YMax 将视图下移 (b-f-d) //一下为正


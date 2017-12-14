---
title: svg-study
date: 2017-12-13 21:40:49
tags: svg
---
```
1. svg 可缩放矢量位图,是基于xml的2维图像.
2. svg 与 react
	1> 书写一个svg格式的图片文件

	<svg vsersion="1.1"
	     baseProfile="full"
	     width="300" height="200"
	     xmlns="http://www.w3.org/2000/svg"
	     xmlns:xlink="http://www.w3.org/1999/xlink">
	    <!--颜色渐变-->
	    <defs>
		<linearGradient id="Gradient1" x1="0" x2="0" y1="0" y2="1">
		    <stop class="stop1" stop-color="#FFFFFF" offset="0%"/>
		    <stop class="stop2" stop-color="#20FF80" offset="80%"/>
		    <stop class="stop3" stop-color="green" offset="100%"/>
		</linearGradient>
	    </defs>
	    <rect width="100%" height="100%" fill="#FF7F50" />

	    <circle cx="150" cy="100" r="80" fill="url(#Gradient1)" >
		<!--<animateMotion path=" M 50 200 A 100 100 0 0 0 250 200"-->
                       <!--dur="1s" repeatCount="indefinite" rotate="auto" />-->

		<animateTransform
			id="animate"
			attributeName="transform"
			begin="0s"
			dur="0.2s"
			type="rotate"
			from="0 150 100"
			to="4.5 150 100"
			fill="freeze"
			repeatCount="indefinite"
		/>
		<!--<animate attributeName="rotate" from="60" to="80" dur="1s" repeatCount="indefinite"> </animate>-->
	    </circle>

	    <circle cx="150" cy="150" r="3" fill="#EAFFDF">

		<animateTransform
			attributeName="transform"
			begin="0s"
			dur="1.8s"
			type="translate"
			from="0 0"
			to="0 -40"
			fill="freeze"
			repeatCount="indefinite"
		/>
	    </circle>

	    <circle cx="140" cy="152" r="5" fill="#EAFFDF">

		<animateTransform
			attributeName="transform"
			begin="0s"
			dur="1.3s"
			type="translate"
			from="0 0"
			to="0 -50"
			fill="freeze"
			repeatCount="indefinite"
		/>
	    </circle>

	    <circle cx="160" cy="151" r="4" fill="#EAFFDF">

		<animateTransform
			attributeName="transform"
			begin="0s"
			dur="1.6s"
			type="translate"
			from="0 0"
			to="0 -40"
			fill="freeze"
			repeatCount="indefinite"
		/>
	    </circle>

	    <text x="150" y="125" font-size="60" text-anchor="middle" fill="white">念力</text>

	</svg>


	2> 通过<imag src={logo.svg}/>
3. svg 的path
	
	1. 常用的形状直线,矩形,圆形
		a> <circle cx="10" cy="12" r="10" fill="red"/>
		b> <rect x y width="10" height="20" stroke="red" stroke-width="2" fill="green"/>//x y 表示矩形的左上点的坐标
		c> <ellipse rx= "20" ry="30" cx="10" cy="10"/>// cx cy 表示椭圆中心的坐标 rx 表示椭圆x方向半径 
	        d> <line x1="10" y1="23" x2="12" y2="23"/>
		e> <polyline points="60 110, 65 120, 70 115, 75 130, 80 125, 85 140, 90 135, 95 150, 100 145"/>//折线	
	2. path 
		<path d="M x Y H y1 V x1 L x2 x3">
		a> 贝塞尔曲线
		b> 弧线	 A rx ry degree_axie_x 0 1 x y 
			rx ry 分别表示x y 方向上的半径
			degree_axie_x椭圆旋转的角度
			0表示取小弧度 1 表示大弧度
			1 表示逆时针 0 表示顺时针
```


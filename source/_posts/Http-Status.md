---
title: Http-Status
date: 2017-11-29 18:02:17
tags: http-status
---
```
1> 304 not modify
	当请求方法是get head 的时候并且请求是有条件的，只有满足一定
	条件的时候，才返回200；
	a)通过请求头if-none-match和if-modified-since 设置可以实现请求
	成为有条件的。
	b)只有if-none-match 的值与响应头的ETag的值不一致，才会重新请求
	server,返回200;如果if-modified-since 的日期晚于响应头
	Last-Modified,则返回200; 如果if-none-match 与　if-modified－since
	同时存在，那么if-modified-since 将会忽略。
	c)响应头必然包含Cach-Control Content-Location Date Expire 
	ETag Vary;
2> 1xx （不适用于Http 1.0)
	表示请求已经收到并且理解，等候最终的响应。
	100 继续 场景 对于有request Body的请求，当body比较大时候，如果
	请求被拒绝，效率不够高。为了实现者这个目标，需要请求头中包含，
	Expect:100-continue;客户端在收到状态码100后发送request Body,
	如果返回的状态码是403（拒绝）或者405（方法不正确）z这时候不应该
	发送请求头。如果返回码是417(异常),表示请求头不应该包含Expect:100
	-continue。
  	101 表示更换协议 服务端认可这样做
3> 2xx 表示请求已经收到，处理。
	200 Ok 
	201 created 
	203 非权威信息 成功收到代理服务器的信息，传给客户端会做修改 
	204 no Content 请求已经处理，无返返回内容
	205 reset Content 请求已经被成功处理，没有内容返回但需要客户
	端
4> 3xx 表示需要额外的意图来完成请求，一般用于Url重定向
	300 多个资源可以选择
	301 资源人为移除
	304 Not Modified
	305 Use Proxy 必须使用代理才能完成
	307 Temporary Redirect 暂时使用新的Uri,然而未来的请求不变
	308 Permanent Redirect 本次以及以后的请求都需要用新的Uri,
	    同307 一样 请求方法不能变
5> 5xx 服务端错误
	500 内部错误 
	501 没实现
	502 BadGateway 代理或者网关收到来自Upstream Server的响应无效
	504 GateWay TimeOut 代理或者网关没有及时收到来自Upstream Server 的响应
	
6> 4xx 客户端错误
	400 BadRequest
        401 未授权的
	403 Forbidden 请求是有效的，但服务端拒绝，可能是此次权限不够
	404 not found  请求的资源没有发现，但未来可能出现
	405 method not allowed
	406 Not Acceptable 请求的资源不支持
	408 RequestTimeout 
```

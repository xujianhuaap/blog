---
title: nginx-https
date: 2017-11-20 18:02:32
tags: nginx https
---
```
1. 配置一个https server
      http{
	    ssl_session_cache  shared:SSL:10m;#
	    ssl_session_time_out 10m;
	    server {
		listen 443 ssl;
		server_name www.example.com;
		ssl_certificate /usr/local/www.example.com.cert;
		ssl_certificate_key /usr/local/ www.example.com.key;#隐私性要高
		ssl_protocols   TLSv1 TLSv1.2 TLSv1.1;
		ssl_ciphers     HIGH:!aNull:!MD5;
	   }
      }
     ssl_session_cache 有四个选项
	1> off 严格禁止　明确告诉服务端不能重复利用
　　　　2> none 优雅拒绝　不言明
        3> builtin 不建议这样使用，只能被一个工作进程使用　默认的
	   session 数目是2048
	4> shared: name: size
     SSL操作极度消耗CPU资源，最为耗CPU的操作是SSL握手，
     1> 保持长连接，一次连接发送多个请求
　　 2> 重复利用ssl_session 避免并行和后续连接的握手
     
     一个证书，可以有多个server 共享；
     一个地址，可以有多个server 监听；
```

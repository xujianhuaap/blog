---
title: nginx-webserver
date: 2017-11-15 17:25:22
tags: server nginx
---
```
　1>WebServer
	   http{
	       server{
		   listen 192.168.23.74:8080;
		   server_name www.baidu.com baidu.com;
		   #the rest server config
		}
	   }
	  server_name(指令）的参数可以是一个准确的名字（exact),泛型
          （wildcard),正则（regular of perl syntax)。
	　因次当有多个名字满足时候，按照一下顺序：
	　a> exact name
	  b> wildcard name *.baidu.com baidu.*
	  c> 正则的name ~(.com)$
	  在没有server_name 匹配的情况下会有一个默认的default_server.

	  http{
	     server{
		 location ~/.html?{
		    root /data;
		 }
		 location /{
		    proxy_pass baidu.com;
		 }
		#返回错误码
		 location /wrong/url{
		     return 404;
		 }
		 #重新定向
	　　　　 location /users/{
		     rewrite ^/users/(.*)$  /show?/user=$1 break;
		 }
		 location /err/ {
		    err_page 404 = 301 http://baidu.com/err.html;
		 } 
	     }
	  }
	  一个location 上下文包含处理request的指令集，提供一个静态文件或者传给代理服务器。
	  重新定向的指令有一个选项　两个参数，第一个参数，正则必须匹配request url;第二个参数
	  是替换后的url, 选项是一个标志　break(停止）last(继续）
```

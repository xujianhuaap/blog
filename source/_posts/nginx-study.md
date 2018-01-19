---
title: nginx-study
date: 2017-11-14 21:29:52
tags: nginx_server
---
```
1> nginx 作为反向代理服务器
2> 工作模型
　　一个主进程和多个工作进程，主进程管理工作进程和读取配置。
工作进程主要是处理Request，工作进程的数量，取决于配置文件(
固定的或者与内核数目有关）
　　配置文件为nginx.conf 位于/usr/local/nginx/conf
/etc/nginx 或者　/usr/local/etc/nginx
3> nginx 常用命令
nginx -s signal
signal 可以是start stop(shutdown now) reload reopen quit(shut down gracefully)
4> 配置文件
配置文件通常由一系列的指令组成，可以分为简单的指令和block指令。
http{
    server{
	 location / {
		root /data/ww;
	 }
    }
}
http 是server 的上下文，server是location的上下文, root 是简单指令，location 是块指令。
5>静态内容配置
http{
    server{
	 location / {
		root /data/ww;
	 }
	 location /images {
		root /data/images;
	 }
    }
}
匹配request 的url 如果有多个合适匹配项，则匹配最长的。
6> 简单的代理配置
http{
    server{
	location /{
	    proxy_pass http://baidu.com;
	}
        location ~\.(gif|jpg|png)${
	    root /data/images;
        }
    }
}
7> 设置CGI代理设置
http{
    server{
	location{
	    fastcgi_pass 192.168.23.78:89;
	    fastcgi_param SCRIPT_NAME $document_root/$fast_cgi_scriptname;
	    fastcgi_param QUERY_STRING $qury_string;
	}
	
    }
}
由于nginx本身不支持调用外部服务器程序的功能，需要通过FastCgi来完成。
```

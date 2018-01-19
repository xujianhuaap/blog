---
title: nginx-static_content
date: 2017-11-16 19:14:05
tags: nginx_server
---
```
1. root 指令
　　root 指令是指定用于搜寻文件的根目录，为了获取需要的文件，nginx 用root 
    指定的根目录与request URI 进行拼接。
　　该指令可以放置在http server location 的任何级别上下文（context)
    http{
	server{
		root /www/data;
		location /images/{
			autoindex on;
		}
		location / {
		}
		location ~\.(mp3|mp4){
			root /www/media/;
		}
	}
    }
   server 下的root 适用于所有的不包括root指令的location
   autoindex 指令　on是开启目录　off 是关闭目录
2. index指令
   如果request以／结尾，nginx 将视为在对应的目录下寻找所需的index文件,
   这个index 指令定义了index文件的名字（默认情况下是index.html),在返回
　 index 文件过程中，niginx 会检查文件的存在性然后内部重定向。 
　 location / {
	root /www/data;
	index index.htm index.html index.php;
   }
   
   location ~\.php {
	fastcgi_pass localhost:8000;
   }
   如果request url是/images/,如果/www/data/images/index.html　不存在
   但是index.php存在，以/images/index.php为新的url重新定向,这时候就会走
　 第二个location;
3.try_files指令
　try_files　指令是检查指定的文件和目录是否存在，如果存在则内部重定向，
  否则返回特定的错误码。
　server {
	root /www/data;
        location /images/ {
	 	try_files $uri /images/default.gif;
	}
	location / {
		try_files $uri $uri.html  =404;
	}
        location /data/{
        	try_files $uri $uri/ @backend;
	}
	location @backend{
		proxy_pass: http://locahost:8080;
	}
  }
  try_files 的最后一个参数可以是一个状态码或者location的名字
4. sendfile指令
　location /mp3 {
	sendfile on;
	#tcp_nopush 必须在sendfile on 才有效
	tcp_nopush on;
	sendfile_max_chunk 1m;
  }
  sendfile 指令避免了拷贝文件到buffer,直接拷贝文件
　sendfile_max_chunk 限定了传输数据的大小
  tcp_nopush 当开启的时候，在文件传输完成，自动把response 头封包并发送
5. 调整系统最大的网络连接数
　for linux
  sudo sysctl -w net.coresomaxconn=4096;
  你也可以通过配置文件
　　　vi /etc/sysctl.conf
  添加一下内容
　　　net.core.somaxconn=4096
  对于nginx默认值是512,r如果更大的话可以通过配置
　　　server {
	  listen 80 backlog=4096;
      }
6. tcp_nodelay 指令
　tcp_nodelay 指令重写Nagle的算法，以小的packet包解决在网络慢 的难题　　
　适用于长连接
　tcp_nodelay
  location /mp3 {
       tcp_nodelay on;
       keeplive_timeout 65;
  }
```  

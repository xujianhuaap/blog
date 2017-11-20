---
title: nginx-cache
date: 2017-11-20 10:28:24
tags: nginx cache
---
```
1. 开启响应的缓存配置
　　http{
    	proxy_cache_path /data/nginx/cache keys_zone=one:10m max_size = 200m 
                     loader_threshold=300 loader_files=100 loader_sleeps=500;
	# 设置允许发送purge 请求的地址列表　（推荐）
	geo $purge_allowed {
		default 0; # deny from other
		10.0.0.1 1; # allow from localhost
	}
        server {
	    proxy_cache one;
	    proxy_cache_min_uses 5;# http server location
	    proxy_cache_key "$host$request_uri$cookie_user"; # http server location
	    proxy_cache_methods GET HEAD POST; #http server location
	    proxy_cache_valid 200 302 10m;
	    proxy_no_cache $http_pragma $http_authorization;
	    location / {
	         proxy_pass https://baidu.com;
	    }
	}
    }
   proxy_cache_path 定义了缓存内容的路径, max_size 规定了缓存内容的最大缓存
   keys_zone 定义了缓存区的名字，以及大小（该缓存区缓存的是元数据）
   nginx 的缓存管理器周期性检查缓存的状态，当缓存超过规定的缓冲大小，就清理
　　最近缓存的内容
　
　 nginx 的缓存加载器在nginx启动之后只执行一次,它加载元数据到共享内存区域，
　　　如果一次加载会耗费太多资源，拖累nginx的性能，可以通过一下配置予以改善
　　　loade_threshold 一次遍历的时长
　　　loader_files 一次遍历的文件数目
　　　loader_sleeps 遍历间歇时长
　
　　默认情况下，响应永久呆在缓存里，我们可以设置缓存的
　　proxy_cache_valid 200 302 10m;#状态码为200 或者302 的响应时间为10分钟

    定义那些请求的缓存响应不发给请求端
　　proxy_cache_bypass  $cookie_noche $arg_nocache$arg_comment;
    只有以上三个变量　全部为０的时候，才会查取缓存。
    如果有一个变量不为零，则不查取缓存，调用服务器。
	
    定义那些请求的响应不接受缓存
　　proxy_no_cache $http_progma $http_authorization;
    
    
    清除缓存
    在http　水平上设置
　　http{
	map $request_method $purge_method{
		PURGE 1;
		default 0;
	}
    }
    在location的水平上设置
　　location {
     	proxy_cache_purge $purge_method;
    } 
    发送purge 请求(可以利用工具）
　　curl -X PURGE -D - "https:baidu.com"
```


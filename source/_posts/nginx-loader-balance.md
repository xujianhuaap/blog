---
title: nginx-loader_balance
date: 2017-11-20 22:06:11
tags: nginx loader_balance
---
```
1. 负载均衡设置
	http {
		upstream backend {
			ip_hash;# least_time header  
			server backend.example weight=5;
			server skullmind.cn ;
			#unix socket
			server unix:/var/run/nginx.sock;
			server localhost backup;
		}
		location / {
			proxy_pass http://backend;
		}
	}
	upstream 指令用于定义一组server;
	server 指令（不同于上下文server) 
	    address ip unix_socket domain
            weight=5 # 权重
            max_conn=100
	    max_fail =100 
	    fail_timeout= 10
	    backup # 标记为备份
            down; # 不可用
	
2. 负载均衡的算法
　　1> 循环
　　2> 最少连接least_conn
　　3> 权重
　　4> ip_hash 保证同一个请求的要访问的服务器会一样，
       除非服务器不可用
    5> hash (目前不建议使用)
    6>延迟最少的 least_time header least_time last_byte
3. server 监控
	upstream 指令只能在http 上下文中
	upstream backend {
		server www.data.com slow_start=30s;
		server www.data.com max_fails=10 fail_timeout=30s;
		server www.data.com max_fails=10;
	}
        max_fails 表示当连接失败达到一定次数，就认为该server 不可用
	fails_timeout 表示连接的最大时长，超过既认为连接失败

	http {	
		upstream backend {
			zone backend 64k;
		
			sever www.data.com;
			server www.data.com weight =5;
			server www.data2.com backup;
		}
		
		location / {
			proxy_pass http://backend;
			health_check;
		}
	}	
	
	周期性的发送特定的请求给每个server 检查特定的响应可以
	判断server 的可用情况
	zone指令　分配一定的工作进程共享区，存储着server 组的
		配置，能够确保，工作进程用同一套计数集合。
```

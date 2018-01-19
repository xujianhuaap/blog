---
title: nginx-reverse_proxy
date: 2017-11-17 18:52:09
tags: nginx_reverse-proxy
---
```
1. 透传请求
　　location /www/data/ {
        proxy_pass https://host:port/;
    {
    代理服务器的地址没有uri(只有ip和port),因此request 的uri直接
　　拼接上去。
    location /www/data/ {
	proxy_pass https://localhost:port/link/; 
    }
    如果代理服务器的地址包含uri(/link/)请求是/www/data/images/home.html
    那么将会代理到https://localhost:port/link/images/home.html;
2. 请求头的透传 和缓存配置
　　location /www/data/ {
       proxy_buffering on;
       #16代表缓存的请求数目　4k 代表每个请求的缓存大小；
       proxy_buffers 16 4k;
       #来自代理服务器的响应第一部分缓存大小，通常缓存响应的header ,比剩下的响应所占的缓存小；
       proxy_buffer_size 2k;
       proxy_set_header Host $host;
       proxy_set_header X-Real-Ip $remote_addr;
       proxy_pass http://baidu.com;
    }
3.  
  　如果你的nginx 服务器是一个集群，有时候需要指定一个
　 location /www/data/ {
      proxy_bind 127.0.0.1 ;
      proxy_pass http://baidu.com/;
   }
```

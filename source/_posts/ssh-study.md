---
title: ssh-study
date: 2017-11-22 22:37:52
tags: ssh scp
---
```
ssh 远程登陆，基于c/s 架构
　
ssh 常用命令
	ssh user@ip -p

$ ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
‘mkdir -p .ssh && cat >> .sh/authorized_keys’是登陆之后执行的
scp
 复制本地的文件到远程服务器
	scp file user@remote_ip:/path
 复制本地文件夹到远程服务器
	scp -r dir user@remote_ip:/path

 复制远程文件到本地
	scp user@remote_ip:/path/file /local_path
 复制远程文件夹到本地
	scp -r user@remote_ip:/path/dir /local_path

 scp 选项
	－P 端口
	
```

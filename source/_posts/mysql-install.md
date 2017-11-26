---
title: mysql_install
date: 2017-11-26 14:53:04
tags: mysql 
---
```
1. 下载压缩包
  cd /usr/local/
  wget https://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.38-linux-glibc2.12-x86_64.tar.gz
  tar -xvf mysql-5.6.38-linux-glibc2.12-x86_64.tar.gz
  rm -rf mysql-5.6.38-linux-glibc2.12-x86_64.tar.gz
2. 检查mysql组的存在
  groups mysql
  如果沒有 mysql 则添加组　mysql组
　groupadd mysql
  useradd -r -g mysql mysql
3. 重新命名
　mv mysql-5.6.38-linux-glibc2.12-x86_64 mysql
4. 改变mysql的拥有者和组属性
  cd mysql
  chown root:root ./
  chown mysql:mysql data
5. 启动server
  .suport_files/mysql.server start //启动mysql    
   如果失败查看是否已经有运行的进程
	ps -aux | grep mysql

  ./bin/mysqladmin -u root -p //初始化密码请记住密码
6. 添加到service 
 cp support_files/mysql.server /etc/init.d
 svf-rc-conf //查看服务的运行情况
 serive mysqld start
7. 登陆
　./bin/mysql -u root -p
8. 配置环境变量
　vi /etc/profile
  添加一下内容
　export PATH=$PATH:/usr/localmysql/bin　
```



               	

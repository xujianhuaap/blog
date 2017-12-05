---
title: Linux-cmd
date: 2017-11-30 17:53:15
tags: linux-cmd
---
```
1. find ~ -name '*.txt' 在当前用户下查找.txt 文件
   find ./ -name '*.txt'| rm -rf 删除当前文件夹下的.txt 文件
   find ./ -name '*txt' | rm -rf -print  
2. find ./ -name '^test*' |xargs grep -i 'xu' --color=auto 
  从当前的目录下的test的文件中 显示包含xu的内容并对xu以高亮显示
3. netstat -ant 
   -a all listen
   -n 数字化host port
   -t tcp
   -u udp
4. ps -aux 正在运行的进程(BSD-style)
   ps -ef (standard)
5. ln -s target link_name
   创建一个名为link_name,指向target(可以是文件或者目录）的软连接
   ln -t dir target
   在dir目录下创建一个target(文件）的硬连接
   ln -L target link_name 
   创建一个名为link_name 指向target(文件）的硬连接
   软连接在target失去的时候，也会失去作用
   硬连接在target 失去的时候不会失去作用，硬连接的target只能是文件
6. whereis 
   whereis -b filename
   whereis 查找特定位置的文件或者文件夹（/usr/bin /usr/local/bin /usr/local/lib等)
   -b 二进制
   -s 源文件
   -m manual 文件

```




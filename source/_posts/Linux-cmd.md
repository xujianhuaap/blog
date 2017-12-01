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
```




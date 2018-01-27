---
title: java-hash-index
date: 2018-01-24 12:13:47
tags: java_hash_index
---
```
1. mask = 2的n次方-1; // 转换二进制全是1 2的三次方 111 ; 2的四次方 1111;；
/**
* hash 是散列数,没有冲突的
**/
2. hash = new AtomicInteger(0).getAndAdd(delta);
   delta = 2 * 0x61c88647;
3. index = hash & mask;
   public int next (int index){
	return (index + 2) & mask; 
   }
```

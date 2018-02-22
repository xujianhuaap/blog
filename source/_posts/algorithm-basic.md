---
title: algorithm-basic
date: 2018-02-11 17:49:03
tags: algorithm_sort_order
---
```
1. order_direct
   a> 基本思想
     无序序列,通过二次遍历,逐步扩大有序的序列,
     事件复杂度是 n的平方
 

   b> java 的代码实现
   对arr,进行从小到大的排序
   for ( int i = 1; i< arr.len;i++){
      if (arr[i] < arr[i-1]){
	 int temp = arr[i];
	 int k = i-1;
	 for (int j= i-1;temp < arr[j];j--){
	       arr[j+1] = arr[j];
 	       k --;
	 }
	 arr[k] = temp;
      } 
   }

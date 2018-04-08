---
title: algorithm-basic
date: 2018-02-11 17:49:03
tags: algorithm_sort_order
---
```
1. direct_insert_sort
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
2. binary_insert_sort
   a> 二分插入排序,是在直接插入排序基础上实现的
   b> 代码实现
   for(int i=1;i<arr.len;i++){
       int get = arr[i];
       int left = 0;
       int right = 1;
       while(left <= right){
           int mid = (right+left)/2;
           if(get < arr[mid]){
		right = mid-1;
           }else{
                left = mid +1;
   	   }
       }
       for (int j = i-1;j>= left;j--){
		arr[j+1]=arr[j];
       }
       arr[head]=get;
   }
   
3. shell_sort
   a>  基本思想
        将记录分成若干组（gap),每一组按直接插入排序进行组内排序,
        逐渐减小gap（意味着组数越来越小,但是组内元素数量越来越多)
        直至gap 为1,即只有一组。
   b> example
       3 10 12 9 7 6 4 1 14 0
       gap = 10/2 = 5    [3,6] [10,4] [12,1] [9,14] [7,0]
       3 4  1  9 0 6 10 12 14 7
       gap = 5/2 = 2    [3, 12,7,4,14] [10,9,6,1,0]
       3 0  4  1 7  6  12  9 14 10
       gap = 2/2 = 1  [3,0,4,1,7,6,12,9,14,10]
       0 1  3  4 6  7  9   10 12 14 
4. 快速排序
    swap(arr,i,j){
	int temp = arr[i];
        arr[i] = arr[j];
        arr[j]= temp;
    }
    int partion(arr,left,right){
       
       int base = arr[right];
       int tailOfSmallPart = left;
       for(int i=left;i<right;i<++){
		
	   if(arr[i]<=base){
	        tailOfSmallPart++;	
		swap(arr,i,tailOfSmallPart);
           }
       }
       int indexOfBase = tailOfSmallPart+1;
       swap(arr,right,indexOfBase);
       return indexOfBase;
   }
   quichSort(arr,left,right){
	if(left >= right){
		return;
        }
        int index = partion(arr,left,right);
        quickSort(arr,left,index-1);
        quickSort(arr,index+1,right);
   } 
```

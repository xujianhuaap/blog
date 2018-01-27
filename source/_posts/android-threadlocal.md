---
title: android-threadlocal
date: 2018-01-19 17:04:39
tags: android_application_threadlocal
---
1.   涉及的文件
```

 sdk/android/source/android-23/android/os/java/lang/ThreadLocal
 sdk/android/source/android-23/android/os/java/lang/Thread
 sdk/android/source/android-23/adnroid/os/java/util/concurrent/atomic
```

2. ThreadLocal 简介
```
  ThreadLocal 经常用于线程自有变量的缓存,获取,移除.
  ThreadLocal的变量缓存在Thread的localvalues这个字段,
  该字段为ThreadLocal.Values,ThreadLocal.Values 是
  一个静态内部类.
  ThreadLocal 一个重要的字段是reference (WeakReference),存储的是
  该ThreadLocal的实例.
```
3. ThreadLocal.Values的主要方法
```
  // 一般适合在调用cleanup之后使用
  1. add(threadlocal,object){
        1> for(index=mask&threadlocal.hash;;next(index)){
		Object k = table[index];
		if(k == null){
			table[index] = threadlocal.reference;
			table[index+1] = object;
			return;
		}
           }	
  }
  2.  next(index){
	return (index +2) & mask;
  }
  
  3. rehash(){
	// capacity  new Object[capacity*2],表示可以缓存的key-value的数量
        // maxnumLoad capacity*2/3    size 表示存活的key-value个数
	if (size + tombSize < maxnumLoad){
	   return fase;
        }
        
        //当存活的key-value超过一半,需要按照目前的key-value数目扩从一倍的容量
	// 如果存活的key-value少于一半, 按照目前key-value数目改变容量 
	int capacity = table.length; 
	if(size > capacity /2){
		capacity *= 2;
        }
	
	Object[] oldTable = this.table;
        initializeTable(capacity);
	this.tombSize = 0;
	if(size ==0){
	   return true;
        }
	for(){
	   //清除掉 k==null k==tombstone
	   // 如果是存活的那么,就调用add(threadlocal,object)
        }
	return true;
  }
  4. cleanUp(){
	if(rehash){
	   return ;
        }
        int index =clean;
        Object[] oldTable = this.table;
	for(int counter= oldTable.length;counter>0;counter>>=1,
            index=next(index)){
	  1> Object key = oldTable[index);
	  2> if(key == null | key == TOMBSTONE ){
		continue;
             }
          3> ThreadLocal t = key.get();
             if(t== null){
		oldTable[index] = TOMBSTONE;
		oldTable{index+1] = null;
		size --;
                tombSize++;
             }
	}
        clean = index; 
  }
  5. remove(ThreadLocal key){
	//
	cleanUp();
	for(int index= key.hash & this.mask;; index= next(index) ){
               Object reference = this.table[index];
	       if(reference == key.reference){
		   this.table[index] = TOMBSTONE;
		   this.value = null; 
 		   size --;
                   this.tombSize ++;
		}
		
		if(reference == null){
			return;
		}
	}
      }
   6. put(ThreadLocal key ,Object value){
	cleanUp();
        //
	int firstTombStone =-1;
	for (int index = key.hash & this.mask;;index= next(index)){
		Object reference = this.table[index];
		if(reference == key.reference){
			this.table[index+1]= value;
			return;
		}
		if(key == null){
			if ( firstTombStone == -1){
				this.table[index]=key.reference;
				this.table[index+1]=value;
				size ++;
				return;
			}
		        this.table[firstTomeStone]=key.reference;
			this.table[firstTombStone+1] = value;
			size ++;
			tombSize --;
		}
		if(reference == TOMBSTONE && firstTombStone == -1){
			firstTombStone = index;
		}
	}
      } 
	}
  ps:   values.mask是values.table.len-1, hash 是一个标明该ThreadLocal的数字,
	也就是说不同的ThreadLocal实例的hash不同.但是hash之间是有规律的,
        values.table.capacity是2的n次方,那也就意味着有n个二进制位,而且
        values.table.capacity随时根据情况扩容,如果capacity是16(new 
        Object[16*2]),如果table长
        度是8, values.mask转成二进制,为111,hash & mask的值肯定是小于mask,
        而且值互不相同,因此可以用这个值作为index,缓存和取都可以通过它进行
        .而且我们知道,values.table的数据是以key1 value1 key2 value2...  排
        列的,key 有三种情况null bombstone key.
	values只会扩容，不会缩容
```
3. ThreadLocal的主要方法
```
  T get(){
	1. Thread t = Thread.currentThread();
	2. ThreadLocal.Values values  = values(t);
	3. if(values != null){
		[]Object table = values.table;
		int index = hash & values.mask;
		if(reference == table[index]){
			return (T) table[index+1];
		}
           }else{
		values = initValues(t);
	   }
	4. return values.getAfterMiss(this);
  }
	
  ps:
  
```

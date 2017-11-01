title: java_study_threadpoolexcutor
date: 2016-03-22 09:33:09
tags: java concurrent
---
```
java\util\concurrent\ThreadPoolExecutor.java
java\util\concurrent\FutureTask.java
java\util\concurrent\LinkedBlockingQueue.java
```
```
1. ThreadPoolExcutor 的生命周期
    1>running 接受新的任务，并且处理已排队任务
    2>shutdown 不在接受新的任务，但处理已排队的任务
    3>stop     不在接受新的任务，也不处理已排队的任务,打断正在执行的任务
    4>tidying  没有在工作的线程了，紧接着调用terminate()
    5>terminate terminate()执行完成
2.  public void execute(Runnable command) {
        1>判断当前线程的数量workcount若小于corePooleSize则创建新的线程，
        即调用addWorker(command,true).
        2>若当前线程的数量workcount大于或者等于corePooleSize。并且当前处于
        running,workQueue还未满。将任务command加入到workQueue的队尾,并且
        进行二次检查看是否ThreadPoolExcutor处于要关闭状态，这时候要执行
        reject(command)或者现存的线程已死，要开启新的线程。如果
        workQueue已经满，那么调用addWorker(command,false);
    }

    //增加新的线程  如果core为true则以corePoolSize 作为边界否则以
    //maxPoolSize作为边界。
    //
    private boolean addWorker(Runnable firstTask, boolean core) {
      1>通过ThreadPoolExcutor的状态来判断是否能够创建新的线程
      2>通过当前的workcount的数量来判断是否能够创建新的线程
    }
3.FutureTask implements RunnableFuture<T>
    private static final int NEW          = 0;
    private static final int COMPLETING   = 1;
    private static final int NORMAL       = 2;
    private static final int EXCEPTIONAL  = 3;
    private static final int CANCELLED    = 4;
    private static final int INTERRUPTING = 5;
    private static final int INTERRUPTED  = 6;
    //任务执行完的结果或者是异常  
    T report(int state)
    boolean cancle(boolean mayInterruptIfRunning)
4.
```

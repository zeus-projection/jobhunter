# AsyncTask

- onPreExecute(), UI线程；
- doInBackground(Params…) 紧接着onPreExecute执行。后台线程；
- onProgressUpdate(Progress…) 如果在doInBackground中调用publishProgress(Progress...)。在UI线程中更新进度；
- onPostExecute(Result) 操作结束之后的UI线程。

这个任务只能执行一次，即只能调用一次execute方法，否则会报错，运行时异常。

AysncTask不是被设计为处理耗时操作，耗时上限为几秒钟，如果做长耗时操作，使用Executor，ThreadPoolExecutor以及FutureTask

AsyncTask 是一个 abstract的类；

AsyncTask的缺陷，在3.0以前，最多128个线程的并发，然后10个线程等待，再多一个，就会抛出异常java.util.concurrent.RejectedExecution; 在3.0以后，无论多少任务，都会在内部单线程执行。



AsynchTask 使用静态阻塞队列来存放待执行任务，初始容量128个

本质上也是用Handler来调度



```java
 BlockingQueue<Runnable> sPoolWorkQueue = new LinkedBlockingQueue<Runnable>(128);

//静态串行任务执行器，内部实现了串行控制机制
public static final Executor SERIAL_EXECUTOR = new SerialExecutor();

//静态执行器，从队列中拿任务然后 挨个执行 scheduleNext
private static class SerialExecutor implements Executor {
  final ArrayDeque<Runnable> mTasks = new ArrayDeque<Runnable>();
  Runnable mActive;

  public synchronized void execute(final Runnable r) {
    mTasks.offer(new Runnable() {
      public void run() {
        try {
          r.run();
        } finally {
          scheduleNext();
        }
      }
    });
    if (mActive == null) {
      scheduleNext();
    }
  }

  protected synchronized void scheduleNext() {
    //从阻塞队列中拿取当前任务执行
    if ((mActive = mTasks.poll()) != null) {
      THREAD_POOL_EXECUTOR.execute(mActive);
    }
  }
  
 //一个任务有3种状态
  public enum Status {
    PENDING,
   	RUNNING,
    FINISHED
  }
  
  
  public final AsyncTask<Params, Progress, Result> execute(Params... params) {
	reutrn executeOnExecutor(sDefualtExecutor, params); 
  }
  
  //真实调用
  public final AsyncTask<Params, Progress, Result> exectueOnExecutor(Executor exec, Params... params) {
    if (mStatus != Status.PENDING) {
      switch(mStatus){
        case RUNNING:
          //抛异常 running
        case FINISHED:
          //抛异常，只能执行一次
      }
      mStatus = Status.RUNNING;
      
      onPreExecute();
      
      mWorker.mParams = params;
      
      exec.execute(mFuture);
      
      return this;
    }
  }
 
  //AsyncTask 还提供一个方法直接执行Runnable
  public static void execute(Runnable runnable) {
    sDefaultExecutor.execute(runnable);
  }
  
  //核心Handler ，更新消息和计算完成
  private static class InternalHandler extends Handler {
    @Override
    public void handleMessage(Message msg) {
      AsyncTaskResult result = (AsyncTaskResult) msg.obj;
      switch(msg.what) {
        case MESSAGE_POST_RESULT:
          result.mTask.finish(result.mData[0]);
          break;
        case MESSAGE_POST_PROGRESS:
          result.mTask.onProgressUpdate(result.mData);
          break;
      }
    }
  }
}
```



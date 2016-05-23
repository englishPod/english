#Java Homework 10 Note
####Thread switch
`CountDownLatch.wait()`, `Semaphore.join()`, etc, all of them will wait for the correct signal to continue running. But it won't influence thread switching, Thread1 is waiting, the JVM will normally change Thread1 to Thread2 to run, instead of waiting on Thread1 all the time.
r


#Java Homework 10 FAQ
####Q1. What is difference between submit and execute in Executor?
The `submit()` method is an executor framework extension introduced in `ExecutorService` interface.
Its main difference from `execute(Runnable)` is that `submit()` can accept a `Callable<V>` (whereas `execute()` accepts only `Runnable`) and returns an instance of `Future<V>`, which you can use later in the caller to retrieve the result asynchronously (potentially blocking until the computation performed by the `Callable` is compeleted.)

####Q2. How to solve java.util.concurrent.RejectedExecutionException
######1. A simple Executor Example
To demonstrate this exception we are going to create a simple Java application that uses a `ThreadPoolExecutor` to executor a number of worker threads. Let's see how you can do that.
```java
public class Worker implements Runnable{

  private int ID;

  public Worker(int id){
    ID = id;
  }

  @Override
  public void run(){
    try{
      Thread curThread = Thread.currentThread();
      System.out.println(curThread.getName()+" currently executing the task " + ID);
      Thread.sleep(500);
      System.out.println(curThread.getName()+" just completed the task "+ID);
    }catch(Exception e){
      System.out.println(e);
    }
  }

  public int getID(){
    return ID;
  }

  public void setID(int iD){
    ID = iD;
  }
}

```
And here is how you can use an `ThreadPoolExecutor` to create a number of threads to execute your tasks.

RejectedExecutionExceptionExample:
```java
public class RejectedExecutionExceptionExample{

  public static void main(String[] args){

    ExecutorService executor = new ThreadPoolExecutor(3,3,0L,TimeUnit.MILLISECONDS,new ArrayBlockingQueue<Runnable>(15));
    Worker tasks[] = new Worker[10];
    for(int i = 0; i < 10; i++){
      tasks[i] = new Worker(i);
      executor.execute(tasks[i]);
    }
    executor.shutdown();
  }

}
```
Everything went normal at this point. We create a `ThreadPoolExecutor` with pool size of 3. This means that it will create 3 threads that will be charged with the execution of our tasks-workers. As we submit new tasks to the `ThreadPoolExecutor`, he will place them in a `BlockingQueue`, as some of its 3 threads might be currently occupied with other tasks and thus the new tasks will have to wait until one of these 3 threads becomes available. In this particular case we use an `ArrayBlockingQueue` of size 15, to do this job.

######2. A simple case of RejectedExecutionException
One of the causes of `RejectedExecutionException` is when we try to **execute a new task after we've shutdown the executor**. When `shutdown()` is called upon an executor, older tasks might still progress, but it allows no more tasks to be submitted.


####Q3. Why size of Executor in ModernFortification will influence currentlyRunning int in Invasion line 85?

####Q4. Difference between Executors shutdown() and shutdownnow()
`shutdown()` will just tell the executor service that it can't accept new tasks, but the already submitted tasks continue to run.
`shutdownnwo()` will do the same AND will **try to cancel** the already submitted tasks by interrupting the relevant threads. Note that if your tasks ignore the interruption, `shutdownNow` will behave exactly the same way as `shutdown`.

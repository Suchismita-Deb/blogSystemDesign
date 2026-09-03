`ExecutorFramework` is an interface used to create and manage a pool of threads.

The Executor framework is provided by the `java.util.concurrent` package. It is a modern way of performing multithreading.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    void ExecutorServiceProject() {
        ExecutorService executorService = Executors.newFixedThreadPool(3);
    }
}
```

`Executors` is a utility class with static methods. We provide the number of threads, and it creates a pool of that size.
**Why not using the new Thread() to create a thread?**

```java
public class Main {
    void demo() {
        Thread t1 = new Thread(() -> System.out.println("Task 1"));
        t1.start();
    }
}
```
Creating a new thread for every task consumes memory and is harder to manage. Threads are also limited in the JVM.


The solution is to use `newFixedThreadPool()` to create a thread pool and reuse the same threads.

### How to execute the task using Executor Framework?

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    void ExecutorServiceProject() {
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        executorService.execute(() -> System.out.println("Task 1 " + Thread.currentThread().getName())); // The execute method takes a Runnable object.

        System.out.println("Main thread " + Thread.currentThread().getName());
    }
}
// Main Thread main
// Task 1 pool-1-thread-1

```


The order of the thead is not fixed. In total there will be 3 thread and the task will be done by 3 threads.

```java
for(int i=0;i<100;i++) {
    executorService.execute(() -> System.out.println("Task " + Thread.currentThread().getName()));
}
```
The thread will be moving in the pool and it will not create more threads.

### The execute() and submit().

```java
ExecutorService executorService = Executors.newFixedThreadPool(3);
executorService.execute(()->{
    try{
        Thread.sleep(1000);
    }
    catch(InterruptedException e) {
        throw new RuntimeException(e);
    }
    System.out.println("Email sent.");
});

executorService.execute(()->{
    try{
        Thread.sleep(1000);
    }
    catch(InterruptedException e) {
        throw new RuntimeException(e);
    }
    System.out.println("Sms sent.");
});
executorService.execute(()->{
    try{
        Thread.sleep(1000);
    }
    catch(InterruptedException e) {
        throw new RuntimeException(e);
    }
    System.out.println("Whatsapp sent.");
});
System.out.println("All message send.");

// All message send.
// Email send.
// Sms send.
// Whatsapp send.
```

There are requirement like the message should be send first then the main thread should print the message. The execute() method is not good for the task as it return void and it does the task and completes it, the requirement is to get something like that returns when the task is completed and then next task will work.

The solution use submit() and it will return Future the interface.

```java
ExecutorService executorService = Executors.newFixedThreadPool(3);
Future<?> future1 = 
executorService.submit(()->{
    try{
        Thread.sleep(1000);
    }
    catch(InterruptedException e) {
        throw new RuntimeException(e);
    }
    System.out.println("Email sent.");
});

Future<?> future2 = executorService.submit(()->{
    try{
        Thread.sleep(1000);
    }
    catch(InterruptedException e) {
        throw new RuntimeException(e);
    }
    System.out.println("Sms sent.");
});
Future<?> future1 = executorService.subit(()->{
    try{
        Thread.sleep(1000);
    }
    catch(InterruptedException e) {
        throw new RuntimeException(e);
    }
    System.out.println("Whatsapp sent.");
});
future1.get();// The get will return the task and the next will work.
future2.get();
future3.get(); // The tasks will run in parallel and when done it will wait in the get() and then the print.
System.out.println("All message send.");

// All message send.
// Email send.
// Sms send.
// Whatsapp send.
```
The submit() method takes Runnable and Callable(return value in the method) and the submit method return Future<>

In the submit method when it return something then it will directly go to the callable parameter. In the first example when going inside submit it will take runnable parameter. In the next example when going inside submit it is callable.
```java
Future<String> future1 = executorService.submit(()->{
    System.out.println("Fetch the name");
    return "Suchi";
});
// The Callable return the value and the submit will return the task.
String name = future1.get();
// The get() will wait for the task to complete and then return the value.

executorService.shutdown(); // It will stop the project.
```

Additionally execute() takes **runnable as parameter** and return type void. 

submit() with Runnable - Runnable as parameter. Retrun type is Future<?> and Future can be used to wait, cancel, verify if the task is done.

submit() with Callable - Callable as parameter. Return type is Future<V> and Future can be used to wait, cancel, verify if the task is done and **get the return value**.

### Completable Future.

CompletableFuture is a class used for multithreading. It is a modern way to create multiple threads and run asynchronous tasks.

There is ExecutorService and it returns Future then what is the need of the CompletableFuture. There are limitations in Future. The limitations and the solution is by CompletableFuture are listed below.

**Future does not have a callback Function.**
```java 
Future<String> future1 = executorService.submit(()->{
    System.out.println("Fetch the name");
    return "Suchi";
});
String name = future1.get();
```
The code in one line using the Completable Future. When the task is completed it will trigger the get() and will get the result.

```java
CompletableFuture.supplyAsync()
```
supplyAsync() executes a task asynchronously.
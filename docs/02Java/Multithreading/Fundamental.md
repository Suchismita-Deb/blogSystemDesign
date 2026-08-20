## Creating thread in Java.

### Extending the Thread class.

The thread class provides the foundation of creating thread in Java. Extend the thread class and override the run() method to define the code executed in separate thread.

```java
public class J01Thread {

    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        MyThread thread2 = new MyThread();
        thread1.start();
        thread2.start();
    }
}


public static class MyThread extends Thread {
    @Override
    public void run(){
        for(int i=0;i<3;i++) {
            System.out.println("Thread Running Name - "+Thread.currentThread().getName() + " "+i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
// The output.

// Thread Running Name - Thread-1 0
// Thread Running Name - Thread-0 0
// Thread Running Name - Thread-1 1
// Thread Running Name - Thread-0 1
// Thread Running Name - Thread-1 2
// Thread Running Name - Thread-0 2

```

### Implementing the Runnable Interface.

The Runnable interface is another way to create a thread in Java.
```java
public class J02Runnable {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        MyRunnable myRunnable1 = new MyRunnable();
        myRunnable.run();
        myRunnable1.run();
    }
}
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for(int i=0;i<5;i++){
            System.out.println("Runnable thread - "+Thread.currentThread().getName() + " "+i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```
The code is wrong here we called the run() method directly instead of the start() method. The run() method will be executed in the main thread and not in a separate thread. The correct way is to call the start() method which will create a new thread and then call the run() method.

The t1.start() will create separate thread and no start meaning calling the run() method inside main.

Runnable is a task definition and it describe hat should run the run() method. Thread is the execution and it create OS-level thread.

```java
public static void main(String[] args) {
    MyRunnable myRunnable = new MyRunnable();
    MyRunnable myRunnable1 = new MyRunnable();

    Thread t1 = new Thread(myRunnable);
    Thread t2 = new Thread(myRunnable1);
    t1.start();
    t2.start();

}
// The output.
// Runnable thread - Thread-0 0
// Runnable thread - Thread-1 0
// Runnable thread - Thread-0 1
// Runnable thread - Thread-1 1
// Runnable thread - Thread-1 2
// Runnable thread - Thread-0 2
```

### Callable Interface.
It is introduced in Java 5 and provides more alternatives to Runnable. 
It can return results and throw checked exception.

It works with Future object to get result after task completion.

It works with executor frameworks. 

```java
public class J03Callable {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(5);
        Callable<String> c1 = new MyCallable("Task 1");
        Callable<String> c2 = new MyCallable("Task 2");

        Future<String> f1 = executor.submit(c1);
        Future<String> f2 = executor.submit(c2);
        try {
            // Get result from future object.
            System.out.println("Result from first task - ");
            System.out.println(f1.get());
            System.out.println("Result from second task - ");
            System.out.println(f2.get());
        } catch (InterruptedException | ExecutionException e) {
            throw new RuntimeException(e);
        } finally {
            executor.shutdown();
        }
    }
}

class MyCallable implements Callable<String> {
    private final String name;

    public MyCallable(String name) {
        this.name = name;
    }

    @Override
    public String call() throws Exception {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < 5; i++) {
            result.append("Callable - ").append(name).append(" ");
            Thread.sleep(500);
        }
        return result.toString();
    }
}

// The output
// Result from first task -
// Callable - Task 1 Callable - Task 1 Callable - Task 1 Callable - Task 1 Callable - Task 1
// Result from second task -
// Callable - Task 2 Callable - Task 2 Callable - Task 2 Callable - Task 2 Callable - Task 2 
```
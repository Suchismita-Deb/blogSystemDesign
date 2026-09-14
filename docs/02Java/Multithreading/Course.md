### Thread.

Threads are thefuncdament uniyts of executon that alows programs to perfoem multipl etasks concurrently.

It utilize multi-core processir to improve performance.

Say a computer is 4 core CPU meaning it will perform 4 task at a moment daes that eman total 4 ? no It does context switching and at a time 4 core will work.

Process - It is an independent program with its own memory space and resources. It can contain multiple threads.

Example - Baking is a process. 
Getting the ingredients, proparing the ingredients, cleaning the kitchen are thread.Chef is doing baking is single core. Chef with 5 helper is 6 core it will be fast.

Key feature of thread - ConcurrentExecution, Resource Sharing.

### Creation of Thread.


In the LLD repo the multithreading package project is there it extends he Thread class. 
The main thread is the thread that will run in every program. A new thread is created and when the thread,start() then a new thread is spawned and will run in parallel to main thread. It will constantly run and the main thread will continue.


In the example thread1 started and then sleep and then thread2 started and then sleep. The main thread will continue to run and print the main thread running.

The production we use the Executor framework.

Callable return result and when it return result meaning it will be managed with the throw checked exception and Runnable run() return void.
Callable works with Future objects to retrieve results after task completion.
### Exceptions.
Checked Exception - The exceptions that must be either declared in the method signature using throw or handled using try-catch. IO Exception(file not found), SQL Exception, ClassNotFoundException are checked exceptions. The compiler checks at compile time whether the exception is handled or not. Its compile time.

Unchecked Exception - The exceptions that do not need to be declared or handled explicitly. These are usually programming errors, such as NullPointerException, ArrayIndexOutOfBoundsException, and ArithmeticException. The compiler does not check for these exceptions at compile time. Its runtime.

### Callable.
Callable interface works with the ExecutorService framework and not directly extending Thread. In the future1.get() the main thread will be blocked and in case its taking time then it will complete then the main will complete.

### What is thread safety and how it can be achieved?

The code that functions correctly during simultaneous execution by the multiple thread. It can be achieved through - synchronization, immutable objects, concurrent collections, atomic variables, thread-local variable.

### What's the difference between sleep() and wait()? 

The sleep() causes the current thread to pause for a specified time without releasing locks. wait() causes the current thread

to wait until another thread invokes notify() or notifyAll() on the same object, and it releases the lock on the object. 

### Thread Pool and Thread Lifecycle.

ThreadLifecycle.png

Thread Pool are the managed collections of reusable threads.

### Thread Lifecycle for the ThreadPool.

Pool Creation - When a thread pool is created, it may pre-create some threads (core threads) in the NEW state and immediately start them to RUNNABLE.

Task Execution - When a task is submitted -   
An idle thread in the pool executes the task.  
The thread's state changes according to task operations (RUNNABLE, RUNNING, BLOCKED, WAITING, etc.)  
After task completion, the thread returns to the pool (RUNNABLE state waiting for next task).

Pool Shutdown - During shutdown, threads complete their current tasks and are eventually terminated.

The task where the CPU is working and it is mainly doing something is the RUNNABLE state.

### Thread Lifecycle Management.
Interrupted Exception to allow clean thread termination.  Avoid thread leak by ensuring it does ot get stuck in WAITING or BLOCKED states - A thread waiting indefinitely for a notify signal can cause a leak. Use timeouts to prevent this while acquiring locks or waiting on conditions - M7 file in the LLD repo.

The idea of putting timeout in every place is bad as some process might take time. Alternate solution like finally then wait time long then interrupt the thread.



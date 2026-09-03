# Java Multithreading â€” Full Revision Notes

## Full Topic List

**Beginner**
1. Process vs Thread
2. Creating threads by `Thread` class vs `Runnable` interface
3. `start()` vs `run()`
4. Thread lifecycle (NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED)
5. `sleep()`, `join()`
6. Thread priority
7. Daemon threads

**Intermediate**
8. Race conditions & critical section
9. `synchronized` (method vs block)
10. Intrinsic locks / object monitor
11. `wait()`, `notify()`, `notifyAll()`
12. Producer-Consumer problem
13. Deadlock, livelock, starvation
14. `volatile` keyword
15. Java Memory Model (happens-before)

**Advanced**
16. Executor framework â€“ `ExecutorService`, `Executors`, `ThreadPoolExecutor`
17. `Callable`, `Future`, `FutureTask`
18. `CompletableFuture`
19. Locks â€“ `ReentrantLock`, `ReadWriteLock`, `StampedLock`
20. Atomic classes â€“ `AtomicInteger`, `AtomicLong`, `AtomicReference`
21. Concurrent collections â€“ `ConcurrentHashMap`, `CopyOnWriteArrayList`, `BlockingQueue`
22. Synchronizers â€“ `CountDownLatch`, `CyclicBarrier`, `Semaphore`, `Phaser`
23. `ForkJoinPool` / Fork-Join framework
24. `ThreadLocal`
25. Virtual Threads (Java 21+, Project Loom)

**âœ… Progress: Topics 1â€“3 covered in depth below, plus two deep-dive doubt sessions (OS-level threading, and cores/kernel/JVM/virtual threads/CAS/mutex). Next up: Topic 4 (Thread lifecycle).**

---

## Topic 1: Process vs Thread

A **process** is an independent running program with its own memory space (e.g., opening Chrome). A **thread** is a lightweight unit of execution *inside* a process â€” multiple threads share the same memory (heap) but each has its own stack.

Key differences:
- **Memory**: Processes don't share memory; threads within a process do.
- **Creation cost**: Processes are heavy to create; threads are cheap.
- **Communication**: Inter-process communication is complex (sockets, pipes); threads just share variables directly.
- **Crash isolation**: One process crashing doesn't kill another; one thread crashing can bring down the whole process (if unhandled).

**Why multithreading?** To do multiple things "at once" â€” e.g., a server handling many client requests, or a UI staying responsive while a file downloads in the background.

Quick mental model: think of a process as an office building, and threads as employees inside it. All employees share the same building resources (memory), but each has their own desk and to-do list (stack, program counter).

---

## Topic 2: Creating Threads “ `Thread` class vs `Runnable` interface

There are two main ways to create a thread in Java.

**Way 1: Extend `Thread`**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running in: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start(); // starts a new thread
    }
}
```

**Way 2: Implement `Runnable`**
```java
class MyTask implements Runnable {
    public void run() {
        System.out.println("Running in: " + Thread.currentThread().getName());
    }
}

public class Main {
    public static void main(String[] args) {
        Thread t1 = new Thread(new MyTask());
        t1.start();
    }
}
```

**Which one should you use?**
- Prefer `Runnable`. Java doesn't support multiple inheritance, so if your class extends `Thread`, it can't extend anything else. `Runnable` keeps your task separate from "being a thread," which is cleaner and more flexible (you can pass it to executors later â€” see Topic 16).
- Use `Thread` extension only for quick demos/learning.

**Lambda shortcut (since Java 8):**
```java
Thread t1 = new Thread(() -> {
    System.out.println("Running in: " + Thread.currentThread().getName());
});
t1.start();
```
This is the most common style you'll see in real code.

**Try it yourself:** create 3 threads that each print numbers 1 to 5 with their thread name, and run them. You'll notice the output order is *not* guaranteed â€” that's the essence of concurrency.

---

## Topic 3: `start()` vs `run()`

```java
Thread t1 = new Thread(() -> {
    System.out.println("Hello from: " + Thread.currentThread().getName());
});

t1.run();    // wrong way
t1.start();  // right way
```

**`run()`** a normal method call. It executes on the **current thread** (e.g., `main`). No new thread is created. It's like calling any regular function.

**`start()`** the real deal:
1. Creates a new OS-level thread.
2. That new thread calls `run()` internally.
3. Your code now executes concurrently with the caller.

The start() begins the thread execution and calls the run().

**Proof with code:**
```java
public class Test {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            System.out.println("run() called by: " + Thread.currentThread().getName());
        });

        System.out.println("Calling run():");
        t1.run(); // prints "main"

        System.out.println("Calling start():");
        t1.start(); // will throw an error if reused! (see below)
    }
}
```

**Important gotcha:** You can only call `start()` **once** per thread object. Calling it twice throws `IllegalThreadStateException`. If you need to run the task again, create a new `Thread` object.
A thread that completed execution cannot be restarted.

### What is thread safety and how it can be achieved?

The thread safety meaning the thread working perfectly during the execution by multiple threads. Its achieved through synchronization, immutable objects, concurrent collection, atomic variables and thread-local variables. 

### What will happen when the thread run method gives an exception?

When the thread is called like `start()` then the thread is created and not part of the main. When it gives error then the main thread will continue running.  
When the thread is called like `t.run()` directly then its like normal method call and it will execute in the main and the exception will be thrown in the main thread.

```java
public class MyThreadException {
    public static void main(String[] args) {
        ExecutorService  executor = Executors.newFixedThreadPool(1);
        Callable<Void> callable1 = new MyThreadCustomException();
        Future<Void> f1 = executor.submit(callable1);
        try {
            f1.get(); // The return type is void see its Void. When there is no return type then use Runnable.
        } catch (InterruptedException | ExecutionException e) {
            System.out.println("Main thread interrupted.");
        } finally {
            executor.shutdown();
        }
        System.out.println("Main thread continues ..... ");
    }
}

class MyThreadCustomException implements Callable<Void> {
    @Override
    public Void call() throws Exception {
        try{
            System.out.println("Error inside Callable");
            throw new RuntimeException("Simulated error inside callable.");
        } catch (Exception e){
            System.out.println("Exception in thread - "+Thread.currentThread().getName());
            throw e;
        }
    }
}

// Error inside Callable
// Exception in thread - pool-1-thread-1
// Main thread interrupted.
// Main thread continues .....

```
### What is the difference between sleep() and wait()?

sleep() causes the current thread to pause for a specified time without releasing locks. wait() causes the current thread to wait until another thread invokes notify() or notifyAll() on the same object and it releases the lock on the object.

---

## Threads at the OS Level (Semaphores & Depth)

**1. What a thread actually is in the kernel**

Every thread has a **Thread Control Block (TCB)** maintained by the OS kernel, containing:
- Thread ID
- Program counter (where execution currently is)
- Register values (saved when not running)
- Stack pointer (each thread gets its own stack, typically ~512KB-1MB default in Java/JVM)
- State (running, ready, blocked)
- Priority

The **process** owns shared resources: heap memory, open file descriptors, code segment. All threads inside it point to the *same* these things â€” that's why they can share Java objects on the heap, but each has an isolated call stack (which is why local variables are thread-safe by default).

**2. Kernel threads vs Java threads**

Modern JVMs (HotSpot, on Linux/Windows/macOS) use a **1:1 mapping**: every `java.lang.Thread` you create maps directly to one native OS thread (managed via pthreads on Linux, for example). So when you call `.start()`, the JVM makes a system call (`clone()` on Linux) that asks the OS kernel to create a real kernel-scheduled thread.

This is why threads aren't free â€” each one costs kernel memory for its TCB + stack, and creating too many (thousands) can exhaust resources. (This limitation is exactly why Virtual Threads/Project Loom exist â€” Topic 25 â€” they use an M:N model instead.)

**3. Context switching**

The CPU has a limited number of cores. If you have more runnable threads than cores, the OS scheduler **time-slices**: it runs thread A for a few milliseconds, then does a **context switch** â€” saves A's registers/program counter into its TCB, loads thread B's saved state, and resumes B. This switch has real cost (flushing CPU caches, saving/restoring registers), which is part of why excessive threading can *hurt* performance rather than help.

**4. Semaphores â€” the OS-level primitive**

A semaphore is just an **integer counter** + a **wait queue**, with two atomic operations:

- **`wait()` / `P()` / `acquire()`**: decrement the counter. If it goes negative (no permits left), the calling thread is *blocked* â€” the OS moves it from "running" to "blocked" state and puts it on the semaphore's wait queue. No CPU is wasted spinning.
- **`signal()` / `V()` / `release()`**: increment the counter. If threads are waiting, the OS wakes one up and moves it back to "ready."

The "atomic" part matters â€” the increment/decrement itself must not be interruptible mid-operation, otherwise two threads could both read the same value and corrupt the count. This is implemented at the hardware level using CPU instructions like `compare-and-swap (CAS)`.

**Binary semaphore vs counting semaphore:**
- Counting semaphore: counter can be any non-negative number â†’ allows N threads into a resource pool at once (e.g., a DB connection pool of 10).
- Binary semaphore (0 or 1): acts like a lock â€” only one thread gets in at a time. This is basically a **mutex**, with one key difference: a mutex has *ownership* (only the thread that locked it can unlock it), while a semaphore has no ownership â€” any thread can call `signal()` even if it didn't call `wait()`.

**5. One more depth layer â€” futex**

On Linux, naively blocking on every lock/semaphore operation via a full kernel syscall is expensive. So Java's `synchronized` and `ReentrantLock` (Topics 9-11, 17) use a **futex (fast userspace mutex)**: if the lock is uncontended, acquiring it is just a fast atomic CAS in user space â€” no kernel involved at all. Only when there's actual contention (someone else holds it) does the thread make a syscall to actually block and get queued by the kernel. This is why uncontended `synchronized` blocks are cheap in modern JVMs â€” a common beginner misconception is that every lock is "expensive."

---

## Cores, Kernel, JVM, Virtual Threads, CAS, Mutex

### 1. How Cores Relate to Threads

**Junior level:**
A **core** is a physical circuit that can execute instructions. A **thread** is a software-level unit of execution. If you have 4 cores, exactly 4 threads can run *truly simultaneously* â€” that's parallelism. If you spawn more threads than cores, the OS interleaves them via time-slicing (context switching) â€” that's concurrency, not parallelism. It just *looks* simultaneous because switches happen in milliseconds.

**Senior level / real-world:**
Modern CPUs use **hyper-threading (SMT)** â€” each physical core exposes 2 logical cores to the OS by having duplicate register sets share one execution pipeline. A 4-core CPU with hyperthreading shows as 8 logical processors (`Runtime.getRuntime().availableProcessors()` returns this number in Java). This matters for sizing thread pools:
- **CPU-bound tasks** (heavy computation): optimal pool size â‰ˆ number of cores (or cores+1). More threads than that just adds context-switch overhead with no gain.
- **I/O-bound tasks** (DB calls, HTTP calls): you can run *far* more threads than cores, because most threads are parked waiting on I/O, not consuming CPU. A common formula (from *Java Concurrency in Practice*): `threads = cores Ã— (1 + wait_time/compute_time)`. A server with 8 cores handling DB calls where each request waits 90% of its time might run 100-200 threads comfortably â€” only ~8 are ever actually executing on a core at any instant.

### 2. What is the Kernel

**Junior level:**
The kernel is the core of the OS â€” it manages CPU scheduling, memory, devices, and file systems. It runs in a privileged **kernel mode (ring 0)** with full hardware access. Your Java program runs in **user mode (ring 3)**, which is restricted â€” it can't touch hardware directly.

**Senior level / real-world:**
The bridge between user mode and kernel mode is a **system call (syscall)**. When your Java code calls `thread.start()`, eventually the JVM invokes `clone()` (on Linux) â€” this triggers a **mode switch** (a trap into the kernel), costing roughly 1â€“2 microseconds. The kernel creates a `task_struct` (its internal thread record), and schedules it using its scheduler (Linux uses CFS â€” Completely Fair Scheduler â€” in most current kernels). This overhead is exactly why the "thread-per-request" model breaks down at scale (the classic **C10K problem**) â€” thousands of syscalls and context switches per second become a real bottleneck.

### 3. How JVM Relates to All This

**Junior level:**
The JVM sits between your Java code and the OS. When you create a `Thread` in Java, the JVM (HotSpot) maps it **1:1 to a real OS thread** â€” every Java thread is a genuine kernel-scheduled thread.

**Senior level / real-world:**
This 1:1 mapping is expensive: each OS thread gets a dedicated stack (JVM flag `-Xss`, default ~512KBâ€“1MB). Spin up 10,000 threads â†’ that's 5â€“10GB of memory just for stacks, before you've done any real work â€” a common cause of `OutOfMemoryError` in naive thread-per-request servers. The JVM itself doesn't do scheduling for these â€” it fully delegates to the OS scheduler. GC threads, JIT compiler threads â€” all are also real OS threads managed the same way.

This limitation is *exactly* the problem Virtual Threads were built to solve.

### 4. Virtual Threads â€” Why They Exist

**Junior level:**
Virtual threads (Java 21+, Project Loom) are lightweight threads scheduled **by the JVM itself**, not the OS. You can create millions of them cheaply â€” starting at only a few hundred bytes, growing as needed, instead of a fixed 1MB stack.

**Senior level / real-world:**
This is an **M:N model**: millions of virtual threads get multiplexed onto a small pool of real OS threads (called **carrier threads**, typically = number of cores, backed by a `ForkJoinPool`). The key trick: when a virtual thread **blocks on I/O** (waiting on a DB response, HTTP call, etc.), the JVM **unmounts** it from its carrier thread and mounts a *different* waiting virtual thread onto that same carrier â€” the OS thread is never wasted sitting idle.

Real-world impact: a Spring Boot app using virtual threads with plain old blocking JDBC calls can handle 100,000 concurrent requests, because each gets a cheap virtual thread instead of an expensive OS thread â€” you get simple, readable, synchronous-looking code with the throughput characteristics that used to require reactive/async frameworks (Netty, WebFlux).

**Caveat:** virtual threads don't help CPU-bound work at all â€” you're still limited by physical cores. And if a virtual thread does something like a `synchronized` block calling native code, it can get "pinned" to its carrier thread, negating the benefit.

### 5. How a Thread Handles a DB Connection

**Junior level:**
When a thread makes a DB call, it sends a request over a socket, then **blocks** waiting for the response. While blocked, the OS moves it to a "waiting" state â€” it consumes zero CPU. When the DB response arrives (via a hardware interrupt), the kernel moves the thread back to "ready," and the scheduler eventually resumes it.

**Senior level / real-world:**
This blocking is exactly why **connection pools** (like HikariCP) exist. Two constraints stack on top of each other:
- **Thread limit**: with platform threads, only so many can exist before memory runs out (per section 3 above).
- **DB connection limit**: most DB servers cap concurrent connections (e.g., Postgres default `max_connections = 100`).

So even if you *could* spin up 5,000 platform threads, you'd exhaust the DB's connection pool long before that. With **virtual threads**, the thread-count constraint disappears â€” blocking is nearly free â€” so the *real* bottleneck shifts entirely to how many connections your database can handle, forcing you to size your connection pool deliberately regardless of thread count.

### 6. CAS (Compare-And-Swap)

**Junior level:**
CAS is an atomic CPU instruction with 3 inputs: a memory location, an expected value, and a new value. It checks: "does this memory location still hold the expected value?" If yes, it swaps in the new value. If no, it does nothing. All of this happens as **one indivisible hardware operation** â€” no thread can interrupt it mid-way.

```
if (memory[location] == expected) {
    memory[location] = newValue;
    return true;
}
return false;
```

**Senior level / real-world:**
On x86, this maps to the `CMPXCHG` instruction, which locks the relevant cache line for the operation (coordinated via the **MESI cache coherence protocol** across cores). Because it's lock-free, a failed CAS doesn't put the thread to sleep â€” it just **retries in a loop** (spin-and-retry), with zero kernel involvement, zero context switch. This is why `AtomicInteger`, `AtomicLong`, and `ConcurrentHashMap`'s internal bucket updates (Topics 20-21) vastly outperform `synchronized` under moderate contention â€” there's no OS scheduler in the loop at all.

**Known downside â€” the ABA problem:** if a value goes A â†’ B â†’ A, a CAS thinks "nothing changed" even though it did. Java solves this with `AtomicStampedReference` (adds a version counter alongside the value). Also, under *very* heavy contention, CAS retry loops can burn CPU spinning â€” at that point, a blocking mutex can actually be more efficient.

### 7. Mutex

**Junior level:**
A mutex (**mut**ual **ex**clusion) ensures only one thread accesses a critical section at a time. Unlike a semaphore, a mutex has **ownership** â€” only the thread that locked it may unlock it. Java's `synchronized` keyword and `ReentrantLock` are both mutex implementations.

**Senior level / real-world:**
At the OS level, mutex acquisition has two paths:
- **Fast path (uncontended):** just a CAS in user space â€” no syscall, extremely cheap (nanoseconds).
- **Slow path (contended):** the thread makes a **futex** syscall, and the kernel actually parks it (moves it to "blocked," removes it from CPU scheduling entirely) until the lock is released and the kernel wakes it up.

HotSpot JVM adds **adaptive spinning**: before fully parking a thread on the slow path, it'll briefly busy-wait (spin) betting the lock releases soon â€” avoiding a costly context switch if the wait turns out to be short. "Reentrant" means the *same* thread can re-acquire a lock it already holds (tracked via a hold count) without deadlocking itself â€” critical for recursive methods that lock the same object.

---

## ðŸ’» Code Snippets â€” Concepts in Practice

Each is a standalone runnable program (own `main`). Copy each into its own `.java` file matching the class name to compile and run.

### Snippet 1: Cores & Thread Pool Sizing

```java
public class CoreDemo {
    public static void main(String[] args) {
        int cores = Runtime.getRuntime().availableProcessors();
        System.out.println("Available cores (logical processors): " + cores);

        // CPU-bound pool: don't exceed core count (+1)
        int cpuBoundPoolSize = cores + 1;

        // IO-bound pool: can be much larger since threads spend most time waiting
        int ioBoundPoolSize = cores * 10; // rough estimate, depends on wait/compute ratio

        System.out.println("Recommended CPU-bound pool size: " + cpuBoundPoolSize);
        System.out.println("Recommended IO-bound pool size (approx): " + ioBoundPoolSize);
    }
}
```

### Snippet 2: Platform Thread Cost (why 1:1 OS mapping is expensive)

```java
public class PlatformThreadCost {
    public static void main(String[] args) throws InterruptedException {
        long startMem = usedMemoryMB();
        int threadCount = 5000;
        Thread[] threads = new Thread[threadCount];

        for (int i = 0; i < threadCount; i++) {
            threads[i] = new Thread(() -> {
                try { Thread.sleep(5000); } catch (InterruptedException ignored) {}
            });
            threads[i].start();
        }

        Thread.sleep(500); // let them all start
        long endMem = usedMemoryMB();

        System.out.println("Threads created: " + threadCount);
        System.out.println("Approx memory used: " + (endMem - startMem) + " MB");
        // Each platform thread reserves ~512KB-1MB stack -> 5000 threads = 2.5-5GB potential

        for (Thread t : threads) t.join();
    }

    static long usedMemoryMB() {
        Runtime rt = Runtime.getRuntime();
        return (rt.totalMemory() - rt.freeMemory()) / (1024 * 1024);
    }
}
```

### Snippet 3: Virtual Threads vs Platform Threads at Scale (Java 21+)

```java
import java.time.Duration;
import java.time.Instant;
import java.util.concurrent.*;

public class VirtualVsPlatform {
    public static void main(String[] args) throws Exception {
        int taskCount = 100_000;

        // Platform threads via bounded pool - will bottleneck
        runWithExecutor("Platform (pool of 200)",
            Executors.newFixedThreadPool(200), taskCount);

        // Virtual threads - one per task, cheap to create/park
        runWithExecutor("Virtual threads",
            Executors.newVirtualThreadPerTaskExecutor(), taskCount);
    }

    static void runWithExecutor(String label, ExecutorService executor, int taskCount) throws Exception {
        Instant start = Instant.now();
        CountDownLatch latch = new CountDownLatch(taskCount);

        for (int i = 0; i < taskCount; i++) {
            executor.submit(() -> {
                try {
                    Thread.sleep(100); // simulate blocking I/O (e.g., DB call)
                } catch (InterruptedException ignored) {}
                latch.countDown();
            });
        }

        latch.await();
        executor.shutdown();
        System.out.println(label + " took: " + Duration.between(start, Instant.now()).toMillis() + " ms");
    }
}
```
Virtual threads finish close to the ~100ms floor since blocking is nearly free. The bounded platform pool takes much longer because only 200 tasks can be "in flight" at once â€” the rest queue up.

### Snippet 4: DB Connection Pool Simulation (blocking + limited resource)

```java
import java.util.concurrent.*;

public class DbConnectionDemo {
    // Simulates a DB connection pool with limited connections (like HikariCP)
    static final Semaphore connectionPool = new Semaphore(10); // max 10 DB connections

    public static void main(String[] args) throws InterruptedException {
        ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();

        for (int i = 0; i < 50; i++) {
            int requestId = i;
            executor.submit(() -> handleRequest(requestId));
        }

        executor.shutdown();
        executor.awaitTermination(30, TimeUnit.SECONDS);
    }

    static void handleRequest(int id) {
        try {
            System.out.println("Request " + id + " waiting for DB connection...");
            connectionPool.acquire(); // blocks here if all 10 connections are busy
            System.out.println("Request " + id + " got a connection, querying DB...");
            Thread.sleep(200); // simulate DB query latency
            System.out.println("Request " + id + " done.");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            connectionPool.release(); // always return the connection
        }
    }
}
```
Even with cheap virtual threads for all 50 requests, the semaphore caps real concurrent DB work at 10 â€” the bottleneck shifts from thread count to connection count, exactly as discussed above.

### Snippet 5: CAS â€” Race Condition vs AtomicInteger

```java
import java.util.concurrent.atomic.AtomicInteger;

public class CasDemo {
    static int unsafeCounter = 0;                       // plain int - not thread-safe
    static AtomicInteger safeCounter = new AtomicInteger(0); // CAS-based

    public static void main(String[] args) throws InterruptedException {
        int threads = 10, incrementsPerThread = 100_000;

        Thread[] pool = new Thread[threads];
        for (int i = 0; i < threads; i++) {
            pool[i] = new Thread(() -> {
                for (int j = 0; j < incrementsPerThread; j++) {
                    unsafeCounter++;               // read-modify-write, NOT atomic
                    safeCounter.incrementAndGet();  // atomic via CAS loop internally
                }
            });
            pool[i].start();
        }
        for (Thread t : pool) t.join();

        int expected = threads * incrementsPerThread;
        System.out.println("Expected: " + expected);
        System.out.println("Unsafe counter (race condition): " + unsafeCounter); // usually LESS than expected
        System.out.println("Safe counter (CAS-based): " + safeCounter.get());    // always correct
    }
}
```

What `incrementAndGet()` roughly does internally:
```java
public int incrementAndGet() {
    int current, next;
    do {
        current = get();
        next = current + 1;
    } while (!compareAndSet(current, next)); // retry if another thread got there first
    return next;
}
```

### Snippet 6: Mutex â€” `synchronized` vs `ReentrantLock`

```java
import java.util.concurrent.locks.ReentrantLock;

public class MutexDemo {
    static int counter = 0;
    static final Object lockObj = new Object();
    static final ReentrantLock lock = new ReentrantLock();

    public static void main(String[] args) {
        long t1 = time(() -> incrementSynchronized(1_000_000));
        System.out.println("synchronized: " + counter + " in " + t1 + " ms");

        counter = 0;
        long t2 = time(() -> incrementWithLock(1_000_000));
        System.out.println("ReentrantLock: " + counter + " in " + t2 + " ms");
    }

    static void incrementSynchronized(int times) {
        Runnable task = () -> {
            for (int i = 0; i < times / 4; i++) {
                synchronized (lockObj) { counter++; }
            }
        };
        runFourThreads(task);
    }

    static void incrementWithLock(int times) {
        Runnable task = () -> {
            for (int i = 0; i < times / 4; i++) {
                lock.lock();
                try { counter++; } finally { lock.unlock(); } // always unlock in finally!
            }
        };
        runFourThreads(task);
    }

    static void runFourThreads(Runnable task) {
        try {
            Thread[] threads = new Thread[4];
            for (int i = 0; i < 4; i++) { threads[i] = new Thread(task); threads[i].start(); }
            for (Thread t : threads) t.join();
        } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
    }

    static long time(Runnable r) {
        long start = System.currentTimeMillis();
        r.run();
        return System.currentTimeMillis() - start;
    }
}
```
Note the `finally` block for `ReentrantLock` â€” unlike `synchronized`, which auto-releases even on an exception, a `ReentrantLock` **must** be manually unlocked or everything else waiting on it deadlocks forever.

---

## â­ï¸ Next Up

**Topic 4: Thread lifecycle** (NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED) â€” not yet covered, continue from here.

<div class="quiz-box">
<b>Difference between Future and CompletableFuture, Runnable and Callable.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>Future</th>
            <th>CompletableFuture</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Core Idea</b></td>
            <td>Represents the result of an asynchronous computation.</td>
            <td>Extends <code>Future</code> with composition and callback APIs for async workflows.</td>
        </tr>
        <tr>
            <td><b>Result Handling</b></td>
            <td>Mostly blocking retrieval via <code>get()</code>.</td>
            <td>Provides non-blocking callbacks like <code>thenAccept()</code>, <code>thenApply()</code>, and <code>thenCompose()</code>.</td>
        </tr>
        <tr>
            <td><b>Composition</b></td>
            <td>No built-in fluent chaining for multiple async stages.</td>
            <td>Supports fluent chaining, combining, and dependent async operations.</td>
        </tr>
        <tr>
            <td><b>Exception Handling</b></td>
            <td>No rich built-in exception pipeline; handling is typically done around <code>get()</code>.</td>
            <td>Built-in exception handling via <code>exceptionally()</code>, <code>handle()</code>, and <code>whenComplete()</code>.</td>
        </tr>
    </tbody>
</table>

<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>Runnable</th>
            <th>Callable</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Return Value</b></td>
            <td>Returns no result (<code>void</code>).</td>
            <td>Returns a result of type <code>V</code>.</td>
        </tr>
        <tr>
            <td><b>Exception Support</b></td>
            <td>Cannot throw checked exceptions from <code>run()</code>.</td>
            <td>Can throw checked exceptions from <code>call()</code>.</td>
        </tr>
        <tr>
            <td><b>Typical Use</b></td>
            <td>Best for fire-and-forget tasks.</td>
            <td>Best for tasks that compute and return a value.</td>
        </tr>
        <tr>
            <td><b>Method Signature</b></td>
            <td><code>public abstract void run();</code></td>
            <td><code>V call() throws Exception;</code></td>
        </tr>
    </tbody>
</table>
</details>
</div>

### A counter incremented by multiple threads inside a synchronized method is still producing wrong totals what are the possible causes ?
The solution involves for us to save the whole method is entirely synchronized on just part of it.  
The next thing to see if multiple objects are being used as locks instead of one shared locsk.  
In case the counter is itself has a non atomic Read modify write happening outside the synchronized block 

### What is multithreading in Java ?
Multithreading allows multiple throws to execute simultaneously maximizing the Super utilization and it can be implemented by extending the thread class or implementing through another interface.


### What is the difference between wait and sleep in Java?

Wait is used in multithreading to pause the current thread until it's notified and it releases the monitor lock.  
Sleep pauses the current thread for a specified time but does not release any locks.

### What is the purpose of the volatile keyboard in Java?
The virtual keyboard ensures that a variable's value is always read from the main memory not from the thread's local cache. It helps maintain a consistency in multithreaded environment.

### What is the thread in Java how it is different from process ?
A thread is a lightweight sub process the smallest unit of CPU scheduling. Multiple threads within the same process shared memory while processes are independent and do not share memory.

### What is teh difference between notify and notifyAll in Java?
Notify wakes up a single thread that is waiting on the object's monitor, while notifyAll wakes up
all threads that are waiting on the object's monitor. Notify is used when only one thread needs to be awakened, while notifyAll is used when multiple threads may need to proceed.


### What is the difference between a shallow copy and a deep copy in Java?
- A shallow copy creates a new object but copies references to the original object's fields, meaning changes to mutable fields in the original object will affect the shallow copy.
    - A deep copy creates a new object and recursively copies all fields, ensuring that the new object is completely independent of the original, including any mutable objects it contains.




### What is the difference between callable and runnable in Java?
- A `Runnable` is an interface that represents a task that can be executed by a thread, but it does not return a result or throw checked exceptions.
    - A `Callable` is a similar interface that represents a task that can be executed by a thread, but it can return a result and throw checked exceptions, making it more flexible for concurrent programming.


### What is the difference between synchronized and lock in Java?

The synchronized keyword provides a special mechanism to control access to critical sections. Lock is more flexible and powerful mechanism from the Java.util.concurrent package offering additional features like time locks and interruptible locks.

### What is deadlock in java?

Adidas cockles went two or more threads are waiting for each other's resources causing them to remain in a waiting state for a wrapper it is a commonly threatening issue that should be avoided.

### What is reflection in Java?
Reflection is a feature that allows a program to inspect and modify its own structure and behaviour at runtime. It is often used for dynamic method invocation and inspecting classes methods and fields.



### What is eh fork/join framework in Java?
The Fork/Join framework is a parallel programming framework introduced in Java 7 that allows for efficient.

### What is the executor framework in Java?
The Executor framework is a high-level API introduced in Java 5 that provides a way to manage and control thread execution, allowing developers to submit tasks for execution without having to manage thread creation and lifecycle.





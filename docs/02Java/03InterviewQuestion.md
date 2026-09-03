### How you are using Multithreading in your application.

In real applications, we usually do **not** create threads manually using `new Thread(...)`.
We use the **Executor Framework** so that thread creation, reuse, queueing, and shutdown are handled in a controlled way.

Typical use cases:
- Sending email/SMS in background
- Calling multiple downstream services in parallel
- File processing and report generation
- Scheduled jobs and batch operations
- Improving API response time by running independent tasks concurrently

If we need a return value from each task, we use:
- `Callable<T>` instead of `Runnable`
- `Future<T>` or `CompletableFuture<T>` to get the result

Example:

```java
ExecutorService executorService = Executors.newFixedThreadPool(3);

Callable<String> userTask = () -> "User loaded";
Callable<String> orderTask = () -> "Order loaded";

Future<String> userFuture = executorService.submit(userTask);
Future<String> orderFuture = executorService.submit(orderTask);

System.out.println(userFuture.get());
System.out.println(orderFuture.get());

executorService.shutdown();
```

So in interview language -

> We use multithreading through the Executor Framework to run independent tasks in parallel. For tasks that return data, we use `Callable` with `Future` or `CompletableFuture`.


## How to configure the Executor Framework in Spring Boot and how to use them.

First we need to understand **why we need it**.

### Why do we need the Executor Framework?

If we create a new thread for every request or every task:
- thread creation becomes expensive
- too many threads can consume a lot of memory
- context switching increases
- application performance becomes unstable
- there is no central control over concurrency

The **Executor Framework** solves this by using a **thread pool**.

Benefits:
- Reuses threads instead of creating them again and again
- Controls how many tasks run in parallel
- Improves performance and throughput
- Makes error handling, monitoring, and shutdown easier
- Fits very well with Spring Boot async processing

### How to configure Executor Framework in Spring Boot

In Spring Boot, the common approach is:
1. Enable async processing using `@EnableAsync`
2. Create a `ThreadPoolTaskExecutor` bean
3. Use `@Async` on service methods
4. Return `CompletableFuture<T>` if a result is needed

### 1. Configuration class

```java
import java.util.concurrent.Executor;
import java.util.concurrent.ThreadPoolExecutor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
@EnableAsync
public class AsyncConfig {

	@Bean(name = "applicationTaskExecutor")
	public Executor applicationTaskExecutor() {
		ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
		executor.setCorePoolSize(5);
		executor.setMaxPoolSize(10);
		executor.setQueueCapacity(100);
		executor.setThreadNamePrefix("app-exec-");
		executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
		executor.initialize();
		return executor;
	}
}
```

### Meaning of important properties

- `corePoolSize`: minimum number of worker threads kept ready
- `maxPoolSize`: maximum number of threads allowed during high load
- `queueCapacity`: how many tasks can wait in queue before new threads are created
- `threadNamePrefix`: helps identify executor threads in logs
- `RejectedExecutionHandler`: decides what happens when pool and queue are full

## How to use it in Spring Boot

### 2. Async service method

```java
import java.util.concurrent.CompletableFuture;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

@Service
public class ReportService {

	@Async("applicationTaskExecutor")
	public CompletableFuture<String> generateReport(Long userId) {
		System.out.println("Running on thread: " + Thread.currentThread().getName());

		// simulate time-consuming work
		String result = "Report generated for user: " + userId;

		return CompletableFuture.completedFuture(result);
	}
}
```

### 3. Calling the async method

```java
import java.util.concurrent.CompletableFuture;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ReportController {

	private final ReportService reportService;

	public ReportController(ReportService reportService) {
		this.reportService = reportService;
	}

	@GetMapping("/reports/{userId}")
	public CompletableFuture<String> generate(@PathVariable Long userId) {
		return reportService.generateReport(userId);
	}
}
```

## If no return value is needed

Use `void` with `@Async`:

```java
@Async("applicationTaskExecutor")
public void sendEmail(String to) {
	// send email in background
}
```

## If you want to submit tasks manually

Instead of `@Async`, you can inject the executor and call it directly.

```java
import java.util.concurrent.Executor;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;

@Service
public class NotificationService {

	private final Executor executor;

	public NotificationService(@Qualifier("applicationTaskExecutor") Executor executor) {
		this.executor = executor;
	}

	public void triggerNotification() {
		executor.execute(() -> {
			System.out.println("Notification sent by " + Thread.currentThread().getName());
		});
	}
}
```

## Interview answer in short

> We use the Executor Framework because creating threads manually is expensive and difficult to manage at scale. In Spring Boot, we configure a `ThreadPoolTaskExecutor` bean, enable async execution using `@EnableAsync`, and use `@Async` on service methods. If a task returns data, we use `CompletableFuture`; otherwise, we run it as a fire-and-forget background task.

## Best practices

- Keep separate executors for separate workloads if needed
- Use smaller pools for CPU-bound tasks
- Use comparatively larger pools for I/O-bound tasks
- Always monitor queue size and thread usage in production
- Handle exceptions properly in async methods
- Gracefully shut down executors during application stop

## Common real-time examples

- Order placed -> send email, SMS, invoice generation asynchronously
- Dashboard API -> fetch user, orders, and payment details in parallel
- Upload flow -> virus scan and thumbnail generation in background
- Batch job -> process records using controlled parallelism


### How to handle a case where multiple thread needs to access a shared resource withoyt using the synchronized keyword.
Commonly volatile keyword.

### Volatile keyword solves concurrent issue?

#### We are using counter and many thread are changing the counter and volatile keyword is not used for it.


Boolean we use the volatile and for numeric we dont use.

### What are the different states of thread in Java?

### Describe a situation when the thread remains indefinely in waiting state and how to resolve it.

When two or more thread waits for each other completion like releasing locks. Thread dumps to see the logs and what is mainly its waiting for. The solution is to use the `tryLock` method with a timeout to avoid indefinite waiting.

### What will happen when the run method is directly called without the start method?

When the `run()` method is called directly, it will execute in the current thread instead of creating a new thread. This means that the code inside the `run()` method will run synchronously, and the current thread will block until the `run()` method completes. The new thread will not be created, and the benefits of multithreading (like concurrent execution) will not be realized. It will run the method in the current thread like normal method call.

### The synchronized keyword works in Java?
The `synchronized` keyword in Java is used to control access to a block of code or method by multiple threads. It ensures that only one thread can execute the synchronized code at a time for a given object, preventing race conditions and ensuring thread safety.

When a thread enters a synchronized block or method, it acquires a lock on the object (or class, if it's a static method). Other threads that attempt to enter the same synchronized block or method will be blocked until the lock is released by the first thread. This mechanism helps maintain data consistency and prevents concurrent modification issues in multi-threaded environments.

When thread t1 enters a synchronized block, it acquires the lock on the object. If thread t2 tries to enter the same synchronized block while t1 holds the lock, t2 will be blocked and will have to wait until t1 exits the synchronized block and releases the lock. This ensures that only one thread can execute the synchronized code at a time, maintaining thread safety.
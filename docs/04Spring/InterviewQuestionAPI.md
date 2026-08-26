```markmap
##Spring Core Internal
- ApplicationContext
- BeanDefinition
- Dependency resolution
- BeanLifecycle - @PostConstruct, InitializingBean, BeanPostProcessor, @PreDestroy
- 
```

### How does Application Context create bean?

Where would you implement custom initialization logic?
How does Spring resolve circular dependencies?
Why does constructor injection generally fail for circular dependencies?
How does Spring's three-level singleton cache work?
What happens internally when you annotate a class with @Transactional?
Proxy creation
Transaction interceptor
Transaction manager
Commit/rollback
Explain Spring AOP and its limitations.

Explain transaction propagation in Spring.
Compare:
REQUIRED
REQUIRES_NEW
NESTED
SUPPORTS
NOT_SUPPORTED
MANDATORY
NEVER
Give a real-world example where REQUIRES_NEW is useful.
What happens if an exception occurs inside a @Transactional method?
Discuss:
@Transactional
public void process() {
saveA();
saveB();
throw new RuntimeException();
}
What gets rolled back? What changes if the exception is checked?
Why doesn't @Transactional work in some situations?
Explain cases such as:
private methods
self-invocation
final methods/classes
manually created objects using new
calls occurring before the Spring proxy is created
How would you handle a transaction that updates DB and publishes a Kafka event?
For example:
DB update
↓
Kafka event
How would you prevent:
DB SUCCESS
Kafka FAILURE
from creating inconsistent state?
Discuss Transactional Outbox and other approaches.
How would you diagnose database connection pool exhaustion in a Spring Boot application?
Discuss:
HikariCP
connection leaks
long-running queries
pool size
transaction duration
thread pool vs connection pool
Spring Boot Internals & Production

How does Spring Boot Auto Configuration work internally?
Explain:
@SpringBootApplication
and how Spring Boot decides whether to create a particular bean.
Discuss:
@EnableAutoConfiguration
conditional annotations
@ConditionalOnClass
@ConditionalOnMissingBean
auto-configuration ordering
How would you optimize a Spring Boot API that has high latency?
Suppose:
p50 = 100 ms
p95 = 2 sec
p99 = 8 sec
What would you investigate first?
Consider:
API
↓
Thread Pool
↓
DB
↓
Redis
↓
Kafka
↓
External APIs
How do Spring Boot applications handle concurrency?
What happens when 10,000 requests hit a Spring Boot REST API?
Discuss:
Tomcat/Netty threads
request threads
thread pools
blocking I/O
connection pools
backpressure
asynchronous processing
When would you choose Spring MVC vs Spring WebFlux?
Explain:
Spring MVC
vs
Spring WebFlux
Also explain why simply changing MVC to WebFlux doesn't automatically make an application faster.
Distributed Systems
How would you design retries for a Spring Boot service calling another service?
Example:
Order Service
↓
Payment Service
Discuss:
timeout
retry
exponential backoff
jitter
circuit breaker
idempotency
retry storms
How would you implement a Circuit Breaker in Spring Boot?
Explain the states:
failure
┌──────────────────┐
↓                  │
CLOSED ───────────→ OPEN
↑                  │
│                  ↓
└──────────── HALF_OPEN
What metrics determine when the circuit should open?
How would you make a Spring Boot API idempotent?
Suppose the client sends:
POST /payments
Idempotency-Key: abc123
The client times out and retries.
How would you ensure the payment isn't processed twice?
Security & Architecture
Explain how Spring Security authentication works internally.
Walk through:
HTTP Request
↓
SecurityFilterChain
↓
AuthenticationFilter
↓
AuthenticationManager
↓
AuthenticationProvider
↓
UserDetailsService
↓
SecurityContext
↓
Controller
Then explain how this changes for JWT-based authentication.
Design a production-grade Spring Boot microservice architecture.
You have:
API Gateway
↓
Order Service
↓
Payment Service
↓
Inventory Service
Design the system considering:
authentication/authorization
service-to-service communication
Kafka
database transactions
distributed transactions
retries
circuit breakers
idempotency
caching
observability
tracing
metrics
centralized configuration
deployment on Kubernetes

How does Spring decide which bean to inject when multiple beans implement the same interface?
@Primary
@Qualifier
bean name
collection injection
what happens if ambiguity remains?
What is the difference between BeanPostProcessor and BeanFactoryPostProcessor?
When are they executed?
What can each modify?
Give a practical use case for both.
How does Spring create proxies for @Transactional, @Async, @Cacheable, and custom AOP annotations?
Are multiple proxies created?
How are multiple advisors ordered?
What happens if two BeanPostProcessors modify the same bean?
How does ordering work?
Explain Ordered and @Order.
Explain Spring's singleton scope.
Is a Spring singleton:
Thread Safe?
What happens when multiple requests access the same singleton bean concurrently?
Transactions & Persistence
Consider this code:
@Transactional
public void methodA() {
methodB();
}

@Transactional(propagation = REQUIRES_NEW)
public void methodB() {
// DB operation
}
If methodA() and methodB() are in the same class, does REQUIRES_NEW actually create a new transaction? Why?
How does Spring handle transaction isolation levels?
Compare:
READ_UNCOMMITTED
READ_COMMITTED
REPEATABLE_READ
SERIALIZABLE
What problems do they prevent?
Dirty Read
Non-repeatable Read
Phantom Read
How would you debug a transaction that is unexpectedly holding database locks for several seconds?
What would you inspect?
Application logs
↓
Transaction duration
↓
SQL queries
↓
DB locks
↓
Connection pool
What is the difference between optimistic and pessimistic locking in JPA?
Explain:
@Version
private Long version;
When would you choose each approach?
What problems can Hibernate's persistence context cause in a high-volume application?
Discuss:
first-level cache
dirty checking
memory consumption
N+1 queries
lazy loading
batch inserts
Performance & Concurrency
Your Spring Boot service suddenly starts throwing:
java.util.concurrent.RejectedExecutionException
How would you investigate it?
Discuss:
executor configuration
queue size
thread count
task duration
traffic spike
backpressure
What is the difference between these thread pools?
@Async
CompletableFuture
Executors
ThreadPoolTaskExecutor
Which one would you use in a Spring Boot production application and why?
Suppose your API has 500 Tomcat threads but only 50 database connections.
What happens when 500 requests simultaneously execute DB queries?
How would you size:
HTTP thread pool
DB connection pool
Kafka consumer threads
Async executor
Your API works perfectly with 100 requests/sec but becomes unstable at 2,000 requests/sec. How would you find the bottleneck?
Explain the metrics you would monitor:
CPU
Memory
GC
Threads
DB connections
DB latency
Kafka lag
HTTP latency
External API latency
How does JVM garbage collection affect Spring Boot API latency?
Suppose:
p95 = 100 ms
p99 = 4 seconds
How would you determine whether GC is responsible?
Microservices & Distributed Systems
How would you implement graceful shutdown for a Spring Boot service running on Kubernetes?
Consider:
Pod receives SIGTERM
↓
Stop accepting traffic
↓
Finish existing requests
↓
Stop Kafka consumers
↓
Commit offsets
↓
Close DB connections
↓
Pod terminates
What can go wrong if termination happens too quickly?
A Kafka consumer processes a message and then crashes before committing the offset. What happens when it restarts?
How would you design the consumer to avoid duplicate business processing?
Discuss:
idempotency
offset commits
transactions
database constraints
exactly-once vs at-least-once processing
How would you prevent cascading failures across Spring Boot microservices?
Example:
API Gateway
↓
Order
↓
Payment
↓
Fraud
↓
External Bank
If the bank becomes slow, how do you prevent the entire system from running out of threads?
Discuss:
timeout
circuit breaker
bulkhead
rate limiting
bounded queues
fallback
Production Architecture
Your Spring Boot application has the following architecture:
Client
↓
Load Balancer
↓
Kubernetes
↓
Spring Boot
↓
Redis
↓
PostgreSQL
↓
Kafka
The application occasionally returns 500, but application logs show nothing useful.
How would you design observability so you can trace one request across the entire system?
Discuss:
correlation ID
distributed tracing
OpenTelemetry
metrics
structured logging
trace/span IDs
Kafka message correlation
Your Spring Boot service is deployed with 10 Kubernetes pods. One pod starts consuming significantly more Kafka messages than the others. How would you investigate it?
Think about:
Kafka partitions
↓
Consumer groups
↓
Partition assignment
↓
Consumer instances
↓
Pod distribution
What happens if you have 20 pods but only 8 Kafka partitions?
🔥 The level I'd expect at 5 YOE
For experienced interviews, I'd especially prepare these 10 scenarios:
Area
Question
Spring
Bean lifecycle + BeanPostProcessor
AOP
Proxy creation + self-invocation
Transactions
Propagation + isolation
JPA
N+1 + locking + persistence context
Concurrency
Thread pool exhaustion
Performance
DB pool vs HTTP threads
Kafka
Duplicate processing
Distributed
Retry + timeout + circuit breaker
Kubernetes
Graceful shutdown
Observability
End-to-end distributed tracing



### What exactly happens when a Spring Boot application starts?

The main flow goes like - `main() - SpringApplication.run() - Environment - ApplicationContext - Auto Configuration - Bean creation - Embedded server - ApplicationReadyEvent`

The JVM starts execution at the main() method. 
```java
public static void main(String[] args) {
    SpringApplication.run(MyApp.class, args);
}
```
**SpringApplication.run()** - This is where Boot takes over.

It sets up the SpringApplication object, configures defaults, prepares logging, and kicks off the startup sequence.

It acts as the orchestrator: it decides what environment, context, and configurations to load.

**Environment** - Boot builds the Environment (profiles, properties, system variables).

It merges values from:

application.properties / application.yml

Environment variables

Command-line arguments

This is critical because beans and auto-configurations often depend on property values.
Senior Engineering Interview Style.

Dont make the technical answer like Definition - Example - Stop.

Senior Engineering technical answer **Context - Decision - Implementation - Tradeoff - Failure/edge cases - Outcome.**

The target is to **Drive the interview** and answer say like 3 mins in the question to give depth and also manage all time.

Context (Build the hype or give answer immediately and explain where it matters in your system) - Architecture/Implementation (how you implemented it) - Trade-off (Why this approach?) - Failure modes (Show that what happens when things go wrong) - Outcome (metrics in case you have worked with it) - Controlled hooks (end with hook like "The most important part was how we handled Kafka consumer failures and idempotency as they became important at scale)

Thinks you should know as Java Backend Software Developer working in Event Driven and cloud system with AI driven environment.

```markmap
## RoadMap
### Java
- OOPS.
- Collections.
- HashMap Internal(equals/hashCode)
- Immutability
- Exception
- Generics
- Streams
- Concurrency
- Threads, Executors, Completable Futures
- Synchronization locks.
- JVM Basics
- Memory and GC fundamentals.

### Spring Boot
- DI and IOC
- Bean lifecycle
- Scopes
- Auto-configuration and configuration
- REST
- Validation
- Exception Handling
- Transaction
- Spring Security Fundamentals
- Actuators
- Autowire and Annotation creation
- Database Integration

### Microservice
- REST, HTTP
- Versioning
- Retries, Timeouts
- Rate limiting
- Circuit Breaker
- Service-To-Service Communication

### Event Driven
- Topic, Partition, Offset
- Consumer Group, Ordering, Delivery Semantics
- Rebalancing, Retries, Dead Letter Queue
- Schema Evolution
- Consumer lag, throughput, backpressure

### Distributed System
- Horizontal Scaling
- CAP
- Replication, Partitioning, Caching
- Distributed locking, Retries, Timeouts
- Failure Isolation, Message Delivery. Service discovery
- Load balancing

### Observability
- Latency, Throughput, Error Rate, Satuaration
- p95/p99
- Correalation Id
- Distributed Tracing
- OpenTelemetry concept
- Dashboard, alert, bottleneck isolation, profiling
- JVM Monitoring
- Metrics, Tracing

### CI/CD
- Git, Branching, PR, Code review, Merge, Rebase
- Build pipelines
- Deployment strategy - rolling update, canary, blue-green, shadow deployment
- Artifact management, Environment management, Secrets management
- Quality gates, Test coverage, Code quality

### AI ML
- LLM Fundamental
- Agents vs workflow
- Tool calling
- MCP, MCP cleint/ server concept
- Context
- RAG Fundamentals
- Embeddings, Vector DB
- Evaluation, guardrails
- AI Observability
- Model selection
- Security
```

### JD is teh target, Resume is the evidence.
Tell about your project.


Kafka Topic - MED_MULTI_MASTER topic and get the data from the topic and using microservice modify the data and push to the database and downstream application using the internal API call and update the table so that other microservice can use it.

To frame the answer dont directly go with the technology. Mention the design **Business Events - Ingestion - Processing - Enrichment - Persistence - Downstream - Distribution - Observability.**

Upstream order event - Topic(Order Event) - SpringBoot service (Consume, Validate, Transforms, Enrich) - Database persistence and Kafka Downstream - Downstream Consumer - Internal API call using API gateway.

Sample answer model.
> One of the backend system I have worked on is a Spring Boot based microservice responsible for processing order-related events coming through Kafka and preparing them for the downstream consumers.

> From the business perspective the service sits between the upstream order-event stream and multiple downstream systems that needs different representations of the same business data. The challenges was that the source events contains more and entire data that every consumer needs so teh service performs validation, transformation and enrichment before distributing the relevant data.

> Architecturally the flow starts with an auto event arriving into the Kafka topic and my spring boot service consumes that event, validates the payload, transform it into the downstream specific representations. The additional information which is recovered we get that through internal API call data sources or services. The downstream requirements we process the data and then we publish to another topic to send through an internal API call.

> My ownership is primarily around the microservice implementation and its event processing flow including Kafka integration the transform logic and the downstream communication and all sort of production support speed debugging or be it live support. I am not independently owning the entire platform architecture the broader architecture is and the requirement were discussed with the lead and then proposed and enterprise wise but I do own the implementation of my service and associated changes.

In case you feel to add more.


> One of the production issue that I would love to discuss that we have encountered was the latency and the consumer and producer lag in the part of the pipeline. I had to investigate the entire data flow for data movement rather than assuming it's kafka's fault I looked into the processing time, logging overhead and producer configuration and finally the solution was optimizing few of the Kafka configuration including the batching and linger Ms. like timeline and also reducing few of the logging in our system.

> The important consideration was the asynchronous processing introduce its own problem, particularly failure handling, ordering and duplicate processing so we had to make sure that making the system asynchronous is not compromising the correctness of the data.  
In terms of the performance of our design it actually changes the contribution in the throughput and it's consistent with the application around asynchronous processing using `CompletableFuture`, `KafkaTemplate` and synchronous inter service communication using `RestTemplate`.
### Points to notice.

While describing the project and your ownership do not add 10 technologies, you should add the ownership, architecture, boundaries, decision making, failure awareness, trade-off, correctness, performance, reasoning that makes sound senior 

### Explain the project data and event in depth.

Start with an event - Add the validation like DLQ - Data Enrichment - Downstream pattern.

Sample answer.
Start with an event say the business event is pushed into the topic then my microservice will consume it - There are some validation and error data will go to the DLQ topic and separate consumer and reprocessing flow - The data has a key like the customer id and Loc value say MH03 like Manhattan and the partition maintains the orders - The order data does not contain everything there is data enrichment and data processing flow - There are many downstream patterns like kafka server db and kafka service API.

The point of validation should be clear like when to validate the data there are structural validation and business logic validation.
Structural validation - Field missing, malformed JSON, invalid data type, schema violation.  
Business Validation - Status paid but amount missing, invalid order state transition, impossible quantity.
The point is we perform payload-level validation first then followed by the business validation. The invalid events are routed to the DLQ rather than entering the downstream processing path.

In case they ask like how did you implement the idempotent processing and all and in case have not worked on it then mention - Idempotent processing wasn't something I personally implemented in this flow, so I don't want to overstate my ownership. I understand it as an important concern in event-driven systems, particularly because a consumer can potentially process the same event more than once.

Sample answer.

> The order event and the data lifecycle. The Upstream system publishes an order event to the Kafka topic using the business key based on the customer ID and the location information. Kafka loses that key to print determine the partition. When the service consumes the event the first stages of validation. We verify the required fields are presnt and then teh business-critical values are present and valid. Events that dont staisfies the validation are separated in the DLQ topic rather than further processing.

> The valid events are processed by my service and perform enrichment. The original events contains order level information. The downstream application needs additional information like the user or the business information. We retrieved the required information from the relevant data source, combine it with the events and transform it into the representation required for each downstream application.  
> 
> The data that follow different paths in case of analytics related consumers we process the transformed information to a database table that the analytics system consumes and for operational downstream services we send the relevant subsets through our internal API integration and in other events the transformed event can be published for downstream consumption.
> 
> The important point that my service acts as a transformation and integration boundary between the source event system and the downstream consumers. My ownership is primarily around the service implementation, Kafka integration, transformation or enrichment of the logic and the downstream integrations.
> 
> I also have to deal with the performance issues around the event processing and lags. I investigated the processing path and turned the Kafka producer behaviour including batches and linger.ms and also reducing unnecessary logging and improving the performance.


The follow up question like - Why this partition key? What is the process with the DLQ topic? How are you preventing the duplicate events? How are you making sure of the atleast once semantics?

Mention in case I have nor worked in the idempotency part and you dont own that part but you know what it does.























































### Planning.

Topic	Frequency/pattern	What to prepare
LLD	Extremely common	E-commerce, marketplace, Instagram, food delivery, parking lot, ride sharing
Distributed Systems	Extremely common	consistency, CAP, distributed locks, leader election, backpressure, outbox
Medium DSA	Extremely common	sliding window, binary search, arrays/strings
Database design	Very common	schema, indexes, transactions, locking
Kafka/event-driven systems	Very relevant for backend	partitions, consumer groups, offsets, delivery semantics
Project deep dive	Very common	architecture, scaling, failures, tradeoffs

Recent reports specifically mention distributed locks, leader election, outbox pattern, backpressure and consistency under network partitions.

Another recent report had binary search + system design + design patterns + distributed-system consistency + caching + event-based file processing + current-project deep dive.

1. DSA — What I would practice

Don't spend your next 7 days doing 100 random LeetCode problems.

Kotak's DSA examples include:

Minimum Size Subarray Sum
Search in Rotated Sorted Array
Roman to Integer
Remove Duplicates from Sorted Array
Two Sum
Best Time to Buy and Sell Stock
Remove All Adjacent Duplicates
Max Consecutive Ones III
Word Ladder
First Missing Positive
Longest Increasing Path
Coin Change
Kadane-type problems
interval problems
DP problems

These are directly reported across recent interviews.

Your highest-priority DSA topics

1. Sliding Window

Minimum Size Subarray Sum
Longest Substring Without Repeating Characters
Max Consecutive Ones III
permutation in substring

2. Binary Search

Search in Rotated Sorted Array
binary search on answer
first/last occurrence

3. Arrays / HashMap

Two Sum
Kadane
frequency problems
prefix sum

4. Intervals

Merge Intervals
Non-overlapping Intervals
Meeting Rooms

5. Linked List

reverse
cycle
odd-even
add one to number

6. Trees / Graph

Number of Islands
BFS/DFS
Word Ladder
tree distance/burning tree

7. DP

Coin Change
House Robber
Longest Increasing Path
basic 1D/2D DP
2. LLD is probably your biggest scoring opportunity

This is where I would spend a lot of your preparation time.

Recent Kotak SDE-2 Bar Raisers repeatedly ask things like:

Very common designs
E-commerce system
Online marketplace
Instagram
Food delivery
Ride sharing
Parking lot
Parcel delivery
Order management
Flight booking
Notification system

For example, a March 2026 Bar Raiser asked for a Parcel Delivery System, including DB schema, classes, APIs, service design and design patterns.

Another recent Bar Raiser asked for Instagram, including schema, APIs, Observer pattern and scalability problems caused by celebrity posts.

Another asked for an e-commerce platform with warehouse inventory, including requirements, DB schema and APIs.

You need to be able to produce this structure automatically:
Requirements
      ↓
Entities
      ↓
Relationships
      ↓
DB Schema
      ↓
APIs
      ↓
Service Classes
      ↓
Design Patterns
      ↓
Concurrency
      ↓
Scalability
      ↓
Failure Handling

For example, if they say:

"Design a food delivery system."

You should immediately start discussing:

User
Restaurant
Menu
Order
OrderItem
Payment
DeliveryPartner
Delivery
Address

Then:

POST /orders
GET /orders/{id}
POST /restaurants
GET /restaurants/{id}/menu
POST /delivery/assign

Then DB.

Then class structure.

Then:

Strategy
Factory
Observer
State

Then concurrency.

Then scalability.

3. Distributed Systems — THIS is extremely important

This is the part I would not underestimate.

The latest March 2026 experience specifically asked:

Distributed lock

Where does a distributed lock work and when should you avoid it?

You need to know:

Redis distributed lock
DB lock
optimistic locking
pessimistic locking
lease/TTL
lock expiry
split brain
idempotency
Leader election

Know:

Why leader election?
        ↓
Only one node performs a particular responsibility
        ↓
Leader fails
        ↓
Another node becomes leader

And understand the practical role of systems such as ZooKeeper/etcd/consensus-based coordination.

Outbox Pattern

This is high priority.

Know exactly why this is needed:

DB transaction
      +
Kafka publish

The problem:

DB commit succeeds
Kafka publish fails
       ↓
inconsistent state

Outbox:

DB transaction
   ↓
business data + outbox event
   ↓
commit
   ↓
Outbox publisher
   ↓
Kafka

This was explicitly asked in the March 2026 Bar Raiser.

4. Backpressure

Definitely prepare this.

Kotak has asked it multiple times.

You should be able to explain:

What is backpressure?

Example:

Producer
   ↓
Kafka
   ↓
Consumer

If producer:

10,000 msg/sec

but consumer:

2,000 msg/sec

then backlog grows.

You should discuss:

bounded queues
rate limiting
batching
consumer scaling
Kafka partitions
consumer groups
lag monitoring
load shedding
retries
circuit breakers

A March 2026 interview explicitly asked what backpressure is and how to handle it end-to-end.

5. CAP + consistency

Prepare these extremely well.

Likely questions:

CAP theorem

Be able to explain:

Consistency
Availability
Partition Tolerance

and why a distributed system cannot simultaneously guarantee all three during a partition.

Eventual consistency

You may get:

"Explain eventual consistency to a non-technical stakeholder."

This exact type of question has been reported.

Also know
strong consistency
eventual consistency
read-after-write consistency
quorum
leader/replica
2PC
replication
conflict resolution

One recent Bengaluru report specifically mentions quorum, 2PC and leader-replica architectures.

6. Kafka — VERY important for you

Because your background is heavily Kafka/Spring Boot, I expect they may go much deeper with you than with someone who doesn't list Kafka.

Prepare:

Kafka fundamentals
topic
partition
offset
producer
consumer
consumer group
partition assignment
rebalancing
replication factor
ISR
leader/follower
Delivery semantics

Know:

At most once
At least once
Exactly once

And explain why exactly-once end-to-end is difficult.

Failure scenarios

They could ask:

Consumer processes message but crashes before committing offset. What happens?

Or:

How do you prevent duplicate processing?

Answer should include idempotent consumers.

Scaling

Kafka consumer is lagging. What do you do?

Talk about:

Consumer lag
   ↓
Partitions
   ↓
Consumer instances
   ↓
Processing time
   ↓
Batch size
   ↓
Concurrency
7. Core Java

For an SDE-2 backend interview, prepare these.

Must know

Collections

HashMap internals
ConcurrentHashMap
ArrayList vs LinkedList
HashSet
TreeMap

Concurrency

synchronized
volatile
AtomicInteger
locks
ReentrantLock
ReadWriteLock
ExecutorService
CompletableFuture
thread pool
deadlock
race condition

A recent Kotak interview explicitly included threads and locks.

JVM

Know at least:

Heap
Stack
Young generation
Old generation
GC
Minor GC
Major/Full GC
8. Spring Boot

Since you're a Java/Spring backend engineer, expect project-specific probing.

Be ready for:

Spring
Dependency Injection
IoC
Bean lifecycle
@Component vs @Service vs @Repository
@Configuration
@Bean
@Autowired
scopes
proxy
AOP
Spring Boot
auto configuration
starters
configuration properties
profiles
actuator
exception handling
filters/interceptors
transactions
Transactions

Know:

@Transactional

deeply.

Especially:

propagation
isolation
rollback
proxy limitation
nested transactions
DB transaction vs Kafka transaction
9. Database

This is another recurring area.

Prepare:

SQL
joins
indexes
composite indexes
query optimization
execution plan
normalization
denormalization
Transactions
ACID
Isolation levels
Dirty read
Non-repeatable read
Phantom read
Locking

Very important:

Optimistic locking
vs
Pessimistic locking

Also:

How do you prevent two users from purchasing the last item?

Excellent SDE-2 question.

Your 7-day preparation plan

You have 7 days, so don't try to learn everything.

Day 1 — DSA + Java
DSA

Do these:

Two Sum
Best Time to Buy/Sell Stock
Minimum Size Subarray Sum
Max Consecutive Ones III
Search in Rotated Sorted Array
Merge Intervals
Java

Revise:

HashMap
ConcurrentHashMap
equals/hashCode
immutable objects
synchronized
volatile
ExecutorService
CompletableFuture
Day 2 — LLD

Practice:

Design 1

Parking Lot

Design 2

E-commerce

Design 3

Food Delivery

For each one:

Requirements
Entities
Class diagram
DB schema
APIs
Patterns
Concurrency
Extensibility

Don't just watch solutions.

Draw it yourself.

Day 3 — Distributed Systems

Master these:

MUST
CAP
consistency
replication
partitioning
quorum
distributed lock
leader election
idempotency
retry
timeout
circuit breaker
backpressure
outbox
saga

Spend extra time on:

Outbox + Kafka + retries + idempotency

because that combination fits your background particularly well.

Day 4 — Kafka + Microservices

Build your mental model around:

Spring Boot
     ↓
Kafka Producer
     ↓
Kafka Topic
     ↓
Partitions
     ↓
Consumer Group
     ↓
Consumer
     ↓
DB

Be able to answer:

How Kafka works internally
Why partitions?
Why consumer groups?
What happens during rebalance?
How do you handle duplicate events?
How do you handle failed consumers?
How do you handle poison messages?
How do you handle Kafka lag?
How do you guarantee ordering?
At-least-once vs exactly-once
Retry architecture
DLQ architecture
Day 5 — HLD

Practice these three:

1. Notification System
2. Online Marketplace
3. Fund Transfer System

The fund-transfer design is particularly worth doing for Kotak because it has appeared in reported SDE-2 interviews.

For every HLD, practice:

Requirements
↓
Capacity estimation
↓
API
↓
DB
↓
Architecture
↓
Caching
↓
Messaging
↓
Scaling
↓
Consistency
↓
Failure
↓
Monitoring
Day 6 — YOUR PROJECT

This is extremely important.

I would spend 4–5 hours only on your Cardinal Health project.

They can take one line from your resume and go 10 levels deep.

For your Kafka/Spring Boot experience, prepare questions such as:

Why Kafka instead of REST?

Why this partition key?

How did you decide partition count?

What happened if Kafka went down?

How did you handle duplicate events?

How did you handle consumer lag?

How did you monitor lag?

How did you achieve 99.99% availability?

Why did you choose this database?

What happens if DB succeeds but Kafka fails?

How did you scale from X to Y TPS?

What was the biggest production incident?

What would you redesign today?

Why Spring Boot?

How did you deploy it?

How did Kubernetes affect your architecture?

These are far more important for you than memorizing another 50 LeetCode questions.

Day 7 — Full Mock Bar Raiser

Do a 90-minute simulation.

0–5 min

Introduction + project.

5–30 min

DSA.

Pick:

Minimum Size Subarray Sum

Then follow-up:

What if numbers can be negative?

This tests whether you understand why sliding window works.

30–60 min

LLD:

Design an e-commerce system.

You must cover:

Product
Inventory
Cart
Order
Payment
User

DB + APIs + classes + patterns.

60–80 min

Distributed systems:

Ask yourself:

DB commit succeeded but Kafka publish failed. What happens?

How would you fix it?

What is outbox?

What if Kafka consumer processes the event twice?

What if consumer is slower than producer?

How would you handle that?

80–90 min

Project deep dive.

🔥 My priority ranking for YOUR 7 days

Considering your 4 YOE Java/Spring Boot + Kafka + GCP backend profile, I'd rank preparation:

                    Priority

LLD                  ██████████  10/10
Distributed Systems  ██████████  10/10
Project Deep Dive    ██████████  10/10
Kafka/Microservices  █████████   9/10
HLD                  █████████   9/10
DSA                  ████████    8/10
Java Concurrency     ████████    8/10
SQL/DB               ███████     7/10
Spring Boot          ███████     7/10
JVM                  █████       5/10

Don't make the mistake of spending all 7 days on DSA.

The recent evidence strongly suggests Kotak's Bar Raiser evaluates problem solving + LLD + distributed systems/HLD together.

The 20 questions I'd bet on

If I had to make a Kotak SDE-2 Bar Raiser hit list for your interview:

Search in Rotated Sorted Array
Minimum Size Subarray Sum
Sliding-window variation
Two Sum / HashMap variation
Kadane variation
Design E-commerce
Design Food Delivery
Design Parking Lot
Design Instagram / social media
Design Order Management
Design Notification System
What is CAP theorem?
Strong vs eventual consistency?
What is backpressure?
What is the Outbox Pattern?
What is a distributed lock?
How does leader election work?
Optimistic vs pessimistic locking?
Kafka consumer groups / partitions / offsets / rebalancing
Deep dive into your current project

That list is much more representative of the recent reports than a generic "SDE-2 interview question list."

One important observation

The March–April 2026 reports are especially relevant to your upcoming interview. They show a repeated structure of 90-minute Bar Raiser → coding → LLD → distributed systems/HLD, and one recent report from a 3.8 YOE Java/Spring Boot backend engineer is remarkably close to your profile.

So for the next 7 days, I'd optimize for being able to have a deep technical conversation, not merely solving LeetCode quickly.

If you want, I can next turn this into a 
7-day Kotak SDE-2 crash course with ~100 exact questions (DSA + LLD + HLD + Java + Spring + Kafka + DB), day-by-day, with the questions ordered by probability and difficulty.

Give me a 7-day Kotak SDE-2 crash course with ~100 exact questions (DSA + LLD + HLD + Java + Spring + Kafka + DB), day-by-day, with the questions ordered by probability and difficulty

Absolutely. I re-checked recent Kotak SDE-2 reports, including March 2026 reports and 2025 reports, rather than giving you a generic SDE-2 syllabus.

The pattern is unusually consistent: 90-minute Bar Raiser = DSA + LLD + distributed systems/HLD, with project/CS fundamentals often mixed in. Recent reports include Roman to Integer, Longest Common Prefix, Minimum Size Subarray Sum, Search in Rotated Sorted Array, Kadane, Word Ladder, e-commerce, Instagram, food delivery, ride-sharing, parking lot, parcel delivery, distributed locks, leader election, outbox, backpressure and consistency.

Below is the 7-day crash course I'd actually use if your interview were 7 days away.

Kotak SDE-2 — 7-Day Crash Course
Priority legend
🔴 P0 — Must do
🟠 P1 — Very important
🟡 P2 — If time permits
⭐ = particularly representative of recent Kotak reports

You have ~100 questions, but do not treat them as 100 equal questions. The first ~50 are the real priority.

DAY 1 — DSA + Core Java
Goal

Get comfortable solving the type of coding questions appearing in the Bar Raiser.

Recent reports show easy/medium problems such as Roman to Integer, Longest Common Prefix, Minimum Size Subarray Sum, Search in Rotated Sorted Array, Two Sum, Kadane and Remove Duplicates.

DSA — 10 questions
#	Question	Priority	Difficulty
1	⭐ Roman to Integer	🔴	Easy
2	⭐ Longest Common Prefix	🔴	Easy
3	⭐ Two Sum	🔴	Easy
4	⭐ Best Time to Buy and Sell Stock	🔴	Easy
5	⭐ Minimum Size Subarray Sum	🔴	Medium
6	⭐ Search in Rotated Sorted Array	🔴	Medium
7	⭐ Remove Duplicates from Sorted Array II	🔴	Medium
8	⭐ Maximum Subarray / Kadane	🔴	Medium
9	Max Consecutive Ones III	🟠	Medium
10	Remove All Adjacent Duplicates in String	🟠	Easy

Important: Don't merely solve them.

For every problem say aloud:

Brute force
↓
Observation
↓
Optimal approach
↓
Why it works
↓
Complexity
↓
Edge cases

That communication matters in a Bar Raiser.

Core Java — 5 questions
11. 🔴 How does HashMap work internally?

Be able to explain:

hashCode()
    ↓
bucket
    ↓
collision
    ↓
equals()
    ↓
Node / tree
12. 🔴 HashMap vs ConcurrentHashMap?
13. 🔴 synchronized vs volatile vs Atomic classes?
14. 🔴 What is a race condition? Give a real example.
15. 🟠 How does ExecutorService / ThreadPoolExecutor work?

Know:

corePoolSize
maximumPoolSize
queue
keepAliveTime
rejection policy
DAY 2 — LLD Day

This is one of your highest-value days.

Recent Kotak reports repeatedly contain LLD: e-commerce, Instagram, food delivery, ride-sharing, parking lot, parcel delivery and marketplace systems.

LLD — 10 questions
16. 🔴 ⭐ Design an E-commerce system

Must cover:

User
Product
Cart
Inventory
Order
Payment
Shipment

Follow-ups:

inventory race condition
payment failure
order state
database schema
APIs
scaling
17. 🔴 ⭐ Design a Food Delivery system

Entities:

Customer
Restaurant
Menu
Order
Payment
DeliveryPartner
Delivery

Follow-ups:

assign delivery partner
order state machine
restaurant unavailable
payment failure

A March 2026 report specifically used food delivery with schema + APIs + class design.

18. 🔴 ⭐ Design Instagram

Know:

User
Follow
Post
Like
Comment
Feed

Follow-up:

What happens when a celebrity with 50M followers posts?

This tests fan-out and scalability.

19. 🔴 ⭐ Design Ride Sharing
Rider
Driver
Trip
Location
Payment

Follow-ups:

nearest driver
concurrent booking
driver state
cancellation

This appeared directly in a recent Bar Raiser.

20. 🔴 ⭐ Design Parking Lot

Know:

multiple floors
vehicle types
parking spots
ticket
pricing
availability

This has also appeared directly in a Kotak SDE-2 Bar Raiser.

21. 🟠 Design Parcel Delivery System

This is particularly important because a March 2026 Bar Raiser explicitly asked for:

DB schema
class structure
APIs
service design
Strategy
Observer

22. 🟠 Design Flight Ticket Booking

Cover:

Flight
Aircraft
Seat
Passenger
Booking
Payment

Important follow-up:

Two users attempt to book the last seat. What happens?

A reported Kotak offer experience included HLD + LLD for flight booking.

23. 🟠 Design Notification Service

Channels:

Email
SMS
Push

Use:

Strategy Pattern

24. 🟠 Design Job Scheduler

Think:

Job
Schedule
Worker
Queue
Retry
Dead Letter
25. 🟡 Design Logger

Cover:

Logger
Appender
Formatter
LogLevel

Patterns:

Chain of Responsibility
Factory
Singleton — and discuss why you'd be careful with Singleton.
DAY 3 — Distributed Systems

This is critical.

The March 2026 Bar Raiser directly asked about distributed locks, leader election, outbox, backpressure and consistency during network partitions.

15 questions
26. 🔴 ⭐ What is CAP theorem?

You should explain it without memorizing a definition.

27. 🔴 ⭐ Strong consistency vs eventual consistency?
28. 🔴 ⭐ What is a distributed lock?

Follow-ups:

Redis lock
TTL
lock expiration
process crashes
why distributed locks can be dangerous
29. 🔴 ⭐ When should you NOT use a distributed lock?

This was explicitly asked.

30. 🔴 ⭐ How does leader election work?

Why do we need it?

What happens when leader dies?

31. 🔴 ⭐ Explain the Outbox Pattern.

You absolutely need this.

Business DB
     +
Outbox table
     ↓
same transaction
     ↓
Outbox publisher
     ↓
Kafka
32. 🔴 ⭐ What is backpressure?

Example:

Producer = 100K events/sec
Consumer = 20K events/sec

What happens?

How do you fix it?

This has been explicitly reported.

33. 🔴 ⭐ How do you maintain consistency during a network partition?
34. 🔴 What is idempotency?

Give a payment example.

35. 🔴 How do you design an idempotent API?
36. 🟠 What is a circuit breaker?

Explain:

Closed
 ↓
Open
 ↓
Half Open
37. 🟠 Retry strategy?

Why is:

retry immediately

often dangerous?

Know:

exponential backoff
jitter
max retries
38. 🟠 What is Saga Pattern?

When would you use it?

39. 🟠 What is 2PC?

Why is it expensive/problematic?

40. 🟠 What is quorum?

Understand:

N = replicas
W = write quorum
R = read quorum
DAY 4 — Kafka + Microservices

For your profile, make this a major day.

Don't just know Kafka from the resume. Be able to defend architectural decisions.

Kafka — 12 questions
41. 🔴 ⭐ Explain Kafka architecture.
Producer
   ↓
Topic
   ↓
Partition
   ↓
Broker
   ↓
Consumer Group
42. 🔴 ⭐ Why do Kafka topics have partitions?
43. 🔴 ⭐ What is a consumer group?
44. 🔴 ⭐ What happens when a consumer dies?
45. 🔴 ⭐ What causes consumer rebalance?
46. 🔴 ⭐ What is consumer lag?

How do you troubleshoot it?

47. 🔴 ⭐ How do you guarantee ordering in Kafka?
48. 🔴 ⭐ At-most-once vs at-least-once vs exactly-once?
49. 🔴 ⭐ How do you prevent duplicate event processing?

Answer:

Idempotency, not magical "exactly once."

50. 🔴 ⭐ DB update succeeds but Kafka publish fails. What do you do?

Answer:

Outbox Pattern.

51. 🟠 Consumer is processing slower than producer. What do you do?

Discuss:

Partitions
Consumer instances
Batching
Concurrency
Processing optimization
Backpressure
52. 🟠 How do you handle poison messages?

Discuss:

retry
↓
retry limit
↓
DLQ
↓
manual/replay
Microservices — 5 questions
53. 🔴 REST vs Kafka?

When would you choose each?

54. 🔴 How do microservices communicate?
55. 🟠 How do you handle service discovery?
56. 🟠 How do you trace a request across 10 microservices?

Think:

Correlation ID + distributed tracing.

57. 🟠 How do you handle cascading failures?

Think:

Timeout
Retry
Circuit breaker
Bulkhead
Rate limit
DAY 5 — HLD + Database
HLD — 8 questions
58. 🔴 ⭐ Design an Online Marketplace

This is highly representative because Kotak has explicitly used marketplace/e-commerce designs.

59. 🔴 ⭐ Design a Twitter/X-like system

Recent reports mention Twitter-style LLD/HLD.

Know:

Post
Follow
Feed
Like
Comment
60. 🔴 ⭐ Design a Trading System

Recent Kotak interviews included an online trading system with real-time stock-price updates and order management.

61. 🔴 Design Notification System
62. 🟠 Design File Processing Pipeline

Example:

Upload
 ↓
Event
 ↓
Kafka
 ↓
Workers
 ↓
Processing
 ↓
Storage

This is relevant because a reported SDE-II interview included event-based file processing.

63. 🟠 Design URL Shortener
64. 🟠 Design Rate Limiter
65. 🟠 Design Distributed Cache
Database — 10 questions
66. 🔴 ⭐ Explain ACID.
67. 🔴 ⭐ Explain database isolation levels.

Know:

Read Uncommitted
Read Committed
Repeatable Read
Serializable

And:

Dirty Read
Non-repeatable Read
Phantom Read
68. 🔴 ⭐ Optimistic vs pessimistic locking?
69. 🔴 ⭐ How does an index work?
70. 🔴 Composite index — how does column order matter?
71. 🔴 When can an index hurt performance?
72. 🟠 How do you optimize a slow SQL query?
73. 🟠 Normalization vs denormalization?
74. 🟠 SQL vs NoSQL — when would you choose each?
75. 🟠 What is database sharding?
DAY 6 — Spring + Java + Project Deep Dive

This day is extremely important for you personally.

Spring Boot — 10 questions
76. 🔴 ⭐ How does Spring Dependency Injection work?
77. 🔴 ⭐ What is Spring Bean lifecycle?
78. 🔴 ⭐ @Component vs @Service vs @Repository?
79. 🔴 ⭐ How does @Transactional work internally?

This one can expose shallow knowledge very quickly.

Know:

Spring Proxy
 ↓
Transaction begins
 ↓
Method executes
 ↓
Commit / rollback
80. 🔴 ⭐ Why does @Transactional sometimes not work on self-invocation?
81. 🟠 Transaction propagation types?

Especially:

REQUIRED
REQUIRES_NEW
NESTED
82. 🟠 Transaction isolation?

Connect this with your DB knowledge.

83. 🟠 Spring AOP — how does it work?
84. 🟠 Filter vs Interceptor vs AOP?
85. 🟠 How does Spring Boot auto-configuration work?
Java concurrency — 5 questions
86. 🔴 synchronized vs Lock?
87. 🔴 What is deadlock? How do you prevent it?
88. 🔴 volatile — what does it guarantee and what doesn't it guarantee?
89. 🟠 CompletableFuture — how does it work?
90. 🟠 How would you design a thread-safe cache?
YOUR PROJECT — 10 questions

This is where I want you to spend at least 2–3 hours.

Recent Kotak reports explicitly include deep project discussion, sometimes as a major part of the interview.

Given your Java/Spring/Kafka background, prepare these:

91. 🔴 ⭐ Explain your current architecture end-to-end.

Draw:

Client
 ↓
API
 ↓
Spring Boot
 ↓
DB
 ↓
Kafka
 ↓
Consumer
 ↓
Downstream
92. 🔴 ⭐ Why did you choose Kafka instead of synchronous REST?
93. 🔴 ⭐ Why did you choose your Kafka partition key?
94. 🔴 ⭐ What happens if Kafka goes down?
95. 🔴 ⭐ What happens if your consumer crashes after processing but before committing offset?
96. 🔴 ⭐ How do you prevent duplicate processing?
97. 🔴 ⭐ How did you monitor Kafka consumer lag?
98. 🔴 ⭐ What was your hardest production issue?

Use:

Problem
↓
Investigation
↓
Root cause
↓
Fix
↓
Prevention
99. 🔴 ⭐ What would you redesign in your current system?

This is a very strong SDE-2 question.

Don't say:

"Nothing."

Have a genuine improvement ready.

100. 🔴 ⭐ Explain one architecture decision where you considered two alternatives and rejected one.

Example:

Kafka vs REST
Redis vs DB
GCP vs ...
SQL vs NoSQL
Sync vs Async

This tests engineering judgment, not memorization.

DAY 7 — Full Mock + Revision

Do NOT learn new topics on Day 7.

Your job is to simulate the actual interview.

Recent Bar Raiser reports show approximately 1.5-hour sessions split across problem solving, design and distributed systems.

Mock #1 — 90 minutes
0–20 min — DSA

Pick:

Search in Rotated Sorted Array

Follow-up:

Can you do it in O(log n)?

Then:

What changes with duplicates?

20–50 min — LLD

Design Food Delivery

You must produce:

Requirements
↓
Entities
↓
Classes
↓
DB schema
↓
APIs
↓
Patterns
↓
Concurrency
50–70 min — Distributed Systems

Answer without notes:

Distributed lock?
Leader election?
Outbox?
Backpressure?
Eventual consistency?
Network partition?

These are directly aligned with recent Bar Raiser questioning.

70–90 min — Project

Explain your current project.

Then attack it:

Why Kafka?

Why this DB?

What if Kafka fails?

What if DB fails?

What if consumer processes twice?

How did you scale?

What was the bottleneck?

What would you change?

Your final priority list

If you're short on time, don't try to complete all 100.

Do these first:

🔥 Top 30
1.  Minimum Size Subarray Sum
2.  Search in Rotated Sorted Array
3.  Two Sum
4.  Kadane
5.  Remove Duplicates Sorted Array II

6.  E-commerce LLD
7.  Food Delivery LLD
8.  Instagram LLD
9.  Ride Sharing LLD
10. Parking Lot LLD

11. CAP
12. Strong vs Eventual Consistency
13. Distributed Lock
14. Leader Election
15. Outbox Pattern
16. Backpressure
17. Network Partition
18. Idempotency
19. Circuit Breaker
20. Saga

21. Kafka architecture
22. Kafka partitions
23. Consumer groups
24. Consumer rebalance
25. Consumer lag
26. Kafka ordering
27. Kafka delivery semantics
28. Duplicate processing
29. DB + Kafka failure
30. Poison messages / DLQ

Then:

31. ACID
32. Isolation levels
33. Optimistic/Pessimistic locking
34. Indexes
35. SQL optimization

36. @Transactional
37. Spring proxy
38. Spring Bean lifecycle
39. AOP
40. Filter vs Interceptor

41. HashMap
42. ConcurrentHashMap
43. synchronized
44. volatile
45. ExecutorService

46. Marketplace HLD
47. Twitter HLD
48. Trading System HLD
49. Notification HLD
50. File Processing HLD

51–60. Your project deep dive
One important change I'd make for you

Because you already have Java + Spring Boot + Kafka + Kubernetes/GCP experience, I would not spend 7 days trying to become an expert in every technology.

Your likely differentiation is:

"I know how to build production distributed systems."

So your interview story should become:

             SDE-2 Candidate

                  │
        ┌─────────┴─────────┐
        ↓                   ↓
   Problem Solving      Engineering
                           Judgment
        │                   │
       DSA          ┌───────┼────────┐
                    ↓       ↓        ↓
                  Java    Kafka     DB
                    │       │        │
                    └───────┼────────┘
                            ↓
                    Distributed Systems
                            ↓
                    System Design
                            ↓
                       Production

That's much closer to what the recent Kotak interviews are testing than simply grinding another 100 LeetCode problems. Recent reports repeatedly describe DSA + LLD + distributed systems/HLD, and one February 2026 report specifically says the Bar Raiser covered problem solving, LLD and HLD in a 1.5-hour session.

For the next 7 days, your mantra should be: solve → design → defend → handle failure → explain trade-off.

Yesterday 4:59 PM
Give a scenario of teg api interview like the lld part focussing on the api and the server  on any of the imp question.
Maintain the question they ask and expectation from the interviewer.
The interview following the lld or pattern the api design on the api given or the imp and the db design for the rounds like 45 mins.

First focus on the api then db design.

Yes. For a 45-minute Kotak SDE-2 LLD round, I would practice it differently from a generic "design a parking lot" LLD.

The interviewer can give you a business scenario and expect you to move through:

Requirements → API contract → server/service design → DB schema → concurrency/transactions → design patterns

Since you want to first focus on API, then DB, let's do a realistic mock.

Mock Interview #1 — E-commerce Order API
Interviewer

Design an Order Management API for an e-commerce application.

A customer can create an order containing multiple products, view an order, cancel an order, and check its status.

Assume that inventory already exists in the system.

You have 45 minutes. Start with the APIs and server-side design. We will discuss the database after that.

What the interviewer is actually evaluating

They aren't primarily checking whether you know HTTP syntax.

They're checking whether you can turn an ambiguous requirement into a production-quality API.

They want to see whether you naturally think about:

Resource modeling
HTTP semantics
Request/response contracts
Validation
Error handling
Authentication/authorization
Idempotency
Concurrency
Transactions
Pagination/filtering
API versioning
Server/service boundaries
DB consistency

For SDE-2, don't jump directly into Java classes.

Start with the API contract.

0–5 minutes — Clarify requirements
Interviewer

What questions do you have before designing the APIs?

Strong candidate

I'd ask:

Can one order contain multiple products?
Does the client specify the price, or does the server calculate the price?
Should inventory be reserved when the order is created?
Can an order be cancelled after payment?
Do we support partial cancellation?
Do we need pagination for order history?
Is payment handled by this service or an external payment service?
Should order creation be synchronous?
Do we require idempotency for order creation?

That's already a strong SDE-2 signal.

5–15 minutes — API Design

Now say:

I'll model Order as the primary resource.

API 1 — Create Order
POST /api/v1/orders

Request:

{
  "items": [
    {
      "productId": "P101",
      "quantity": 2
    },
    {
      "productId": "P205",
      "quantity": 1
    }
  ],
  "shippingAddressId": "ADDR123",
  "paymentMethodId": "PM123"
}
Important question

The interviewer may ask:

Why isn't price present in the request?

Answer:

Price is server-owned data. The client should not be trusted to determine the price. The server retrieves the current price and creates the order using that price.

Good.

Idempotency

Now the interviewer may deliberately ask:

What happens if the client sends the request twice?

This is very important.

Your answer:

POST /api/v1/orders
Idempotency-Key: 8f72...

The server stores:

idempotency_key
user_id
request_hash
response
status

If the same request arrives again:

Same key
   ↓
Already processed?
   ↓
YES
   ↓
Return previous response
Interviewer follow-up

What if the same idempotency key is used with a different request?

Answer:

I would reject it with 409 Conflict because an idempotency key should represent one logical operation.

That's an SDE-2 level answer.

API 2 — Get Order
GET /api/v1/orders/{orderId}

Response:

{
  "orderId": "ORD123",
  "status": "CONFIRMED",
  "items": [
    {
      "productId": "P101",
      "quantity": 2,
      "unitPrice": 500
    }
  ],
  "totalAmount": 1000,
  "createdAt": "2026-09-08T10:30:00Z"
}
Interviewer

Why return unitPrice from the order rather than retrieving the current product price?

Excellent opportunity.

Answer:

Order price is historical data. Product price can change after the order is created, so the order should maintain its own price snapshot.

This is both API and DB thinking.

API 3 — Order History
GET /api/v1/orders?status=CONFIRMED&page=0&size=20

Don't do:

GET /api/v1/getAllOrders
Interviewer

Why pagination?

Answer:

A customer could have thousands of orders. Returning everything causes unnecessary DB, network and memory consumption.

API 4 — Cancel Order
POST /api/v1/orders/{orderId}/cancel

Request:

{
  "reason": "CUSTOMER_REQUEST"
}
Interviewer

Why POST instead of DELETE?

This is a very good interview question.

Answer:

DELETE is appropriate when we're deleting a resource. Here cancellation is a business operation that changes the state of an order and may trigger inventory release, refund and events. Therefore I'd model cancellation as a command/action.

Very good.

API 5 — Get Order Status

You could use:

GET /api/v1/orders/{orderId}

and return status within the order.

You don't necessarily need:

GET /api/v1/orders/{id}/status

unless there's a strong reason.

Interviewer

Why not create a separate status API?

Answer:

If status is already part of the order representation, a separate endpoint adds unnecessary API surface. I'd introduce it only if status has different access patterns or performance requirements.

Again: engineering judgment.

API Error Design

The interviewer will often push here.

What happens if product doesn't exist?
404 NOT_FOUND
Invalid quantity?
400 BAD_REQUEST
User isn't allowed to access order?
403 FORBIDDEN
Duplicate idempotency key?

Potentially:

409 CONFLICT
Order cannot be cancelled because it has already shipped?
409 CONFLICT

You should have a consistent error structure:

{
  "code": "ORDER_ALREADY_SHIPPED",
  "message": "Order cannot be cancelled after shipment.",
  "traceId": "abc123"
}
15–25 minutes — Server Design

Now the interviewer says:

Good. Show me how you'd implement the server.

Don't immediately dump 20 classes.

Start with:

Controller
    ↓
OrderService
    ↓
Repositories
    ↓
Database

And external systems:

                    ┌── Inventory Service
                    │
Client
  ↓                 ├── Payment Service
Controller
  ↓                 │
OrderService ───────┼── Kafka
  ↓                 │
Repository          └── ...
  ↓
Database
Controller
@RestController
@RequestMapping("/api/v1/orders")
public class OrderController {

    @PostMapping
    public ResponseEntity<OrderResponse> createOrder(
            @RequestHeader("Idempotency-Key") String key,
            @Valid @RequestBody CreateOrderRequest request) {
        
        return ResponseEntity.ok(
            orderService.createOrder(key, request)
        );
    }

    @GetMapping("/{orderId}")
    public OrderResponse getOrder(
            @PathVariable String orderId) {
        
        return orderService.getOrder(orderId);
    }

    @PostMapping("/{orderId}/cancel")
    public OrderResponse cancelOrder(
            @PathVariable String orderId,
            @RequestBody CancelOrderRequest request) {
        
        return orderService.cancelOrder(orderId, request);
    }
}

You don't need to write every class in the interview.

Show the important boundaries.

Service Layer
OrderService

createOrder()
getOrder()
cancelOrder()
getOrders()

Then:

OrderService
    |
    +-- InventoryClient
    |
    +-- PaymentClient
    |
    +-- OrderRepository
    |
    +-- OutboxRepository
Interviewer

Why are you using interfaces for these dependencies?

Answer:

To keep the domain/service logic independent of infrastructure implementations, improve testability and allow implementations to change without modifying the business logic.

25–35 minutes — Now the interviewer attacks the API

This is where a Bar Raiser can differentiate candidates.

Question 1

Two requests arrive simultaneously trying to buy the last item. What happens?

Don't say:

"I'll use synchronized."

That only protects one JVM.

Instead:

Request A ──┐
            ↓
        Inventory
            ↑
Request B ──┘

You need distributed concurrency control.

Possible approaches:

Optimistic locking

Inventory:

product_id
available_quantity
version

Update:

UPDATE inventory
SET available_quantity = available_quantity - 1,
    version = version + 1
WHERE product_id = ?
  AND available_quantity >= 1
  AND version = ?;

If affected rows = 0:

Concurrency conflict

That's a strong answer.

Question 2

What happens if payment succeeds but order creation fails?

Now you're in distributed transaction territory.

Don't say:

"Use @Transactional."

@Transactional cannot magically make an external payment service transactional with your DB.

Discuss:

Order Service
      ↓
Payment Service

Possible approach:

Saga / compensation

If payment succeeds but order cannot proceed:

Payment SUCCESS
       ↓
Order FAILURE
       ↓
Refund payment
Question 3

What happens if your API creates the order in DB but Kafka publish fails?

Excellent.

Say:

I wouldn't perform DB commit and Kafka publish as two unrelated operations.

Use:

Order DB transaction
        ↓
Order + Outbox Event
        ↓
COMMIT
        ↓
Outbox Publisher
        ↓
Kafka

Now your API has become a production-quality design.

Question 4

Client receives 500. Did the order actually get created?

You should say:

The client cannot assume that a 500 means the operation was not completed. The request may have succeeded on the server and failed while returning the response. That's exactly why idempotency is important for order creation.

Excellent.

Question 5

Should POST /orders return 200 or 201?

Best answer:

201 Created

when the order has been successfully created.

You can return:

Location: /api/v1/orders/ORD123
35–45 minutes — DB Design

Now interviewer says:

Okay. Design the database.

Start with the core entities.

users
products
orders
order_items
inventory
payments
addresses
outbox_events
idempotency_keys
orders
orders
--------------------
id PK
user_id FK
status
total_amount
shipping_address_id
created_at
updated_at
version
order_items
order_items
--------------------
id PK
order_id FK
product_id FK
quantity
unit_price

Notice:

unit_price is stored here.

Because product price may change later.

inventory
inventory
--------------------
product_id PK
available_quantity
reserved_quantity
version
updated_at

version supports optimistic concurrency.

payments
payments
--------------------
id PK
order_id FK
payment_reference
amount
status
created_at
updated_at
idempotency_keys
idempotency_keys
--------------------
key PK
user_id
request_hash
response
status
created_at
expires_at

Critical index:

UNIQUE(key, user_id)
outbox_events
outbox_events
--------------------
id PK
aggregate_id
event_type
payload
status
created_at
processed_at
Interviewer: "What indexes?"

You should proactively answer.

Orders
INDEX(user_id, created_at)

because:

GET /orders?...

mostly queries a user's orders.

Order items
INDEX(order_id)
Payments
INDEX(order_id)
Outbox
INDEX(status, created_at)

for publisher polling.

Final 5-minute Bar-Raiser follow-up

The interviewer might now say:

Your order table has 500 million rows. What happens?

You discuss:

indexing
partitioning
archiving
read replicas
pagination
possibly sharding

Then:

Your order API gets 20K requests/sec.

Discuss:

Load Balancer
       ↓
Multiple Order Service instances
       ↓
Cache where appropriate
       ↓
DB read replicas
       ↓
DB partitioning/sharding if required

But don't immediately say "Redis + Kafka + Kubernetes" for everything.

Explain why each component is necessary.

What a strong 45-minute answer looks like

Your flow should be almost automatic:

0–5 min
Requirements
     ↓
5–15 min
API contracts
     ↓
15–25 min
Server architecture
     ↓
25–35 min
Concurrency + failures
     ↓
35–45 min
DB schema + indexes

And the interviewer should hear these concepts naturally:

REST semantics
Validation
HTTP status codes
Idempotency
Authentication
Authorization
Pagination
Versioning
Service layer
Repository
Transactions
Optimistic locking
DB constraints
Indexes
Outbox
Kafka
Saga
Failure handling
The 10 questions you should practice exactly this way

For your next 45-minute API → server → DB preparation, I'd do these in this order:

🔴 Level 1 — Must practice
Design E-commerce Order API
Design Food Delivery API
Design Payment/Transaction API
Design Flight Booking API
Design Ride Booking API
🟠 Level 2
Design Inventory Management API
Design Notification API
Design Parcel Delivery API
Design Wallet API
Design Ticket Booking API

For each, don't start with classes.

Start:

"Let me first clarify the requirements and define the resources."

Then:

API → request/response → errors → idempotency → server → concurrency → DB.

That is the exact practice style I recommend for your Kotak round. Recent Kotak reports specifically show LLD questions being pushed into DB schema, APIs, classes, design patterns and scalability, including e-commerce, parcel delivery and Instagram-style designs.





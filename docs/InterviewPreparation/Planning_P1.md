Round 1 / Bar-Raiser DSA

#	Question	Reported in
1	Minimum Size Subarray Sum — minimum-length subarray whose sum is ≥ target	Kotak SDE-2 Bangalore — Rejected
2	Minimum Size Subarray Sum	Kotak SDE-2 Sept-Nov 2025
3	Minimum Size Subarray Sum	Kotak SDE2 Bangalore offer
4	Best Time to Buy and Sell Stock	Kotak SDE-2 Hyderabad Backend
5	Two Sum	Kotak SDE-2 Hyderabad Backend
6	Search in Rotated Sorted Array	Kotak SDE-2 Bangalore Backend
7	Search in Rotated Sorted Array	Kotak SDE2 Bangalore 2025 Offer
8	Best Time to Buy and Sell Stock	Kotak SDE-1 Bengaluru 2024
9	Container With Most Water	Kotak SDE-1 Interview Experience
10	Kadane's Algorithm / Maximum Subarray-type problem	Kotak SDE-2 Backend 3 Rounds
11	Coin Change — DP	Kotak SDE-2 Hyderabad
12	Minimum-cost travel passes — 1-day, 1-week, 1-month passes; cover given travel days with minimum cost	Kotak SDE II Interview Exp.
13	House Robber-type DP	Kotak SDE-2 Sept-Nov 2025
14	Tree maximum independent set — each node has coins; choose non-adjacent nodes to maximize sum; DFS + DP	Kotak SDE-2 December 2025 Reject
15	Variation of Jump Game — brute force → DP	Kotak SDE-1 Backend
16	Climbing Stairs	Kotak SDE-I Experience
17	Merge Two Sorted Arrays in-place	Kotak SDE-I Experience
18	Merge Two Sorted Arrays — compare extra-space and in-place approaches	Recently Interviewed Kotak SDE-1
19	Two Sum — return indexes	Recently Interviewed Kotak SDE-1
20	Two Sum — return indexes	Kotak SDE-1 Bar Raiser
21	Smart Cache with Dependency-Aware Eviction — cache items have dependencies; invalidating one invalidates dependents; minimize recomputation cost	Kotak SDE-1 Interview Experience
B. Round 2 DSA
#	Question	Reported in
22	Longest Increasing Path in a Matrix — LC 329	Kotak SDE-2 Hyderabad Backend
23	Longest Increasing Path in a Matrix — LC 329	Kotak SDE-2 Backend 3 Rounds
24	Largest Rectangle in Histogram — LC 84	Kotak SDE2 Interview Process Selected
25	Wildcard Matching	Kotak SDE2 Bangalore Experience
26	Prefix Sum problem	Kotak SDE2 Bangalore Experience
27	Max Robber / House Robber DP	Kotak SDE2 Sept-Nov 2025
28	Minimum intervals to remove to make remaining intervals non-overlapping	Kotak SDE2 Sept-Nov 2025
29	IPO — LC 502	Kotak SDE-2 Bangalore Backend
30	Reorganize String — LC 767	Kotak SDE-2 Bangalore Backend
31	Segregate prime and non-prime numbers in a Linked List — variation of Odd-Even Linked List	Kotak SDE-2 Bangalore Reject
32	Array/string question — exact problem not disclosed	Kotak SDE-2 Bangalore Reject
33	JSON nesting count — count nested objects in a JSON string	Kotak SDE II Interview Exp.
34	Power + recursive digit sum — calculate 2^n, repeatedly sum digits to one digit; n ≤ 10^9	Kotak SDE II Interview Exp.
35	Find element in rotated array	Kotak SDE-2 Hyderabad
36	Find all palindromes for a string; optimized version	Kotak SDE-2 Hyderabad
37	H-Index — optimized O(n) approach	Kotak SDE-1 Bengaluru 2024
38	Candy — prefix/suffix approach	Kotak SDE-1 Bengaluru 2024
39	Group Anagrams — LC 49	Kotak SDE-1 Interview Experience
40	Permutations — LC 46	Kotak SDE-1 Interview Experience
41	Minimum counters required — scheduling problem; PriorityQueue solution	Kotak SDE-1 Interview Experience
42	Diameter of Binary Tree — O(N²) vs O(N) single traversal	Kotak SDE-1 Interview Experience
C. Additional DSA reported in these experiences
#	Question	Source
43	Merge Two Sorted Arrays	Kotak SDE-1 Bar Raiser
44	Two Sum	Kotak SDE-1 Bar Raiser
45	Container With Most Water	Kotak SDE-1 Interview Experience
46	Reverse a string without extra space	Kotak SDE-1 Interview Experience
47	Kth smallest number in an array — implemented using the LC Kth Largest formulation	Kotak SDE-1 Interview Experience
48	Find minimum counters / scheduling	Kotak SDE-1 Interview Experience
49	Asteroid Collision — stack	Kotak SDE-2 Bangalore Backend
50	Difference between maximum and minimum in every window of size k — Deque	Kotak SDE-2 Bangalore Backend
51	Array with duplicates at most twice — modify in-place and return new length	Kotak SDE2 Bangalore 2025 Offer
52	Burning Binary Tree — random node catches fire; 1 edge = 1 unit; find time to burn entire tree	Kotak SDE2 Bangalore 2025 Offer
## LLD.

### Design the backend of an online marketplace where users can buy and sell products.

Expected areas reported:

User authentication
Product listing
Product search
Shopping cart
Order management
Multiple items in an order
Different quantities
Partial cancellation
Full cancellation
Class structure
DB schema
APIs
Design patterns

### E-commerce + Warehouse Inventory

Design an e-commerce application and its warehouse inventory-management system.

Expected:

Requirements
DB schema
APIs
Inventory management


### E-commerce Order / Tracking / Delivery

Design an e-commerce platform order, tracking and delivery system.

Additional requirements:

Multiple items
Various quantities
Partial cancellation
Full cancellation

Source — SDE2 @ Kotak

4. Library Management System

Expected:

Entities
Relationships
APIs
DB schema
Design patterns
Search
Issue
Return
Availability

One report explicitly said requirements were supplied on paper and no coding was expected.

SDE-I Experience
SDE-1 Experience

5. Food Delivery System

Core entities reported:

User
Restaurant
Order
DeliveryPartner
Menu
Payment

APIs:

Place Order
Assign Delivery Partner
Track Order
Update Order Status

Patterns mentioned:

Factory
Strategy
Singleton

Also discussed:

Scalability
Real-time tracking
Fault tolerance
Extensibility

Kotak SDE-1 Bar Raiser

6. Social Media Platform

Design a social media platform.

Expected:

DB design
Class diagrams
Design patterns

Kotak Backend

7. Online Survey System

Design an online survey system.

Kotak SDE-2 Hyderabad

8. Google Drive-like File Upload

Scenario:

Upload a 100 GB file over an unstable 10 Mbps network.

Expected discussion around:

Chunking
Upload reliability
Handling unstable network
Large-file processing

Kotak SDE-2 Hyderabad

9. Notification Service

Design a notification system.

One experience specifically required implementation of:

Email
WhatsApp
Messages
Template registration
Template invocation
Local data structures

Example template concepts included:

OTP template
Promotion template
Template variables
emailAddress
mobileNumber
otp
name
couponcode

The interviewer specifically focused on the service class and two API implementations.

Notification Service LLD experience

10. Meeting Scheduler

Design a meeting scheduler.

Expected:

Functional requirements
Non-functional requirements
Entities
Relationships
DB schema
Core scheduling method
Recurring meetings
Prevent double booking
DB locking
Fields involved in locking

Kotak SDE-I Experience

11. Currency Exchange System

Design a currency exchange system.

Kotak SDE2 Selected

12. Airport Scheduler

Design an airport scheduler for a single runway.

Discussed:

Priority Queue
Priority handling
Race conditions
Starvation
Concurrency

Kotak SDE2 Backend

13. Design a document-upload service

Expected:

API specification
Class diagram
Database schema

Kotak SDE-2 Hyderabad

3. HLD / Distributed Systems Question Book
   Round 1 priority
1. Distributed backend for large-scale analytics

Design a distributed backend system capable of handling large-scale data processing/analytics.

Follow-up:

What are the considerations and best practices for deploying and scaling the distributed backend in a cloud environment?

Kotak SDE-2 Bangalore Backend

2. Data consistency in distributed systems

Discussed:

Quorum
Two-Phase Commit / 2PC
Leader-replica architecture

Kotak SDE-2 Bangalore Reject

3. Event-driven processing

How would you design event-driven processing?

Also:

DB selection
Scalability
Trade-offs

Kotak SDE-2 Backend

4. Scaling a system

How do you scale a system?

Kotak SDE-1 Bar Raiser

5. Distributed DB consistency

How do you handle consistency in a distributed database system?

Kotak SDE-1 Interview Experience

6. Deployment failure

How do you handle deployment in a failure situation?

Kotak SDE-1 Interview Experience

7. Horizontal vs Vertical Scaling

Explain horizontal vs vertical scaling.

Also:

Load balancing
Scheduling algorithms
Distributed systems basics

Kotak SDE-1 Interview Experience

8. Database / analytics for e-commerce

Given an e-commerce platform, how would you process very high volumes of data for analytics and prepare dashboards?

Which database would you consider?

SDE 2 @ Kotak

9. Notification System HLD

Design a notification system.

Kotak SDE2 Hyderabad

10. Fund Transfer System

Design a fund transfer system.

Kotak SDE-2 Hyderabad Backend

11. User ID Assignment System

Design a user-ID assigning system.

Kotak SDE-2 Hyderabad Backend

12. Credit Card Payment System

Design a credit-card payment system.

Kotak SDE-2 Hyderabad Backend

13. Log Aggregation System

Design a log aggregation system that receives logs from multiple microservices, aggregates/stores them and supports searching.

Kotak SDE3 Interview

14. Cricbuzz HLD

Design Cricbuzz.

Kotak SDE-2 Hyderabad

15. Voucher System

Design a voucher system with HLD + API design.

Additional discussion:

SQS
Kafka
CPU usage
Multiple instances
Database

Kotak SDE2 Bangalore Experience

4. Technical / Java / Spring / Backend Question Book

This is very important for your SDE-2 backend preparation.

Java / Concurrency
How do you handle concurrency?
Optimistic locking vs pessimistic locking?
How do you ensure availability?
Java Bean lifecycle
@PreConstruct vs @PostConstruct
Map vs ConcurrentHashMap
How does ConcurrentHashMap work internally?
Java locks
Various Java concepts
Java Internals
Java Memory Model
Multithreading
Virtual threads
Thread-safe implementation of a service
Race conditions
Starvation
Atomicity

These are explicitly reported across the SDE2 experiences.

Spring / Microservices
Why do we need asynchronous systems/Kafka?
Why use a queue instead of simply using @Async?
Downstream service fails — how do you still respond successfully to requests?
Resilience4j
Circuit breaker
@Cacheable
SQL vs NoSQL
Aspect-Oriented Programming — what is it and why?
Exception handling
Scalability
DB replication
Kafka internals
Producer / consumer / partition / offset
How do Kafka producers and consumers work?
Distributed messaging with Kafka/RabbitMQ
Distributed session management

Kafka-specific questions

One experience explicitly reported:

Difference between partition and topic
What is a consumer?
What is a consumer group?
If there are 2 partitions and 3 consumers, how are consumers distributed?
How do you ensure message ordering?

Kotak SDE-1 Interview Experience — Kafka questions

Another SDE-II experience reported a detailed discussion around:

Producer
Consumer
Partition
Offset
Kafka internals

Kotak SDE II Interview Exp.

Database / SQL

Reported questions include:

Primary key vs unique constraint
ACID properties
Explain Atomicity
Explain Consistency
Explain Isolation
Explain Durability
What is a transaction?
Types of transaction isolation
Database replication
DB indexing
SQL vs NoSQL
DB schema design
SQL query involving timestamps
Joins
CTEs
Indexes
Query optimization

5. Special Coding / Machine-Coding Questions

These are worth separating because they are not ordinary DSA.

Load Balancer

Implement a load balancer.

Follow-ups:

How do you make it thread-safe?
How do you make it memory-efficient?

Kotak SDE-I Experience

Concurrent Log Processor

Implement:

acceptLog(String logLine)
getUserEventCounts(String userId)

Requirements:

Multiple threads call acceptLog
Thread-safe
Efficient
Maintain user → event → count

Follow-up:

Explain how ConcurrentHashMap works internally.

Kotak SDE-2 December 2025

Rate Limiter

Discussion included:

4xx / 5xx handling
Retry tracking
Rate limiting
Different rate-limiting algorithms
Implementing one rate-limiting algorithm in low-level code

Kotak SDE-2 Bangalore Backend

6. Round 1 vs Round 2 — What the collected experiences show
   Round 1

The recurring structure is:

DSA + LLD + HLD/Distributed Systems + backend fundamentals

The strongest repeated patterns are:

DSA
Minimum Size Subarray Sum
Stock Buy/Sell
Two Sum
Search Rotated Sorted Array
DP
Kadane
Tree DP
LLD

E-commerce is by far the recurring design problem.

You should be able to design:

User → Product → Inventory → Cart → Order → Payment → Delivery

with:

classes
relationships
APIs
DB schema
indexes
design patterns
concurrency
cancellation
extensibility
HLD

Know:

scalability
load balancing
distributed consistency
replication
event-driven architecture
Kafka
cloud deployment
database selection
analytics pipelines
7. Round 2 — The important pattern

Round 2 is much more coding/problem-solving heavy in the SDE-2 reports.

The reported level ranges from:

LC Medium → Medium/Hard → custom optimization problems.

The important question families are:

Arrays / Sliding Window
Minimum Size Subarray Sum
Prefix Sum
Array deduplication
Rotated Sorted Array
DP
House Robber
Coin Change
Travel Passes
Tree DP
Wildcard Matching
Graph / Matrix
Longest Increasing Path in Matrix
DFS-based problems
Heap / Stack
Largest Rectangle in Histogram
IPO
Reorganize String
Asteroid Collision
Sliding-window min/max
Linked List
Prime/non-prime segregation
Strings
JSON nesting
Palindrome
Reorganize String
Wildcard Matching
8. Highest-repeat questions from the entire dataset

If we count recurrence across the supplied experiences, these are the ones I would put at the top of your question book:

🔴 Tier 1 — repeated heavily
E-commerce / Online Marketplace LLD
Minimum Size Subarray Sum
Best Time to Buy and Sell Stock
Two Sum
Search in Rotated Sorted Array
Distributed-system fundamentals
Java/Spring/Kafka fundamentals
DB schema + APIs + class design
Scalability
Database consistency / replication
🟠 Tier 2
Longest Increasing Path in Matrix
DP / House Robber
Coin Change
Kadane
Largest Rectangle in Histogram
Wildcard Matching
Prefix Sum
LRU Cache
Reorganize String
IPO
🟡 Tier 3
H-Index
Candy
Course Schedule II
Rotting Oranges
Asteroid Collision
Sliding Window Max/Min
Burning Tree
Tree DP
JSON nesting
Palindrome problems
Important correction to your preparation strategy

Because you specifically said Round 1 and Round 2 are your focus, I would not spend equal time on every question in this book.

Your preparation should be:

Round 1

DSA + E-commerce LLD + Distributed Systems + Java/Spring/Kafka

Round 2

Medium/Hard DSA + Java/backend follow-ups + complexity + optimization


### P0 — Must do
Sliding Window
Binary Search / Rotated Array
HashMap + Two Pointers
Stack / Monotonic Stack
Heap / PriorityQueue
DP — 1D
Tree DFS/BFS + Tree DP
Matrix DFS/DP
E-commerce LLD
Concurrency + thread safety in LLD


### P1 — Very important
Intervals + Greedy
String problems
Linked List
Backtracking
Notification System LLD
Meeting Scheduler LLD
DB schema + API design
Design patterns

### P2 — If time remains
Trie
Graph algorithms
Advanced DP
LRU Cache
Rate Limiter
Load Balancer

## Day 1.
### First
Minimum Size Subarray Sum ⭐⭐⭐⭐⭐
Longest Substring Without Repeating Characters
Longest Repeating Character Replacement
Minimum Window Substring
Container With Most Water
Best Time to Buy and Sell Stock
Two Sum
3Sum
Search in Rotated Sorted Array ⭐⭐⭐⭐⭐
Find Minimum in Rotated Sorted Array


### Next.
Minimum Size Subarray Sum
Two Sum
Stock Buy/Sell
Rotated Sorted Array
Container With Most Water

## Day 2.
Largest Rectangle in Histogram ⭐⭐⭐⭐⭐
Daily Temperatures
Valid Parentheses
Asteroid Collision
Min Stack
Top K Frequent Elements
Kth Largest Element
IPO ⭐⭐⭐⭐
Reorganize String ⭐⭐⭐⭐
Meeting Rooms II
Merge Intervals
Non-overlapping Intervals.

The important Kotak-specific ones:

Largest Rectangle
IPO
Reorganize String
Interval removal
Asteroid Collision

LLD - Ecommerce.

Concurrency - Be ready for - Two users try to purchase the last item simultaneously. What happens?

You should discuss -

optimistic locking
pessimistic locking
atomic update
DB transaction
race condition
idempotency

This is particularly important for your backend profile.

## Day 3.
House Robber ⭐⭐⭐⭐⭐
Coin Change ⭐⭐⭐⭐⭐
Climbing Stairs
House Robber II
Longest Increasing Subsequence
Partition Equal Subset Sum
Unique Paths
Decode Ways
Word Break
Wildcard Matching ⭐⭐⭐⭐⭐

House Robber
↓
Coin Change
↓
Travel Pass / Minimum Cost
↓
Wildcard Matching
↓
Tree DP

## Day 4.
Trees + Matrix + Graph

Tree
Binary Tree Level Order Traversal
Maximum Depth
Diameter of Binary Tree ⭐⭐⭐⭐
Lowest Common Ancestor
Binary Tree Maximum Path Sum
Tree DP / Maximum Independent Set ⭐⭐⭐⭐⭐
Burning Binary Tree ⭐⭐⭐⭐
Matrix
Number of Islands
Rotting Oranges
Course Schedule
Longest Increasing Path in a Matrix — LC 329 ⭐⭐⭐⭐⭐

DFS + memoization and Topological/BFS approach

## Day 5.
Ecommerce.

Notification System.
Strategy
Factory
Template
interfaces
extensibility

Meeting Scheduler.
double booking
recurring meetings
concurrency
optimistic locking
DB constraints

## Day 6.
Medium Problem.
Any LLD in the 3 listed.

Hard - Longest Increasing Path
Largest Rectangle
Wildcard Matching
Tree DP
Coin Change variation

Java
HashMap internals
ConcurrentHashMap
equals/hashCode
JVM memory
synchronization
locks
volatile
Atomic classes
ExecutorService
CompletableFuture
Spring
Bean lifecycle
dependency injection
@Transactional
transaction propagation
isolation
caching
exception handling
AOP
Kafka
partition
consumer group
offset
ordering
rebalancing
producer acknowledgements
delivery semantics
idempotency

## Day 7.

Priority	Problem
🔴 1	Minimum Size Subarray Sum
🔴 2	Search in Rotated Sorted Array
🔴 3	House Robber
🔴 4	Coin Change
🔴 5	Wildcard Matching
🔴 6	Longest Increasing Path in Matrix
🔴 7	Largest Rectangle in Histogram
🔴 8	Tree DP
🔴 9	Two Sum
🔴 10	Best Time to Buy/Sell Stock
🟠 11	Container With Most Water
🟠 12	IPO
🟠 13	Reorganize String
🟠 14	Merge Intervals
🟠 15	Non-overlapping Intervals
🟠 16	Diameter of Binary Tree
🟠 17	Asteroid Collision
🟠 18	H-Index
🟠 19	Candy
🟠 20	Group Anagrams



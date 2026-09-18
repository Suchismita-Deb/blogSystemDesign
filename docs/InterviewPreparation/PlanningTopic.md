Solve - Design - Defend - handle Failure - Explain trade-off

### DSA 

Roman to Integer 
Longest Common Prefix
Two Sum
Best Time to Buy and Sell Stock
Minimum Size Subarray Sum
**Search in Rotated Sorted Array**
Remove Duplicates from Sorted Array II
Max Consecutive Ones III
Remove All Adjacent Duplicates in String

Set Matrix Zeroes
Sliding Window problems
DP / House Robber-type problem
Interval scheduling — minimum intervals to remove
Priority Queue / First Unique element stream
Tree DP — maximum coins with non-adjacent nodes
Find Peak Element

**Minimum Size Subarray Sum**
Longest substring with at most K distinct characters
Check if permutation of one string exists as substring of another
Burst Balloons
Longest Increasing Path in a Matrix
Minimum number of intervals to remove
Word Ladder<br></br>
https://leetcode.com/problems/water-and-jug-problem/description/
LFU Cache.
https://leetcode.com/problems/longest-consecutive-sequence/description/
https://leetcode.com/problems/group-anagrams/description

2nd round DSA - qs 1: Stack overflow implementing-shuffle-functioning-in-a-music-player. qs 2: GFG print-all-combinations-of-given-length
3rd round system design: Design survey monkey and analytics as a Saas, APIs, DB design, LLD and HLD flow
Container With Most Water.

Kadane's Algorithm / Maximum Subarray-type problem

Coin Change — DP

Minimum-cost travel passes — 1-day, 1-week, 1-month passes; cover given travel days with minimum cost
House Robber-type DP
Tree maximum independent set — each node has coins; choose non-adjacent nodes to maximize sum; DFS + DP
Variation of Jump Game — brute force → DP
Variation of Jump Game — brute force → DP
Merge Two Sorted Arrays in-place
Merge Two Sorted Arrays — compare extra-space and in-place approaches
Smart Cache with Dependency-Aware Eviction — cache items have dependencies; invalidating one invalidates dependents; minimize recomputation cost
Longest Increasing Path in a Matrix — LC 329
Largest Rectangle in Histogram — LC 84
Wildcard Matching
Prefix Sum problem
Max Robber / House Robber DP
Minimum intervals to remove to make remaining intervals non-overlapping
IPO — LC 502
IPO — LC 502
Reorganize String — LC 767
Segregate prime and non-prime numbers in a Linked List — variation of Odd-Even Linked List
Array/string question — exact problem not disclosed
JSON nesting count — count nested objects in a JSON string
Power + recursive digit sum — calculate 2^n, repeatedly sum digits to one digit; n ≤ 10^9
Find element in rotated array
Find all palindromes for a string; optimized version
H-Index — optimized O(n) approach
Candy — prefix/suffix approach
Permutations — LC 46
Minimum counters required — scheduling problem; PriorityQueue solution
Diameter of Binary Tree — O(N²) vs O(N) single traversal

Top N elements in a repeating array of integers, accounting for ties.
https://leetcode.com/problems/longest-valid-parentheses/
Dp
Tree
Rotten Oranges.
https://leetcode.com/problems/h-index/
https://leetcode.com/problems/candy/
https://leetcode.com/problems/course-schedule-ii/description/

https://leetcode.com/problems/rotting-oranges/
Diameter of Binary Tree.
Minimum counters required (scheduling-type problem)
Max robber dp question.
Several intervals are given, find the minimum intervals to be removed so that the remaining intervals are non-overlapping.
https://leetcode.com/problems/largest-rectangle-in-histogram/description/
https://leetcode.com/problems/odd-even-linked-list/
House Robber 3 - Maximizing the DP pattern.
https://leetcode.com/problems/minimum-cost-for-tickets/description/

JSON Internals Count
Given a JSON string, I was asked to find the number of nested objects.
Example:
Input: {{}{}}
Output: 2

Power and Recursive Digit Sum
Given n, compute 2^n, then recursively sum the digits of the result until a single digit remains.
Constraints: 0 ≤ n ≤ 10^9.

https://leetcode.com/problems/first-missing-positive/description/

https://leetcode.com/problems/burst-balloons/description/

https://leetcode.com/problems/longest-increasing-path-in-a-matrix/description/
Longest palindromic substring. Three Approaches.
Buy and sell stock (All variations from 1 to 5)+ Dry run.
Given an array like 1,2,2,2,3,3,3,4,5,6 change it in place such that any character repeats only twice and return the length of the new array output 1,2,2,3,3,4,5,6,5,6 . Array ends at index 7 where the first 6 is present . Any element after index 7 can be ignored . Remove Duplicates from Sorted Array II.

In a binary tree any random node is put on fire . How much time will it take for the tree to be burnt if 1 edge takes 1 unit time . Equates to finding the farthest node from a tree node . Burning Tree All Nodes Distance K in Binary Tree.

Leetcode: Wildcard Matching
Prefix Sum Problem

find all palindrome for a strings
DSA - Simple questions - Build Queue functionality using arrays.


In list of strings, get the common starting substring.

Longest Increasing Path in a Matrix [Leetcode 329]

Implement Min Stack from Scratch

Expected implementation using Linked List
Focus on optimized operations and design clarity.

Top K Frequent Error Codes from Log Stream

Merge Intervals.

Merge Two Sorted Arrays.
https://leetcode.com/problems/group-anagrams/description/
https://leetcode.com/problems/permutations/description/
Reverse a string without using extra space

https://leetcode.com/problems/kth-largest-element-in-an-array/description/

https://leetcode.com/problems/ipo/description/

https://leetcode.com/problems/reorganize-string/description/

Asteroid Collosion using Stack(Standard Leetcode Prob)
Diff bw max and min in a window of size k in an array.(Using Deque)
### Java
Some questions on Exception handling,Scalability & DB replication

Implement ranking system in SQL.
Java internals.
How does HashMap work internally?
HashMap vs ConcurrentHashMap?
synchronized vs volatile vs Atomic classes?
What is a race condition? Give a real example.
How does ExecutorService / ThreadPoolExecutor work?
Things to know - corePoolSize, maximumPoolSize, queue, keepAliveTime, rejection policy.  
Write a Code Snippet on how Immutable Classes work.
SQL query - dealing with timestamp.
Concurrency? How would u deal it?
Availability? how u make sure ur system is always available
optimistic locking pessimistic locking?
Bean life cycle
Pre-construct vs post construct
Why we need asynchronous systems/kafka
why async systems, why not achieve it with @Async annotation instead of queue’s
u call downstream, how do u ensure that you respond 200 to all ur requests despite failures from downstream
Resilience4j?
Cacheable annotation
circuit breaker?
resilience 4j?
sql vs no sql
Virtual threads in Java?
Map vs ConcurrentHashMap?
What is Aspect Oriented Programming? Why do we need it?

SQL(joins, CTEs, indexes, queries)
```text
You are asked to design a Concurrent Log Processor in Java.

Log entries come as strings in the format:

userId,eventType,timestamp

Example Logs
u1,LOGIN,2026-01-01T11:15:10
u2,LOGOUT,2026-04-12T11:16:00
u1,LOGIN,2026-04-11T12:17:10

Multiple threads will push logs concurrently.

Requirements

Implement a class LogProcessor with:

public void acceptLog(String logLine);
// Called by multiple threads

public Map<String, Integer> getUserEventCounts(String userId);

acceptLog must be thread-safe

getUserEventCounts("u1") should return something like:

{
"LOGIN": 2,
"LOGOUT": 1
}

Solution should be thread-safe and reasonably efficient

My Solution :

Used ConcurrentHashMap

Outer map: userId → eventMap

Inner map: eventType → count

Interviewer was okay-ish with this approach

Follow-up Questions :

Interviewer asked how I would implement ConcurrentHashMap internally

I explained basic ideas like:

Fine-grained locking

Locking on writes

Concurrent reads

However, I lacked deep knowledge of its internal implementation

I also suggested a scheduler-based batching approach

Accept logs into a queue

Periodically update aggregates

This introduces slight latency in reads

The interviewer did not seem fully satisfied with these answers.



 ConcurrentHashmap is required in this case because the acceptLog function is called by many threads. That's why if we use simple HashMap, its states is shared by multiple threads which updates the map and reads from it. This is exactly where a simple HashMap fails because additions and updates in a HashMap changes its states and it will break for other concurrent operations. You have to justify your choice for using ConcurrentHashMap in interviews as they are thread safe and locking is applied at bucket level so performance would be good for large number of events as well.

`
public class LogProcessor {

public ConcurrentHashMap<String, ConcurrentHashMap<String, Integer>> map
        = new ConcurrentHashMap<>();

public void acceptLog(String logLine){

    String[] parts = logLine.split(",");

    String userId = parts[0];
    String action = parts[1];
    // LocalDateTime timestamp = LocalDateTime.parse(parts[2]);
    // This is not required as it's not returned

    map.computeIfAbsent(userId, u -> new ConcurrentHashMap<>())
            .merge(action, 1, Integer::sum);
}

public Map<String, Integer> getUserEventCounts(String userId){
    return map.get(userId);
}
}
`

As you were told to write the code, this approach should be fine but if interviewer isn't satisfied, you can defend it by talking in terms of scalability, more threads come in play, idempotent solutions in case multiple thread sends same data for a user event to the acceptLog function.



I wrote similar solution only and interviewer didn't say that it will not work, but he asked me if i have to design it without ConcurrentHashMap or say i have to implement the ConcurrentHashMap, how would i do it my self.
Hence i suggested my approach for implementing ConcurrentHashMap but i was not able to come up Segment/Bucket Level and also i have proposed the Batch based solution but again i don't think the interviewer was satisfied with it or i don't know if he wanted any other solution.

He mentioned for a load of say 1 Million concurrent thread requests as input, not sure if single server will be able to handle such requests, but thats what he asked for.




1 million is a large number. Any kind of solution would need locking. Even If we use lock striping inspired by ConcurrentHashMaps, the lock contention is going to be huge. Was the interviewer talking about super computers? This seems impossible for a single server.



1 million “concurrent” requests for sure is unrealistic on a single jvm, there is a thread limit for every hardware and even if context switching is acceptable still its a big overhead and not possible for this use case. If more users are involved, sharded maps can be used. This is an ideal fit for production level applications as userId itself is the map key, the single map can be sharded based on userId. This reduces data stress on single concurrent hashmap. I personally believe hot key is a not a problem here because realistically this type of event ingress is limited (per user), but still if it’s a case use rebalancing. In this kind of question, you have to walk even though your hands are tied (single machine), come up with whatever best you can give to the interviewer. I would’ve stated this answer for your followup if i were there.

```
SQS and Kafka
CPU Usage and Multiple Instances
Question related to database
### LLD
Design Pattern.
Design a Fund Transfer System
Design a User ID assigning System
Design Credit card payment System
Design a notification system.
HLD - Log aggregation System which can take logs from multiple microservices and aggregate the logs and store. Also searching capability on top of the logs.
Design an Online Survey System.
LRU cache implementation.
LLD - Template based notification Service - Different users can save their templates in the system and later they will just send the variables in the template. Using those variables and template you need to send notification.
Flight ticket booking system . First HLD was asked then LLD on entities and DB schema . Questions on auth and authorization

system similar to Google Drive. The scenario involved uploading a 100 GB file over a network with 10 Mbps speed, which was unstable.

Design cricBuzz HLD : Discussion went very well.

limit system in Kotak. This should limit based on number of transactions, amount per user’s payment method.

Low-Level Design (LLD): Notify Me Service

HLD & API Design: Voucher System Design.


Design a Hospital Management System. Requirements:

Patients are first registered at Reception, who assigns them a priority level (Critical, High, Medium, Low).
Patients wait in a priority queue.
An Initial Consultation Doctor picks the next patient from the queue.
After consultation, patient is assigned to a Specialist Doctor.
System should maintain treatment records.
Working code.



Music Player Design:
I was asked to implement a music player supporting:

Play

Next / Previous

Repeat song

Repeat playlist

tradeoffs, scaling, db indexes, etc
Ecommerce cart system - lld
Design Currency Exchange System
Design an Airport Scheduler (single runway).
Covered priority queue design, priority handling, race conditions, starvation, and concurrency.

Design a web crawler to fetch jobs and job data from the career page of kotak, how will you scale and optimize your design?
Smart Cache with Dependency-Aware Eviction

Design a caching system where items can have dependencies. If an item is invalidated, all dependent items must also be invalidated. The goal is to minimize total recomputation cost across access patterns.
HashSet → cache
Graph → dependencies
DFS → calculate eviction impact
Strategy:
On cache full → evict item with minimum dependency impact
Recursively invalidate dependent items

Design an Online Survey System
Design an E-commerce system - Main User, Product, Cart, Inventory, Order, Payment, Shipment. Follow up - inventory race condition, payment failure, order state, database schema, APIs scaling.

Design a Food Delivery system - Main part - Customer, Restaurant, Menu, Order, Payment, DeliveryPartner, Delivery. Follow up - assign delivery partner, order state machine, restaurant unavailable, payment failure.

Design Instagram - Main part - User, Follow, Post, Like, Comment, Feed.  
What happens when a celebrity with 50M followers posts?

Design Ride Sharing - Main part - Rider, Driver, Trip, Location, Payment. Follow up - nearest driver, concurrent booking, driver state, cancellation.

Design Parking Lot - multiple floors, vehicle types parking spots, ticket, pricing, availability.


Design Parcel Delivery System - DB schema, class structure, APIs, service design, Strategy, Observer.

Design Flight Ticket Booking - Main part - Flight, Aircraft, Seat, Passenger, Booking, Payment. Follow up - Two users attempt to book the last seat. What happens?

Design Notification Service - Main part - Email, SMS, Push. Startegy pattern.

Design Job Scheduler - Job, Schedule, Worker, Queue, Retry, Dead Letter.

Design Logger - Logger, Appender, Formatter, LogLevel. Chain of Responsibility, Factory, Singleton — and discuss why you'd be careful with Singleton.

| Day   |  DSA |  LLD | Revision |
| ----- | ---: | ---: | -------: |
| Day 1 |   3h | 1.5h |     0.5h |
| Day 2 |   3h | 1.5h |     0.5h |
| Day 3 | 3.5h |   1h |     0.5h |
| Day 4 |   3h | 1.5h |     0.5h |
| Day 5 | 1.5h |   3h |     0.5h |
| Day 6 | 2.5h | 1.5h |       1h |
| Day 7 |   2h |   2h |       1h |

Ecommerce - Notification System - Food delivery - Meeting scheduler - Library Management.

Write a Code Snippet on how Immutable Classes work - API Spec, Class diagram & Schema design.


Its a bank - the concurrent and synchronized part are imp.

### Distributed System.

What is a distributed lock?
Redis lock, TTL, lock expiration, process crashes, why distributed locks can be dangerous.

When should you NOT use a distributed lock?

Explain the Outbox Pattern.
Business DB + Outbox table - Same transaction - Outbox publisher - Kafka.

What is backpressure?
Producer = 100K events/sec
Consumer = 20K events/sec

Producer = 100K events/sec
Consumer = 20K events/sec


What is SAGA Pattern?

What is 2PC?

What is quorum? 
N = replicas  
W = write quorum.  
R = Read quorum.


### HLD.

Design an Online Marketplace.

Design a Twitter/X-like system.

Design a Trading System.

Design Notification System.

Design File Processing Pipeline.

Design URL Shortener

Design Rate Limiter

Design Distributed Cache


### Database.

Explain ACID.

Explain database isolation levels.  
Read Uncommitted, Read Committed, Repeatable Read, Serializable.

Optimistic vs pessimistic locking?

How does an index work?

Composite index — how does column order matter?

When can an index hurt performance?

How do you optimize a slow SQL query?

Normalization vs denormalization?

Sharding.

### Spring.

How does Spring Dependency Injection work?

What is Spring Bean lifecycle?

@Component vs @Service vs @Repository?

How does @Transactional work internally?
Spring Proxy - Transaction begins - Method executes - Commit/Rollback.

Why does @Transactional sometimes not work on self-invocation?

Transaction propagation types?
REQUIRED, REQUIRES_NEW, NESTED

Transaction isolation? Connect this with your DB knowledge.

Spring AOP — how does it work?

Filter vs Interceptor vs AOP?

How does Spring Boot auto-configuration work?

synchronized vs Lock?

What is deadlock? How do you prevent it?

volatile — what does it guarantee and what doesn't it guarantee?

CompletableFuture — how does it work?

How would you design a thread-safe cache?



Design a Rate Limiter

I was quite well prepared for this topic, so the discussion went very well.

We discussed multiple approaches/algorithms for rate limiting, including:

Fixed Window
Sliding Window
Token Bucket
Distributed rate limiting
Redis
Locks/concurrency
Handling rate limiting across multiple application instances
Trade-offs between different approaches



















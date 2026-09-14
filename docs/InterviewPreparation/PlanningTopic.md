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
Minimum Size Subarray Sum
Longest substring with at most K distinct characters
Check if permutation of one string exists as substring of another
Burst Balloons
Longest Increasing Path in a Matrix
Minimum number of intervals to remove
Word Ladder

Sample Round.

DSA
├── Minimum Size Subarray Sum
└── Search in Rotated Sorted Array

LLD
└── Instagram

HLD
├── celebrity traffic
├── caching
└── message queues

### Java

How does HashMap work internally?
HashMap vs ConcurrentHashMap?
synchronized vs volatile vs Atomic classes?
What is a race condition? Give a real example.
How does ExecutorService / ThreadPoolExecutor work?
Things to know - corePoolSize, maximumPoolSize, queue, keepAliveTime, rejection policy.  

### LLD

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























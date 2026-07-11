# Common Pattern - The success is all about the pattern.

## Pushing realtime update.

It means delivering immediate notification from sulphur to client as it occurs  
 
Example - It enables low latency and bidirectional communication In case of chat application. 
 
 Example in Google docs for collaborative editing one user making and change will be reflected in a millisecond And it cannot The polling for the server for every updates Next time.
 
The target - efficient persistent communication between clients and servers.   
Http follows a request response model.  

Problems which use real time updates includes - 
Ticketmaster, Whatsapp, Google Docs, Uber, Robinhood, Strava.

There are main 2 parts - How to get the update from the server to the client and how to get the update from client to server.

## Client-Server Connection Protocol.

The first part - make a communication channel between client and server. HTTP request-response is not suitable for real time update as it is unidirectional and stateless and real time system needs persistent connection or poling to enable server to push update to client.

### Networking Layers.

There are 3 layer imp for developer - Application layer, Transport layer and Network layer.  

**Network Layer** - Layer 3 - IP addressing and routing. Data breaks into packets and provide best effort delivery to send to the destination. There is no guarantee of delivery and it can get lost.

**Transport Layer** - Layer 4 - TCP and UDP.    
TCP is **connection oriented** and provide reliable delivery and in order. The connection should be established first and it takes time to maintain resource and bandwidth.       
UDP is **connectionless** and faster but no guarantee of delivery or order. It send data to any IP without any prior set up. Real time system use UDP for low latency but it can tolerate some loss like video streaming.    
For critical data like chat application use TCP.

Application Layer - Layer 7 - Protocols like DNS, HTTP, WebSockets, gRPC. These are common build on top of TCP.

**Request Lifecycle**  
**URL** - **DNS convert to IP** - **TCP connection and TCP handshake** (Syn, Syn Ack, Ack) - **HTTP request** (TCP establish te clients ends GET request to server) - **Server process** the request and send the **HTTP response** - **TCP Teardown**.

The client initiate a TCP connection with the srever - SYN( The client send a SYN packet to the server to request a connection) - SYN ACK (The server respond with a SYN ACK packet to acknowledge the connection request) - ACK (The client send an ACK packet to acknowledge the server's response and establish the connection).

The data transfer is complete the connection close using 4 way handshake - FIN, ACK, FIN, ACK.  

FIN (The client send a FIN(finish) packet to the server to close the connection) - ACK (The server respond with an ACK packet) - FIN (The server send a FIN packet to the client to close its side of the connection) - ACK (The client acknowledges the server).

> The TCP all the round trip adds latency to the requests and TCP connection is state and client and server need to maintain. There is feature HTTP keep-alive to not make the connection set up for every request.

**Load Balancer**  
There are 2 main type L4 and L7.
Layer 4 - operates on TCP/UDP - they make routing decision based on IP address and post and not considering the packet.  
Key characteristics - maintain TCP connection between client and server


User need to see the new message send to the chat room.
### Simple Polling.






## Scaling Reads.
The need to scale read is in physics - CPU core can execute limited num of instruction per second, disk I/O bounded by the speed of the spinning platters or SSD write cycles. More code will not improve the case.  

> The scaling read included - optimize read performance within your database using _indexing_ and _denormalization_, _scale horizontally_ with read replicas and _add external caching layers_ like Redis and CDN.

### Optimize the db.
Index - An index is essentially a sorted lookup table that points to rows in your actual data. Without index its full table scan O(n) and with index its log time O(logn).
B Tree and hash index for exact matches and specialized index handle full text search. Index on the column frequently query, join on or sort by to manage the read scale. Example - User search by the hashtag then index the hashtag column, user sort products by price then index the price column. Too many index will slow down the writes - outdated modern gardware and db easily pull up. Index add write over head and consume buffer cache memory and add write latency.


Improve hardware - SSD and not spinning disk and SSD will give 100 x faster random I/O. Adding RAM means the data will sit on memory instead of disk. Using faster CPU and more cores will handle more concurrent queries.

You mention upgrading hardware is a solution but costly and it will give a bit of relax.




### Denormalization Strategies.

### Scale Db Horizontally.
General rule db needs to scale horizontally or adding cache when the request exceeds 50k-100k read request per second. Its an assumption and it depends on pattern, data model.
Solution - Read replica, db Sharding.

Read replicas - First approach adding read replicas. All writes in primary and read in replicas. It solves the throughput problem and redundancies like leader follower replication. The write is from the leader to followers.   
Issue - Replication lag when user write they might not see the update.  
Replicaton to the follower can be syncronousonous or asynchrounous.


Db Sharding - Read replica dont reduce the data set size that each data set handle. In case data set is large that indexed queries are slow then sharding will help in splitting data in databases.
In read scale shard helps in - smaller datasets and distribute read load in multiple databases.

Functional shard splits data by features and not by records. User data in one database, product search in one database.

Client - Load Post - Server - Fetch post (Db1)
- Fetch User (Db2)
- Fetch likes(Db3)

Geographic shard is effective for global read scaling. Store Us data in US. Shard add complexity and mainly a write scale solution. In read scale adding cache is more effective.

### Adding Cache.
In optimized db in case the read is not optimized then Solution - _**Adding external cache, cdn and edge caching**_.

Caching when the content does not change and read replicas when the data needs to stay fresh. Example same post or product then the data is not changing then cache. Db read the data from disk and execute the queries and cache gave the data from RAM.

**Application Level Caching** - In mem cache like Redis and Memcache sits between application and database.  

Cache invalidation shwuld be taken care and there are many ways.  
- *Time based expiration (TTL)* - Update cache after a fixed time. The issue - it will serve state data until expiration. Use when the data update in a predicted pattern.   
- *Write through invalidation* - Update cache when there is any write in db and cache will fetch the data from the db. It adds latency to writes.
- *Write behind invalidation* - Using asynchronous way cache invalidation. It reduces latency but there were cases where it might get stale data.  
- *Tagged invalidation* - Invalidate all entries with the tag `user:123:posts`  
- *Versioned keys* - It include version number in cache keys. Increment the version number and invalidate the old cache.

Most example - TTL (5 mins) and active invalidation for data like user profile or inventory count. Less critical data rely on TTL. TTl defined in the requirement in case the ask is that search should no more than 30 sec state then TTL 30 sec.

**CDN and Edge Caching** - CDN extends the cache beyond data center to global edge location. CDN manage static data and also dynamic data including API and db result.
Geographic distribution helps in latency improvement - User from Tokyo will get the edge server in Tokyo. The issue is cache invalidation of the edge server to get the latest data from the server.

CDN cache the data all users are mainly searching no need to cache personal details or single user preference.

> Read scale - first see the external API call the high volume - optimize it. 
> 
> Start with the query optimization then cache and read replica.
> 
> You should e able to determine the read blocker. Example When designing the API input like "The userprofile endpoint will get hit everytime someone view the profile. With millions of users and billions of read we need to take care. Will cover in deep dive."

## Common scenarios.

URL Shortener/ Bitly - URL shortened once and read million times. Its a ideal cache candidate. Cache the short URL to long URL mapping in Redis with no expiration - the url does not change. CDN to handle global traffic. The db is hit in cache miss or unpopular link.

Ticketmaster - Event page to cache as everyone visit the page. The seat availability can't be cached as it will change.

News Feed System - Linkedin, Twitter feed generation is read intensive. Precompute the feed for active user - cache recent post from followed user - smart pagination to avoid loading entire feed at once. User read first few posts no use of caching all data.

Youtube - Video metadata creates read load. The recommended video, channel info, view count does not change. Cache video metadata aggressively - title, description as they dont change. View count is eventual consistency and CDN for the thumbnail.

When to not use read scale -  
Write heavy system - Uber driver updating the location every second.  
Small scale application.
Strongly consistent system.  
Real time collaboration system.

> Real scale is to reduce the db load. In case db handle the load and the application need low latency then its a different problem (edge computing or service mesh optimization).


### Queries started taking longer as the data set grows.

Application got popular and CPU at 100% and simple search query running 30 sec. User login to see the email then db did 10 million scan to the record matching user. In joins users order means scanning user table to get the specific user then scanning entire order table for the match with the user. It will be billion of comparision.
200 bytes per row and 2gb of data to scan times the total number of condition. Solution - index on the column used more in queries. Without index it does a full scan.

```sql
-- Before: Full table scan
EXPLAIN SELECT * FROM users WHERE email = 'user@example.com';
-- Seq Scan on users (cost=0.00..412,000.00 rows=1)

-- Add index
CREATE INDEX idx_users_email ON users(email);

-- After: Index scan  
EXPLAIN SELECT * FROM users WHERE email = 'user@example.com';
-- Index Scan using idx_users_email (cost=0.43..8.45 rows=1)
```

In compound queries the column order in the index matter, User search by status and created_at the index will be (status, created_at).
In schema design mention the column used in indexing.

### Millions of concurrent user read on the same cache data.
Cache can handle 50k request per sec and the celebrity post got 500k per sec. Problem - The traditional cash assumes the load distributed across many keys and in celebrity post this assumption bricks. The cache has finite cpu and network capacity and sending 500k request using single server is huge task.  
The solution request coalescing - combining multiple requests for the same key in single request.

```java
import java.util.concurrent.*;

public class CoalescingCache {
    // Map of inflight requests: key -> Future
    private final ConcurrentHashMap<String, CompletableFuture<String>> inflight = new ConcurrentHashMap<>();

    // Simulated backend fetch (replace with real implementation)
    private String fetchFromBackend(String key) throws InterruptedException {
        Thread.sleep(100); // simulate latency
        return "Value for " + key;
    }

    public CompletableFuture<String> get(String key) {
        // If another request is already fetching this key, return its future
        CompletableFuture<String> existingFuture = inflight.get(key);
        if (existingFuture != null) {
            return existingFuture;
        }

        // Otherwise, create a new future and put it in the map
        CompletableFuture<String> future = new CompletableFuture<>();
        CompletableFuture<String> prev = inflight.putIfAbsent(key, future);

        if (prev != null) {
            // Another thread beat us to it, reuse their future
            return prev;
        }

        // Perform the backend fetch asynchronously
        CompletableFuture.runAsync(() -> {
            try {
                String value = fetchFromBackend(key);
                future.complete(value);
            } catch (Exception e) {
                future.completeExceptionally(e);
            } finally {
                inflight.remove(key);
            }
        });

        return future;
    }

    // Example usage
    public static void main(String[] args) throws Exception {
        CoalescingCache cache = new CoalescingCache();

        CompletableFuture<String> f1 = cache.get("foo");
        CompletableFuture<String> f2 = cache.get("foo");

        System.out.println(f1.get()); // both will print "Value for foo"
        System.out.println(f2.get());
    }
}

```
- Request coalescing will reduce the backend load from infinite(all users making the request) to N where N = number of the application server.
  The backend will recieve N request - one per server doing the coalescing.

In applications when coalescing isn't enough then distribute the load.
Cache key fanout spreads a single hot key across multiple cache. Instead of storing it under one key it store the identical celebrity posts copy under 10 different key. Total of 500K requests per second will be spread across ten key at 50K each the cache can handle the load. Append a number with the key - `feed:taylor-swift:1`.  
The issue is memory usage and cache consistency. Storing the same data in multiple times and invalidation becomes more complex because you have to clear all copies. In heavy scenarios where hotkey will risk availability the redundancy is a small price for staying online.

### User try to rebuild an expired cache.

In the application the home page data gets 100 K request per second serving from the cache and it has one hour ttl After one hour the entry expires and 100 K request will go cache miss and all request will go to the db. The db can handle 1000 queries per second I'm getting 100K request will act as a ddos attack. The cash stampede happens because the cash expiration is binary one moment that it exists and the next it does not.   
The problem multiplies when requires joining say the homepage will be generated after joining with 10 tables or calling external api - more parallel calls.  
Solution **distributed locks** to serialise rebuilds. The first request will get rebuilt while everyone else wait for the rebuild to complete.   
The issue - In case the rebuilds fails or takes longer thousands of requests timeout will happen. We need to add complex timeout handling process and fallback logic.   

Solution - Use probabilistic early refresh - Serving cached data when refreshing it in the background. It will refresh the cache before expiring say the expiry is in 60 minutes, a request at min 50 ve have 1% chance of refreshing it, at min 55 it will have 5% chance and at 59 it might be 20%.  
Not giving 100K request at minute spread the request across last 10 to 15 minutes. Most user will get the cache data while few of them will trigger refresh.

Th most critical cached data the approaches are not good. Background refresh should continuously update before expiration. The home page refresh every 50 mins so it never trigger rebuilds. The cost is complex structure and wasted refresh when it is not requested.


### Cache invalidation when data update needs to be immediately visible.

Event organizer updates the address and the attendees were not able to see that updates for another one As it's cached somewhere. The data might be cached in multiple layers like redis, CDN edge, browser cache. Invalidating all of that would be hard.  
Solution A naive approach is deleted the cache entry after a write. Sounds simple but it has problem  -  
Which cache do you delete  application cache cdn or browser?   
What will happen if an invalidation request fail?  
What if the request comes in right after you delete the old value but before adding the new venue - It will compute the cache stale data again. It is the race condition.

Solution a better approach for entry level data is cache versioning. Instead of deleting old cache we make them irrelevant by changing the cache key When the data is changed.

Each record has a version's number in the db when the record is updated the version is incremented in the same transaction. 


**On read** - Read the version number from the small "version key" (cached) or fall back to the DB. Construct a cache key using that version like `event:123:v12`. Read the data using that version key. On cache miss fetch from db and write it back using the same version key.     
**On write** - Begin a db transaction, update the row, increment the version column and commit then write the new data into the new version key.

The old cache is not deleted it's just become unreachable. Cdn browser cache expire stale version as the version is part of the url or cache key.

No race condition as db forces a new version number.
No partial invalidation as its not deleting cache its rerouting. 
Its good under concurrency as version change are atomic in db.

The issue - There are 2 cache lookups per request - to get the version number and the actual data. Old cache will store stale data and it will accumulate as its not deleted so set the TTL to clean up the stale data.
The pattern works in single entity caches like user profiles or product details and no with computed data like search results or feeds where invalidation is difficult.

When versioning is not practical - caching search results or aggregated data - need explicit strategy.   
Solution - Use **deleted items cache** its a smaller working set of items that have been deleted, hidden or changed.  


Instead of Invalidating all field Continue the deleted post we maintain a cache of recent deleted post id.  When serving feed we first see the small cache and filter out matches. It will help to serve mostly correct cached data immediately and background work on the invalidation of the complex data.

In global system - CDN caching invalidation is difficult making update in hundreds of edge locations.   

CDN APIs help but takes time - critical update use cache header to prevent CDN caching - trading performance for consistency. Rest of the data use shorter TTL at the edge and maintaining longer TTL in application. 

> Data have different consistency need user profile update fine with 5 min staleness but venue update should be immediate.



## Managing long-running task.
Task takes too long for synchronous processing - video encoding, report generation, bulk operations. Managing long running task pattern splits API request into **acknowledgement** and **background processing**.

User sends the task - web server push to queue(RabbitMq or Kafka) and returns the job id. Worker pulls the jobs from the queue and execute. Main part queue for coordination and worker pools for processing. The job done then notify via email or push notification or websocket update.
Use wants to see the profile page - db fetch the data and format the response - 100 ms. The job like generate the user activity pdf for a year - query multipl etable, aggregate the data, render the chart and make a document 45 sec of job. User will not wait and make a refresh more issue. Synchronous will not work.

The _async_ part means the original HTTP request completes without waiting for the work to finish and the _worker pool_ refresh the collection of processes dedicated to executing the background task.

The webserver does not need expensive GPUs to process video uploading the worker can use GPU instance and each part can scale when needed. The month end report generated then get more workers in the system.
This pattern is used in everyplace where the job takes more than a few seconds. Example - Image processing, video transcoding, bulk data imports, third-party API calls with strict rate limits, report generation, email campaigns.

**Pros of the system**

_Fast user response time_ - User gets an acknowledgement that the job is received.   
_Independent scaling_ - Scale the worker when needed.   
Fault isolation - A worker crashed processing one video will not stop the API the failed job will be retried without user inputs.   
_Better resource utilization_ - CPU intensive worker runs on compute optimized instances. Memory heavy task gets high memory machines and web severs use general purpose instance.

**Cons of the pattern**

_System complexity_ - queue, worker, job status tracker - more parts meaning more parts can break.  
_Eventual consistency_ - Worker will not see the updates when they are doing the job. User will see stale data time of starting the request to the execution.    
_Job status_ - Store the job status traction, handle retries expose status endpoint.  
_Monitoring_ - Need to see the queue depth, worker health, job failure rates and monitorisng a distributed system and not a request response.
_Planning in the design_ - There will be some design like what happens when the queue is fills up? How to handle poison message that crashes the worker? When to stop retry the failed job?

### Implementation.

The message queue and the pool of workers.


**Message Queue.**

Redis with Bull/BullMQ - Redis provides the storage and the Bull adds the job queue automatic retries, delayed jobs, priority queue.  
Redis adds persistence options the point is its memory first the job will be lost in hard crash. Any durability add the Rabbit MQ or SQS.

AWS SQS - No operational overhead. Amazon manages the infrastructure and scale. Its pay per message - good when less data and expensive at scale. The limit is 1 Mb so the data is storing in separate place and the queue is used to store the job id.

RabbitMQ - Its self hosting and good in complex routing pattern. The cluster, upgrades and disk usage are all manual.

Kafka - Its append only log, replay of the messages, fan-out to multiple consumers - retention time and ordering inside the partition.

**Pool of worker.**

How to run the worker - Normal server, serverless function, containerized service.  
**_Normal server_** - There are 20 worker process on machines and each executing jobs pulling in the queue. Pros - control over the environment, easy to debug ssh to the server and see the logs. Cons - need to manage the server and pay for the idle capacity in low period.

```java
while (true) {
    Job job = queue.pop(); // Blocks until job available
    if (job != null) {
        processJob(job);
        markComplete(job.getId());
    }
}
```
_Serverless function_ - Lambda, Cloud function, Azure function. Pros - no server management, auto scaling, pay per execution. It is good for spiky workload like one minute 1000 and next 100. Cons - It is limited to 15-60 minutes execution time, cold start adds latency and local storage is minimal.

_Container-based worker_ - It deployed on Kubernetes or ECS. The workers are put in the docker container and the orchestrator handle scaling and deployment. 

![AsyncWorkPool.png](..%2Fimages%2FSystemDesign%2FFundamental%2FAsyncWorkPool.png)

<figure>
  <figcaption><b>AsyncWorkPool</b> — Async task queue with worker pool: web server enqueues jobs, workers process independently.</figcaption>
</figure>
Web server create a job id in the db and the status 'pending' and push to queue the job id (not full data as it will be more than 1Mb) - workers pull and get the detail from db - worker update the job as 'processing' - worker store the result in S3 for file or db and update the status as 'completed'.

> Dont overcomplicate by adding the server less part pick kafka and show that the main is to separation of the concern. 
> 
> Dont go to the debate of the queues and merits.

### When to use.
Dont wait for the interviewer to mention long running task. Recognize the place where it will take more time and put async pattern.

Common cases - Any slow processing.

When any term like -  "video transcoding", "image processing", "PDF generation", "sending bulk emails", or "data exports" then its a hint.
The process takes several minutes so return the job id and process it.

When the math not support - When the system mention "process 1 million images per day" and image processing takes 10 sec so do the math loud - single day 85k sec and 1M (1M/85k = 11.6) 12 message per sec. Each job takes 10 sec meaning in one sec you have to do 120 sec of work. The dedicated worker will scale on compute optimized hardware.


When different operation need different hardware - Say the work include API request and GPU heavy - We should not run GPU workload on the server that handle login then separate the async work on GPU instance.

When they ask about scale or failure - The cases like "when the server crash? Scale the system to 10x" 

### Common cases.

**What will happen when the worker crash?**

The job will be taken by another worker.   
The heartbeat will tell in case the worker alive the interval is a deciding factor. When one is not responding the other worker will pick up the task based on the offset. There is an option to set something like session timeout.

**What will happen when the job keeps failing?**

Data error or any doomed job that will keep retry and stop the worker and the other message in the queue.
Solution - DLQ when the job fails say 3 times message will be in DLQ. The DLQ will store the data that need human debug. When done the jobs are back to the queue.

**User click on the generate report for 3 times how to prevent the identical jobs in the queue?**

Without deduplication the resource waste doing identical jobs. Worst case send the email 3 times and charging the money. The issue in the task which are not idempotent.

The solution is idempotency key. When any job arrive make a key that represents the operation. Any user initiated action combine the key (user id+action+timestamp) and system generated jobs user the id based on the input data. Before taking the job see if that task exists like db or cache in case it does then return the job id exists and not the new job.

```java
public String submitJob(String userId, String jobType, String jobData, String idempotencyKey) {
        // Check if job already exists
        Job existingJob = db.getJobByKey(idempotencyKey);
        if (existingJob != null) {
            return existingJob.getId();
        }

        // Create new job
        String jobId = createJob(userId, jobType, jobData);
        db.storeIdempotencyKey(idempotencyKey, jobId);
        queue.push(jobId);
        return jobId;
}

private String createJob(String userId, String jobType, String jobData) {
        // Actual job creation logic (e.g., insert into DB)
        Job job = new Job(userId, jobType, jobData);
        db.saveJob(job);
        return job.getId();
}
```

**Its sale time and huge traffic and 100X more jobs and the workers cant pick up What is the solution.**
Worker cannot process fast enough or the load is more then the jobs gets rejected.  
Solution - Backpressure meaning slow down job acceptance when the worker is overwhelmed. Set the queue depth limit and reject new job its better than keeping it wait. Autoscale the worker based on the queue depth. There is a limit and queue depth at that limit then scale up the worker. The point is its the queue depth and not the CPU usage and by the time the CPU is high the queue is backed up.


**How to separate the jobs like some report 5 mins and yearly account say 5 hours?** 

Long jobs will block the short jobs and the simple report wait for an hour. The worker utilization is not optimum some doing more task and long one job.

Solution -  

## Dealing with contentions.
Multiple users try to access the same resource like tickets and seats then there should be a way to prevent race in system. 

Solution - db level approach like pessimistic locking and optimistic locking. The solution like distributed coordination mechanism.
Pessimistic locking - lock the row until the transaction is complete.   
Optimistic locking - check the version number of the row before updating it. If the version number has changed then retry the operation.

Main part is understanding when to use the atomicity and transaction and when explicit locking.

Distributed system - distributed locks, two-phase commit protocols, or queue-based serialization.

> DB are made to solve the problem of contention. Put the data in multiple db then we are taking the challenges in our hand that the db is designed to solve. 
> 
> Think then propose the solution.

## Scaling reads.
Managing high read request through db optimization, horizontal scaling and caching.


Solution - optimize the db using indexing and denormalization, scale horizontally with read replicas and add external caching layers like Redis and CDN.


## Scaling writes.

The scaling write including the parts like sharding, batching and load management.
Sharding - distributing data across multiple servers.    
Vertical partitioning - separating different types of data.    
Handing write bursts through queue (buffer temporary spikes) and load shedding (prioritize important writes during overload).     
Batching will help reduce per-operation overhead with grouping multiple writes.

Main part select good partition key and make the load even and make the data together.

## Handling large blobs.
Large files like video, image, documents - no routing of Gbs of data through application server - make client-to-storage transfer with the URL (application server makes the URL and client upload the file to the direct storage S3) and CDN delivery (download the data from the CDN).

The server is not at the bottleneck to resume upload or progress tracking.

Main point - state synchronization between db and blob storage, handling upload failure, managing lifecycle of large files, event notification from storage service to keep the state consistent.
## Multi step processes.
Services have multiple connections and long running operations that should take care of failures, retries and external dependencies. There should be coordination.
Solution like single server or workflow. Event sourcing has a distributed approach and each step will emit events that trigger steps.  

Modern workflow like Temporal or AWS Step Function handle state management, filure recobery and retry logic.

Main point upgrade from scatter state management to workflow definition and the system will guarantee exactly once execution.
## Proximity Based Services.
Uber, Airbnb - need the search by location.  
Gespatial index helps to query and get the data based on geographical proximity. It is an extension to db like PostgreSQL with PostGIS extension or Redis geospatial data type.

The search is not global and when the user search it like local to them. It will reduce the search space.
Geospatial index when to index millions of items and when search of 1000 items then search and not the build of a index or service.
## Pattern Selection.
Start simple like polling and single db and then add the parts when needed. Make the focus in design rather than implementation.




















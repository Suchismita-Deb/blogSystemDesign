# Common Pattern - The success is all about the pattern.

## Pushing realtime update.
## Scaling Reads.
The need to scale read is in physics - CPU core can execute limited num of instruction per second, disk I/O bounded by the speed of the spinning platters or SSD write cycles. More code will not improve the case.  
The scaling read included - optimize read performance within your database using _indexing_ and _denormalization_, _scale horizontally_ with read replicas and _add external caching layers_ like Redis and CDN.

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
The problem multiplies when Requires joining say the homepage will be generated after joining with 10 tables or calling external api - more parallel calls.
Solution distributed locks to serialise rebuilds. The first request will get rebuilt while everyone else wait for the rebuild to complete.   
The issue - In case the rebuilds fails or takes longer thousands of requests timeout will happen. We need to add complex timeout handling process and fallback logic.   
Solution - Use probabilistic early refresh - Serving cached data when refreshing it in the background. It will refresh the cache before expiring say the expiry is in 60 minutes, a request at min 50 ve have 1% chance of refreshing it, at min 55 it will have 5% chance and at 59 it might be 20%.  
Not giving 100K request at minute spread the request across last 10 to 15 minutes. Most user will get the cache data while few of them will trigger refresh.

Th most critical cached data the approaches are not good. Background refresh should continuously update before expiration. The home page refresh every 50 mins so it never trigger rebuilds. The cost is complex structure and wasted refresh when it is not requested.


### Cache invalidation when data update needs to be immediately visible.

Event organiser updates the address and the attendees were not able to see that updates for another one As it's cached somewhere. The data might be cached in multiple layers like radius cdn and even browser cache. invalidating all of that would be hard.
Solution A knife approach is deleted the cash entry after a write. Sounds simple but it has problem Which cache do you delete from application cache cdn or browser? What will happen if an invalidation request fail? What if the request comes in right after you delete the old value but before adding the new venue - It will compute the cache stale data again. It is the race condition.

Solution a better approach for entry level data is cache versioning. Instead of deleting old cash We make them irrelevant by changing the cash key When the data is changed.

Each record has a version's number in the db When the record is updated the version is incremented in the same transaction.
On read - Read the version number from the small Version key cach Fall back to the DB. Construct a cash key using that version like `event:123:v12`. Read the data using that version key. On cash miss fetch From db and write it back using the same version key.
On write - Begin a dv transaction update the row increment the version column and commit then write the new data into the new version key.

The old cash is not deleted it's just become unreachable. Cdn browser cache naturally Expire still version As the version is part of the url or cache key.

No race consition as latest write overwrite new data.
No partial invalidation.
Its good under concurrency as version change are atomic in db.

The issue - There are 2 cache lookups per request - to get the version number and the actual data. Old cahe will store stale data, set the TTL to clean up the stale data.
The pattern works in single entity caches like user profiles or product details and no with computed data like searh results or feeds where invalidation is difficult.


## Managing long-running task.

## Dealing with contentions.


## Scaling reads.

## Scaling writes.


## Handling large blobs.

## Multi step processes.

## Proximity Based Services.

## Pattern Selection.
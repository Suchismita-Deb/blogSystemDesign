# Welcome to MkDocs
## Project layout
HLD - Book, Jordan, Hello Interview, Any video. 
The point is it will be topic wise. Like any topic I got anywhere I will add it in single place.

Example - Most commonly asked and pattern.
LLD - Pattern Example. 
Most asked end to end.
Multithreading.

Java Project.
Spring project Interview Guide.

Microservice Architecture.


AI ML - Core to advanced role - Backend AI Engineer.

Run the project - python -m mkdocs serve and mkdocs serve


Scan in 10 seconds

Revise in 2 minutes

Remember key points  
To deploy - push the code in github and `mkdocs gh-deploy`.

Adding image.

Inside `docs - images - SystemDesign - Image.png` and the file are in say `docs - SystemDesign - file.md` the url will be go to the main folder then image.

The image url - `![NetworkLayer](../images/SystemDesign/NetworkLayer.png)`

The tag in the base doc branch.

Making the image small - `<img src="/images/SystemDesign/NetworkLayer.png" style="width:60%;">`

https://suchismita-deb.github.io/blogSystemDesign/

Practice UML diagram to get teh class relationship of the system and no need to write to the code. Design Pattern and UML diagram.

## System Design PDF

<iframe
class="pdf-viewer"
src="./pdfs/Graph.pdf">
</iframe>

<iframe
class="pdf-viewer"
src="./pdfs/SystemDesignTips.pdf">
</iframe>

```
git config user.name
git config user.email
git config http.sslVerify false
git push origin main
```


External monitor works - Extend display (There **Settings - Display** there will be 2 screens and moving the cursor to the left or right will point to the monitor or laptop screen) - Making any app visible in monitor then open the app and then **Win+Shift+Right/LeftKey**.  
Duplicate Screen will open all in the laptop and monitor.





distinctElementOptimized
arr[] = [1, 2, 1, 3, 4, 2, 3]
[3, 4, 4, 3]

int nums[] and int k.
Find the distinct nums in k size window in the array.

```java
import java.util.HashMap;
import java.util.Map;

int[] distinctElementOptimized(int nums[], int target) {
    Map<Integer, Integer> mp = new HashMap<>();
    
    int arr[] = new int[n-k+1];
    for(int i=0;i<k;i++){
        mp.put(nums[i],mp.getOrDefault(mp.get(nums[i]),0)+1);
    }
    int pointer=0;
    int pos=1;
    arr[0] = mp.size();
    for(int i=k;i<n;i++){
        mp.put(nums[pointer], mp.get(nums[pointer])-1);
        if(mp.get(nums[pointer])==0) {
            mp.remove(nums[pointer]);
        }
        mp.put(nums[i],mp.getOrDefault(mp.get(nums[i]),0)+1);
        arr[pos++] = mp.size();
    }
    return arr;
}
```

What is concurrentHashMap.
What is the HashMap internal.
Kafka Partition ordering.
What is the equals method.

In Spring project in the properties file when set it like `spring.ai.openai.api-key=${OPENAI_API_KEY}` In the env edit configuration in the Modify Options add the `OPENAI_API_KEY="Val"` The apply and run.



## System Design Mind Map

```markmap
# System Design

## Key Technology Week 1 to 7

### Data Structure
- Stack and Queue
- Priority Queue
- B Tree and Trie

### Networking
- HTTP/HTTPs, TCP/IP, UDP
- DNS Resolution
- TCp 3 way handshake
- REST API and Status Code
- Latency and throughput

### OS Concepts
- Threads and Process
- CPU Scheduling and Context Switching
- IO Blocking and Async Operations
- Virtual Memory and Paging
- File System and Storage
### DB Concepts
- Sharding and Partitioning
- Replication Models
- Read/Write Replicas
- CAP Theorem and Trade-offs
- Consistency Models
  - Eventual Consistency
  - Strong Consistency

### Load Balancing and Scaling
- Vertical Scaling
- Horizontal Scaling
- L4 Load Balancer
- L7 Load Balancer
- DNS-Based Load Balancing
- Health Check and Failover
- Consistent Hashing

### Caching System
- Redis
- Memcache
- Eviction Policy
  - LRU
  - LFU
  - FIFO
- Write Strategy
  - Write-through
  - Write-back

### Event and Messaging
- Message Brokers
  - Kafka
  - RabbitMQ
- Sync and Async Communication
- Pub/Sub Models
- Delivery Guarantees and Ordering
- Dead Letter Queue and Retries

## Design Architecture
### Design Thinking
- Requirement
- Bottleneck Identification
- Data and Control Flow
- Stateless and Stateful
  - Between two requests, if we need to maintain a session, then it is stateful.
- Failure Handling
### Design Pattern
- Rate Limiting
  - Token Bucket
  - Leaky Bucket
  - Sliding Window
- Circuit Breaker and Retry Logic
- Leader Election
- Health Checks and Heartbeats
- CQRS and Event Sourcing
### Design Architecture
- Monolithic
- Microservices
- Event-Driven
- Layered Architecture
- Client-Server
- Service-Oriented
## Practice
### Practice Pattern Question.
- File Storage and Sharing
  - Drive
  - Github
  - Pinterest
  - OneDrive
  - Photos
- Social Media and Networking
  - Facebook
  - Instagram Reels
  - Twitter Feed
    - Newsfeed generation
    - Recommendation system
  - LinkedIn
  - Reddit
  - Snapchat
- Likes Count
  - Instagram Reel like and comment
- Messaging and Communication
  - WhatsApp
    - Real-time messaging
    - Group chat
    - Video streaming
  - Telegram
  - Slack
  - Zoom
  - Teams
- Search and Discovery
  - Google Search
  - Amazon Product Search
    - Indexing
    - Ranking algorithm
    - Query optimization
    - Caching
- Ride Sharing
  - Uber
  - Swiggy
  - DoorDash
  - Google Maps
    - Real-time location tracking
    - Dispatch algorithm
    - Route optimization
- Content Delivery
  - Netflix
  - Disney Hotstar
  - Spotify
  - YouTube Live
    - Video encoding
    - Live streaming latency
- Ecommerce
  - Amazon
  - eBay
  - Airbnb
- Payment System
  - PayPal
  - Cash App
- Collaboration
  - Google Docs
  - Jira
  - Notion
- Content Publishing
  - Medium
  - Quora
- Healthcare and Fitness
  - Practo
  - Fitbit
- Gaming and High Traffic
  - PUBG
  - Discord
- Ad Tech and Recommendation
  - Google Ads
  - YouTube
```

# 🎓 System Design Flashcards

Study system design concepts like a pro! Click to flip cards. 

---

## 📚 CATEGORY 1: DELIVERY FRAMEWORK & MENTAL MODEL

---

!!! question "Card 1️⃣: Delivery Framework Phases"

    **FRONT:**
    
    What are the 7 phases of the System Design Delivery Framework?

    **---FLIP---**
    
    **BACK:**
    
    1. **Requirements** (5 min) - Functional & Non-Functional
    2. **Capacity Estimation** (3 min) - Fermi estimation
    3. **Entities** (3 min) - Core data models
    4. **API System Interface** (5 min) - Endpoints
    5. **Data Flow** (5 min) - Processing pipeline
    6. **High Level Design** (15 min) - Component interactions
    7. **Deep Dives** - Bottlenecks & NFRs

---

!!! question "Card 2️⃣: Mental Model 4 Pillars"

    **FRONT:**
    
    What are the 4 pillars of System Design Mental Model?

    **---FLIP---**
    
    **BACK:**
    
    🎯 **Problem Navigation** - Identify core problems early
    
    🔧 **Solution Design** - Build elegant solutions
    
    🔬 **Technical Excellence** - Know patterns & technologies
    
    💬 **Communication** - Be open to feedback

---

!!! question "Card 3️⃣: The Golden Rule"

    **FRONT:**
    
    What should you prioritize in system design?

    **---FLIP---**
    
    **BACK:**
    
    ✅ Learn the **most important thing** and connect with **real systems**
    
    ❌ Avoid: Too shallow AND Too deep (over-engineering)
    
    💡 Break ambiguous problems into small infrastructure pieces

---

## 📊 CATEGORY 2: REQUIREMENTS & FRAMEWORK

---

!!! question "Card 4️⃣: FCC SLEDS Framework"

    **FRONT:**
    
    What does FCC SLEDS stand for?

    **---FLIP---**
    
    **BACK:**
    
    NFRs Memory Aid (Furry Cat Climbs Steep Ledges):
    
    - **F**ault Tolerance
    - **C**ompliance
    - **C**AP Theorem
    - **S**calability
    - **L**atency
    - **E**nvironment Constraints
    - **D**urability
    - **S**ecurity

---

!!! question "Card 5️⃣: 3W Framework"

    **FRONT:**
    
    What is the 3W Framework for functional requirements?

    **---FLIP---**
    
    **BACK:**
    
    - **Who (Producer)** → Who sends data?
    - **What (Request)** → What data/contents?
    - **Where (Outcome)** → Desired output/event?

---

!!! question "Card 6️⃣: Specific vs Generic NFRs"

    **FRONT:**
    
    Why is "The system should be low latency" a bad NFR?

    **---FLIP---**
    
    **BACK:**
    
    ❌ **Too generic** - Every system needs low latency
    
    ✅ **Better:** "Low latency search <500ms" 
    
    This identifies the specific part and provides a measurable target.

---

## 🔢 CATEGORY 3: NUMBERS & ESTIMATION

---

!!! question "Card 7️⃣: Storage Units"

    **FRONT:**
    
    How many bytes in 1GB?

    **---FLIP---**
    
    **BACK:**
    
    **1 Billion bytes = 1GB = 10^9 bytes**
    
    Quick conversions:
    - 1KB = 10^3 bytes
    - 1MB = 10^6 bytes
    - 1GB = 10^9 bytes
    - 1TB = 10^12 bytes

---

!!! question "Card 8️⃣: Latency Numbers"

    **FRONT:**
    
    Fill in the latency ladder:
    
    Memory: ___ ms | SSD: ___ ms | Disk: ___ ms

    **---FLIP---**
    
    **BACK:**
    
    - **Memory:** 0.25 ms
    - **SSD:** 1 ms (4x slower than memory)
    - **Disk:** 20 ms (20x slower than SSD)
    
    💡 Rule: SSDs are fast & affordable!

---

!!! question "Card 9️⃣: Real-World Scale"

    **FRONT:**
    
    Fill in the blanks:
    
    - Google searches/sec: O(___k)
    - Netflix hours/day: O(___M)
    - User data size: O(___KB)

    **---FLIP---**
    
    **BACK:**
    
    - Google: O(**100k**) req/sec
    - Netflix: O(**100M**) hours/day
    - User: O(**1KB**) per user

---

!!! question "Card 🔟: Fermi Estimation Example"

    **FRONT:**
    
    Twitter daily storage for 250M DAU.
    
    Given: 150 chars/tweet, 15 tweets/user/day
    
    Calculate: (storage/tweet) × (tweets/user) × (users/day)

    **---FLIP---**
    
    **BACK:**
    
    - 1 tweet ≈ 1.5 KB
    - 15 tweets/user × 250M users = 3.75B tweets
    - 1.5KB × 3.75B = **~5.6 TB/day**
    
    💡 Use dimensional analysis for breakdown!

---

## 🗄️ CATEGORY 4: DATA MODELING

---

!!! question "Card 1️⃣1️⃣: Normalization vs Denormalization"

    **FRONT:**
    
    When should you use **denormalization**?

    **---FLIP---**
    
    **BACK:**
    
    ✅ When:
    - System is **read-heavy**
    - Data changes **infrequently**
    - Need to **avoid expensive joins**
    
    ⚠️ Trade-off: Faster reads, slower/complex writes
    
    💡 Default: Start normalized, denormalize if needed

---

!!! question "Card 1️⃣2️⃣: NoSQL Design Requirement"

    **FRONT:**
    
    What must you know BEFORE designing NoSQL schema?

    **---FLIP---**
    
    **BACK:**
    
    **Access Patterns Upfront!**
    
    DynamoDB needs:
    - **Partition Key** - Single lookups
    - **Sort Key** - Range queries
    
    ⚠️ Wrong pattern = must scan entire table!

---

## 💾 CATEGORY 5: CACHING

---

!!! question "Card 1️⃣3️⃣: Cache-Aside Pattern"

    **FRONT:**
    
    Describe the cache-aside (lazy loading) pattern.

    **---FLIP---**
    
    **BACK:**
    
    1. Check cache
    2. Cache hit → return
    3. Cache miss → fetch DB
    4. Store in cache with TTL
    5. Return to client
    
    ✅ Good for: Read-heavy
    ⚠️ Challenge: Invalidation

---

!!! question "Card 1️⃣4️⃣: Cache Stampede"

    **FRONT:**
    
    What is "cache stampede"? How to prevent?

    **---FLIP---**
    
    **BACK:**
    
    **Problem:** Redis down → All requests hit DB → DB overwhelmed
    
    **Solutions:**
    - Small in-process cache fallback
    - Circuit breaker pattern
    - Cache replication

---

!!! question "Card 1️⃣5️⃣: When NOT to Cache"

    **FRONT:**
    
    When is caching NOT beneficial?

    **---FLIP---**
    
    **BACK:**
    
    When data **updates every write**:
    
    ❌ Cache invalidation complexity
    ❌ Stale immediately
    ❌ More latency than benefit
    
    💡 Cache works for stable or infrequent writes

---

## 📈 CATEGORY 6: SHARDING & SCALING

---

!!! question "Card 1️⃣6️⃣: Hash vs Range Sharding"

    **FRONT:**
    
    Compare hash-based vs range-based sharding.

    **---FLIP---**
    
    **BACK:**
    
    **Hash-Based:**
    - hash(key) % num_shards
    - Evenly distributed
    - No hot spots
    
    **Range-Based:**
    - E.g., IDs 1-10M in shard1
    - Good for multi-tenant patterns
    - Risk: Hot spots

---

!!! question "Card 1️⃣7️⃣: When to Shard"

    **FRONT:**
    
    At what data size should you shard?

    **---FLIP---**
    
    **BACK:**
    
    ✅ Wait until **10-100 TB** of data
    
    ❌ DO NOT shard at **500GB**
    
    📌 Example: Yelp (10M businesses × 1KB × 10 reviews = 100GB) → Single DB sufficient!
    
    💡 Evaluate actual hardware capabilities first

---

!!! question "Card 1️⃣8️⃣: Consistent Hashing"

    **FRONT:**
    
    Why is consistent hashing better than modulo?

    **---FLIP---**
    
    **BACK:**
    
    **Modulo:** Add 1 server to 10 cluster → **90% data moves**
    
    **Consistent (ring):** Add 1 server → **only 10% data moves**
    
    💡 Minimizes data movement during scaling

---

## 🌐 CATEGORY 7: NETWORKING

---

!!! question "Card 1️⃣9️⃣: SSE vs WebSockets"

    **FRONT:**
    
    When to use SSE vs WebSockets?

    **---FLIP---**
    
    **BACK:**
    
    | | SSE | WebSocket |
    |---|---|---|
    | Direction | Uni (server→client) | Bi |
    | Use | Live scores, alerts | Chat, collaboration |
    | Client sends data | ❌ No | ✅ Yes |
    
    ⚠️ Don't over-engineer with WebSockets!

---

!!! question "Card 2️⃣0️⃣: REST vs gRPC"

    **FRONT:**
    
    Why use gRPC for internal services?

    **---FLIP---**
    
    **BACK:**
    
    ✅ **gRPC:**
    - Binary serialization (faster JSON)
    - HTTP/2 (multiplexing)
    - Lower latency
    
    ⚠️ **Browsers don't support gRPC**
    
    💡 Rule: REST external, gRPC internal

---

## 🤝 CATEGORY 8: CONSISTENCY

---

!!! question "Card 2️⃣1️⃣: CAP Theorem"

    **FRONT:**
    
    Which property always exists in CAP?

    **---FLIP---**
    
    **BACK:**
    
    **Partition (P) always exists!**
    
    Real trade-off is:
    - **C**onsistency - All nodes same data
    - **A**vailability - Always responsive
    
    💡 Most systems: Availability > Consistency (unless money/inventory)

---

!!! question "Card 2️⃣2️⃣: PACELC Theorem"

    **FRONT:**
    
    What does PACELC add to CAP?

    **---FLIP---**
    
    **BACK:**
    
    **PAC:** In partition, pick A or C
    
    **ELC:** **Else** (no partition), trade Latency vs Consistency
    
    💡 Strong consistency adds latency even with good network!

---

## 🔍 CATEGORY 9: TECHNOLOGIES

---

!!! question "Card 2️⃣3️⃣: Inverted Index"

    **FRONT:**
    
    What is inverted index? How does it speed search?

    **---FLIP---**
    
    **BACK:**
    
    Maps: **words → documents**
    
    ❌ Bad: `WHERE text LIKE '%word%'` → Full scan
    
    ✅ Good: Inverted index lookup → O(1)
    
    ```
    {
      "word1": [doc1, doc2],
      "word2": [doc3, doc4]
    }
    ```

---

!!! question "Card 2️⃣4️⃣: Blob Storage"

    **FRONT:**
    
    Why NOT store media in primary DB?

    **---FLIP---**
    
    **BACK:**
    
    ❌ Expensive, slow queries, complex backups
    
    ✅ Use Blob Storage (S3):
    - Store media → get URL
    - Store URL in DB
    - Serve via CDN
    
    📌 Example: YouTube, Instagram, Dropbox

---

!!! question "Card 2️⃣5️⃣: Distributed Locks"

    **FRONT:**
    
    Name 3 scenarios needing distributed locks.

    **---FLIP---**
    
    **BACK:**
    
    1. **Ticketmaster** - Lock tickets 10min during checkout
    2. **Uber** - Lock drivers when ride requested
    3. **Cron jobs** - Run on ONE server only
    4. **Auctions** - Lock final seconds

---

## 🏗️ CATEGORY 10: ARCHITECTURE

---

!!! question "Card 2️⃣6️⃣: Load Balancer L4 vs L7"

    **FRONT:**
    
    When use L4 vs L7 load balancer?

    **---FLIP---**
    
    **BACK:**
    
    **L4 (TCP):**
    - Fast, simple
    - Use for: WebSockets
    
    **L7 (HTTP):**
    - Flexible routing by content
    - Default for most cases
    
    💡 Use L7 unless need persistent TCP!

---

!!! question "Card 2️⃣7️⃣: Message Queue Decision"

    **FRONT:**
    
    When should you add message queue?

    **---FLIP---**
    
    **BACK:**
    
    ✅ **Add when:**
    - Writes > 20k/sec
    - Need guaranteed delivery
    - Event sourcing pattern
    - Bursting load
    
    ❌ **Don't add for sync < 500ms latency**
    
    💡 PostgreSQL handles 10-20k writes/sec alone!

---

!!! question "Card 2️⃣8️⃣: API Gateway Roles"

    **FRONT:**
    
    What does API Gateway do?

    **---FLIP---**
    
    **BACK:**
    
    ✅ **Routing** - to backend services
    ✅ **Authentication** - JWT, API keys
    ✅ **Rate limiting** - abuse prevention
    ✅ **Logging** - track all calls
    ✅ **Cross-cutting concerns** - centralized

---

## ⚠️ CATEGORY 11: COMMON PITFALLS

---

!!! question "Card 2️⃣9️⃣: Premature Sharding"

    **FRONT:**
    
    Give example of unnecessary sharding.

    **---FLIP---**
    
    **BACK:**
    
    **Yelp Example:**
    - 10M businesses × 1KB = 10GB
    - 10GB × 10 reviews = 100GB total
    - ✅ Single DB enough!
    
    ❌ Don't shard at 500GB = massive complexity for nothing!

---

!!! question "Card 3️⃣0️⃣: Overestimating Latency"

    **FRONT:**
    
    Why cache isn't always needed?

    **---FLIP---**
    
    **BACK:**
    
    People overestimate SSD slowness:
    
    ✅ SSD indexed lookups: **millisecond range**
    
    ❌ Cache adds: complexity, invalidation, staleness
    
    💡 Only cache if very strict latency OR expensive query!

---

## 📊 CATEGORY 12: SCALING THRESHOLDS

---

!!! question "Card 3️⃣1️⃣: Cache Scaling Triggers"

    **FRONT:**
    
    When scale cache tier?

    **---FLIP---**
    
    **BACK:**
    
    Scale when:
    - Hit rate < **80%**
    - Latency > **1ms**
    - Memory > **80%**
    
    💡 Modern Redis: 100k+ ops/sec, TB-scale, <10ms latency

---

!!! question "Card 3️⃣2️⃣: DB Scaling Triggers"

    **FRONT:**
    
    When scale (shard) database?

    **---FLIP---**
    
    **BACK:**
    
    Scale when:
    - Data → **10-100 TB** (not 500GB!)
    - Writes > **10k TPS** consistently
    - Need **geo-distribution**
    
    💡 Single PostgreSQL: TBs data, 10k writes/sec, ms response!

---

## ✅ TRUE/FALSE

---

!!! question "Card 3️⃣3️⃣: Denormalization First?"

    **FRONT:**
    
    TRUE or FALSE:
    
    "Start with denormalization in DB design"

    **---FLIP---**
    
    **BACK:**
    
    **FALSE**
    
    ✅ Default: **Normalization**
    
    ❌ Only denormalize if read performance bad!

---

!!! question "Card 3️⃣4️⃣: Always WebSockets?"

    **FRONT:**
    
    TRUE or FALSE:
    
    "Use WebSockets for all real-time"

    **---FLIP---**
    
    **BACK:**
    
    **FALSE**
    
    ✅ Use SSE for one-way (simpler, cheaper)
    
    ✅ Use WebSockets for two-way (chat, collaboration)
    
    ⚠️ WebSockets are complex to scale!

---

!!! question "Card 3️⃣5️⃣: Redis Performance"

    **FRONT:**
    
    TRUE or FALSE:
    
    "Redis can handle 100k+ ops/sec"

    **---FLIP---**
    
    **BACK:**
    
    **TRUE ✅**
    
    Modern Redis: > 100k ops/sec per instance!
    
    💡 Usually network/CPU bottlenecks before throughput

---

## 🎯 CATEGORY 13: SCENARIOS

---

!!! question "Card 3️⃣6️⃣: Twitter Feed Bottleneck"

    **FRONT:**
    
    Twitter feed 100M DAU, 500ms latency.
    
    Which NOT likely bottleneck?
    
    A) DB joins for feed
    B) App processing
    C) User table (2GB)
    D) Geography

    **---FLIP---**
    
    **BACK:**
    
    **Answer: C)**
    
    2GB user table is **tiny** for modern hardware!
    
    Real bottlenecks: Complex joins (A), heavy logic (B), geography (D)
    
    💡 Don't get stuck on small details!

---

!!! question "Card 3️⃣7️⃣: Tech Choice: Complex Queries"

    **FRONT:**
    
    Need complex queries:
    
    "Find users created last 7 days AND from SF"
    
    Pick: A) NoSQL | B) SQL

    **---FLIP---**
    
    **BACK:**
    
    **Answer: B) SQL (PostgreSQL)**
    
    ✅ NoSQL needs access patterns upfront
    ✅ Complex queries need SQL indexing
    ✅ Foreign keys for consistency
    
    💡 SQL for flexible queries, NoSQL for known patterns

---

!!! question "Card 3️⃣8️⃣: Add Queue Now?"

    **FRONT:**
    
    Current: 5k writes/sec PostgreSQL
    Growth: 20% month-over-month
    
    Add message queue now?
    
    A) Yes | B) No | C) Always

    **---FLIP---**
    
    **BACK:**
    
    **Answer: B) No**
    
    - PostgreSQL limit: **20k writes/sec**
    - Current: 5k (plenty headroom)
    - Time to limit: ~7 months
    
    💡 Avoid premature optimization!

---

!!! question "Card 3️⃣9️⃣️: Consistency vs Eventual"

    **FRONT:**
    
    E-commerce inventory count.
    
    Strong consistency or eventual?

    **---FLIP---**
    
    **BACK:**
    
    **Strong Consistency!**
    
    ✅ Money/inventory = **always strong**
    ✅ Overselling = revenue loss
    
    ❌ Eventual OK for: recommendations, metrics, social
    
    💡 Rule: Money → Strong | Everything → Eventual

---

!!! question "Card 4️⃣0️⃣: Optimize Search"

    **FRONT:**
    
    Search 1B docs takes 5 sec.
    
    Biggest impact?
    
    A) Redis cache
    B) ElasticSearch
    C) More servers
    D) Code 10%

    **---FLIP---**
    
    **BACK:**
    
    **Answer: B) ElasticSearch**
    
    ✅ Inverted index: massive improvement
    ❌ Cache: only helps repeats
    ❌ Servers: doesn't help slow query
    ❌ Code: marginal gain
    
    💡 Architecture > Resources > Code

---

## 🎓 FINAL SCORES

---

**Your Flashcard Deck:**

- 📚 **40 questions** covering all core concepts
- ⏱️ **5-10 min** to study fully
- 🎯 **Interview-ready** material
- 💡 **Real-world scenarios** included

**Study Tips:**

✅ Focus on **WHY**, not just answers
✅ Practice **Fermi estimation**
✅ Build real systems
✅ Know the **trade-offs**

---

**Last Updated:** June 2026  
**Level:** Intermediate → Advanced 🚀



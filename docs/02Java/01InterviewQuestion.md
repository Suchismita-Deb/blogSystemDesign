## Java Memory Management and Performance Tuning.

### What are the different memory present in Java.
Java memory is managed by JVM and it has heap for objects and stack for each thread methods and calls for local variable. Method area or meta space for class metadata and static variable. PC (Program Counter) register for the current instructions per thread and native method stack for JNI calls.  
**What is the difference between heap and stack memory ?**  
Heap memory is shared across all the threads and it is used for objects and class instances. Stack memory is per thread and it is used for method calls and local variables.    
Stack memory is faster than heap memory and it is automatically managed by the JVM. Heap memory is managed by the garbage collector and it can be tuned by the JVM options.    Stack memory is limited and can cause stack overflow if the recursion is too deep or the method calls are too many. Heap memory can cause out of memory error if the objects are too many or too large.

### The production of freezes intermittently under load how would you determine whether it's a deadlock a long GC pause or a throat starvation ?
Take a thread dump first. The deadlock show up as a clear cyclic logs. Verify the GCS logs for long post in case threads are in long pause.  
If there's a real life but just waiting on the pool then that's thread starvation. The notice part is thread poll soze and queue length.


### A counter incremented by multiple threads inside a synchronized method is still producing wrong totals what are the possible causes ?
The solution involves for us to save the whole method is entirely synchronized on just part of it.  
The next thing to see if multiple objects are being used as locks instead of one shared locsk.  
In case the counter is itself has a non atomic Read modify write happening outside the synchronized block 

### How can a application leak memory even though the JVM has garbage collection, give an example.
Elementary difference happens when object was still getting referenced somewhere so GC Will not be able to collect them even the app is not using it anywhere. Example - static collections keep on growing.

### How to default methods let an interface evolve without breaking existing implementing classes ?
Adding a method into the interface everyone has to implement it. 
Default method let up add a method wth a body directly in the interface.
The entire implementing classes compiled and worked without being forced override.   

### How do you monitor application connections and identify failing connections ?
We should use Actuator health endpoint metric for connection pool stat and monitoring tools like Prometheues, Grafana for dashboard and alerts. 
Application and pool logs also shows the failed or the timeout connections.
There should be some sort of alerts and pool usage and failed connection.

### How would you handle the duplicate messages sent by the Kafka producer ?
The first thing to enable idempotent key setting for the retries dont create duplicates and the consumer side make it idempotent.

### How would you troubleshoot Kafka consumer that is unable to consume messages?
First see the consumer group status and the lag and see if the consumer is part of the group.

In case stuck in rebalancing then check the network connectivity to the broker and the topic. 


### How would you handle a Kafka consumer can that continuously fails while processing messages ?</b>

We should use retry with backup for transient error. In case it is failing then send the letter to the dead letter review topic with the proper error message to debug and understand the issue and alert set up when message going to the dead letter queue topic.

### What is Springwood Actuator and how would you use it to monitor a live app's health and connections ?
The actuator exposes production ready endpoints like health, metrics, info, env, beans, mappings, threaddump, loggers, httptrace and many more. We can see database and connection pool health, memory usage and custom health indicators hooked up to the Prometheus.


### How does Spring Boot auto configuration work internally like the @ConditionalOnClass and @ConditionalOnMissingBean?</b>

Spring Boot scan auto configuration classes listed on the auto configurations.import file. Each class activates when the condition matches.
Example @ConditionalOnClass in case a dependency is on the class path or @ConditionalOnMissingBean in case the bean is not defined.

### The Springboot service was throwing connection-po0l-exhausted errors in production so how would you solve this pool issue ?
The first step to see the active versus the idle connection in the pool matrix. The names of the connections that are not released like long running query. The pool size ad the load and the slow query logs.

### Any functional differences present between @Components, @Service and @Repository or its purely semantic ?
### A teammate wants no sequel for scalability for a service that needs a strong consistency and joins how do you make the call ?
### A legacy partner only supports SOAP but the team standard is REST how do you avoid duplicating logics ?</b>

### How the HashMap works internally?

Internally, a HashMap is backed by an array — `Node<K,V>[]` table. Each slot in that array is called a **bucket**. When you insert a key-value pair, we don't scan the whole array; we compute an index and drop the entry into that bucket.

Each Node holds four things: **hash**, **key**, **value**, and a **next pointer**. That next pointer is what makes each bucket effectively a singly linked list — that's how we handle multiple keys landing in the same bucket, which is collision.

**How put() decides the bucket**

Two steps: compute the hash, then compute the index.

For hash, Java 8 doesn't just use `key.hashCode()` directly. It runs it through a spreading function -

```java
static final int hash(Object key) {
int h;
return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
We XOR the hash with itself shifted right 16 bits. The reason - `hashCode()` gives you a 32-bit int, but our table size is usually small — say 16 or 32 buckets — so only the low bits actually get used when we compute the index. If two keys differ only in their high bits, they'd collide constantly without this step. XOR-ing the upper 16 bits into the lower 16 spreads that entropy down where it matters.

Then for the index, instead of doing hash % capacity, Java does - `index = (n - 1) & hash`

where n is the table length. This only works correctly because table capacity is always a power of 2 — that's a deliberate design choice, because `(n-1) & hash` is a much cheaper bitwise operation than modulo, and it's mathematically equivalent to `hash % n` when n is a power of 2.

**Collision handling**

When two keys hash to the same bucket, we first compare stored hash to the new hash — cheap integer comparison. If hashes match, then we call `.equals()` on the keys to confirm they're actually the same key, not just a hash collision. That's exactly why the hashCode/equals contract matters: if two objects are equal, they must return the same hashCode, otherwise your map will store duplicates and lookups will randomly fail. The reverse isn't required — different objects can share a hashCode, that's a normal collision, and `equals()` is the tiebreaker.

If the key already exists (hash matches and equals returns true), we overwrite the value. Otherwise, we append a new node to the end of that bucket's linked list.

**Resizing**

We track a load factor, default 0.75. Threshold = capacity × loadFactor. Once size crosses that threshold, we double the capacity and rehash. Java 8 optimized this rehash step — instead of recalculating every node's bucket index from scratch, it uses the fact that capacity is always doubling in powers of 2. Each old bucket's nodes split into exactly two possible new buckets — "low" (same index) or "high" (old index + old capacity) — decided by checking a single bit: hash & oldCap. So it's a cheap split instead of a full rehash.

Default size of the array 16 and the load factor 0.75

threshold = capacity × loadFactor = 15 × 0.75 = 11
Concrete example — Imagine capacity stays fixed at 15 buckets, but you keep inserting 1000 entries into it. On average, each bucket now holds 1000 / 15 ≈ 60 entries chained together in a linked list.

Now think about `get()` - to find one key, you compute its bucket — fine, O(1) — but then you have to walk that bucket's linked list comparing hash and equals one node at a time until you find the match. With 62 entries per bucket, that's no longer O(1) lookup, it's closer to O(n) — you've basically degraded HashMap into a linked list with extra steps.

So resizing is what keeps the average bucket size low — roughly load factor's worth of entries per bucket — so lookups stay close to O(1).


Java 8's real headline change — treeification

This is usually the part interviewers are fishing for. Before Java 8, if a bucket had a bad collision storm — say due to a poor or malicious hashCode implementation — that bucket's linked list could grow long, and lookup degraded to O(n) in the worst case.

Java 8 introduced red-black trees for buckets. If a single bucket's chain length exceeds TREEIFY_THRESHOLD (8), and the overall table capacity is at least MIN_TREEIFY_CAPACITY (64), that bucket converts from a linked list to a balanced red-black tree (TreeNode, which extends LinkedHashMap.Entry). That brings worst-case lookup in that bucket down from O(n) to O(log n).

Note the capacity check — if the table's still small (< 64), Java prefers to just resize the table rather than treeify, since a small table with one long bucket usually just means it needs more buckets overall, not tree overhead.

There's also an UNTREEIFY_THRESHOLD of 6 — if enough entries get removed and a tree bucket shrinks back down, it converts back to a linked list, since trees have more memory and maintenance overhead than they're worth for small counts.

get() lookup

Compute hash, jump to bucket via (n-1) & hash, then either walk the linked list or traverse the tree, comparing hash first then equals, until we find the match or hit the end — returning null if not found.

A couple of side notes.

HashMap allows exactly one null key, which always hashes to bucket 0. It's not thread-safe — no synchronization — so under concurrent writes you can get infinite loops in the old Java 7 chaining resize (that's actually a classic pre-Java-8 production bug), which is part of why Java 8 rewrote resize logic, and why we reach for ConcurrentHashMap in multithreaded contexts instead of Collections.synchronizedMap.

### What happens if we do not override the equals() method and the hashCode() method in Java?
equals() Method - <br>
By default, the equals() method in Java, inherited from Object, checks for reference equality (whether two objects point to the same memory location).
If not overridden, even if two objects have identical properties, they will not be considered equal unless they refer to the same memory location.

hashCode() Method - <br>
By default, the hashCode() method generates a hash code based on the memory address of the object.
If not overridden, objects with identical properties may generate different hash codes, affecting their behavior in hash-based collections (like HashSet, HashMap)

```java
class Person {
String name;
int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class Main {
public static void main(String[] args) {
Person p1 = new Person("Alice", 30);
Person p2 = new Person("Alice", 30);

        System.out.println("p1.equals(p2): " + p1.equals(p2)); // Reference equality

        HashSet<Person> set = new HashSet<>();
        set.add(p1);
        set.add(p2);

        System.out.println("Set size: " + set.size()); // Unexpected behavior
    }
}
```

The output.
```java
p1.equals(p2): false
Set size: 2
```

Since equals() is not overridden, the two Person objects are not considered equal, even though they have identical properties.  
The hashCode() method is also not overridden, so the two objects have different hash codes, leading to both being stored in the HashSet

Only doing the equal method.
```java
import java.util.HashSet;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && name.equals(person.name);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Alice", 30);
        Person p2 = new Person("Alice", 30);

        HashSet<Person> set = new HashSet<>();
        set.add(p1);
        set.add(p2);

        System.out.println("Set size: " + set.size()); // Still unexpected behavior
    }
}
```
Set size is 2.

Even though equals() is overridden, the default hashCode() still generates different hash codes for p1 and p2. Therefore, both objects are stored in the HashSet.

Overridding both the equal and hashCode().
```java
import java.util.HashSet;
import java.util.Objects;

class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && name.equals(person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("Alice", 30);
        Person p2 = new Person("Alice", 30);

        HashSet<Person> set = new HashSet<>();
        set.add(p1);
        set.add(p2);

        System.out.println("Set size: " + set.size()); // Correct behavior
    }
}
```
When two object are equal() then the hashCode() should also be equal. When two hashCode is equal then the object may not be equal.

### Difference between HashMap and Hashtable.
| Feature | HashMap | Hashtable |
|---------|---------|-----------|
| **Thread Safety** | Not thread-safe. Requires external synchronization (e.g., `Collections.synchronizedMap()`) in multithreaded environments. | Thread-safe because synchronization is built into its methods. |
| **Null Key / Values** | Allows one `null` key and multiple `null` values. | Does not allow `null` keys or `null` values; throws `NullPointerException`. |
| **Performance** | Usually faster due to no synchronization overhead. | Usually slower because synchronization adds overhead. |
| **Package / History** | Introduced in Java 1.2 as part of the Java Collections Framework (`java.util`). Preferred in modern code. | Legacy class from Java 1.0 in `java.util`. Generally not recommended in new code. |
| **Usage** | Preferred in single-threaded code or when synchronization is handled externally. | Rarely used in modern applications; `ConcurrentHashMap` is usually preferred for thread-safe access. |

### Difference between HashSet and TreeSet.
| Feature | HashSet | TreeSet |
|---------|---------|---------|
| **Performance** | Faster (O(1)) | Faster (O(log n)) |
| **Order** | Unordered. No guarantee of order; depends on the hash function. | Sorted in natural order or custom order. Depends on `Comparable` or a custom `Comparator`. |
| **Iteration** | Iterates in no specific order. | Iterates in ascending sorted order, or as defined by a custom comparator. |
| **Null Values** | Allows one `null` value. | Does not allow `null`. |
| **Internal Data Structure** | Uses `HashMap` internally for storage. | Uses a Red-Black Tree (self-balancing binary search tree). |
| **Sorting** | Custom sorting not supported. | Custom sorting supported via `Comparator`. |

### Difference between HashMap and TreeMap.
| Feature | HashMap | TreeMap |
|---------|---------|---------|
| **Performance** | O(1) average time complexity for basic operations. | O(log n) time complexity for basic operations. |
| **Order** | No fixed order. | Maintains keys in sorted order, either natural ordering or via a custom comparator. |
| **Null Key / Values** | Allows one `null` key and multiple `null` values. | Does not allow `null` keys, but allows multiple `null` values. |
| **Internal Data Structure** | Hash table. | Red-Black Tree (self-balancing binary search tree). |

### Difference between ConcurrentHashMap and SynchronizedHashMap.
| Feature | ConcurrentHashMap | SynchronizedHashMap |
|---------|-------------------|---------------------|
| **Synchronization Mechanism** | Uses segment-based locking (Java 7) or bucket-level locking (Java 8+). | Entire map is locked for each operation using synchronized blocks. |
| **Concurrency** | Allows concurrent reads and writes by multiple threads; only writes to the same bucket are blocked. | Allows only one thread to access the map at a time. |
| **Performance** | Higher performance in multithreaded environments due to finer-grained locking. | Lower performance due to coarse-grained locking (locks the entire map). |
| **Null Values** | Does not allow `null` keys or values. | Allows a single `null` key and multiple `null` values. |
| **Thread Safety** | Thread-safe for concurrent access with better scalability. | Thread-safe, but less efficient in high-concurrency scenarios. |
| **Locking Granularity** | Fine-grained locks improve throughput by reducing contention. | Coarse-grained locks block all threads accessing the map, even for independent operations. |
| **Iteration Behavior** | Does not throw `ConcurrentModificationException` during iteration; reflects changes made by other threads. | Throws `ConcurrentModificationException` if the map is modified during iteration. |
| **Use Case** | Best suited for high-concurrency applications where reads and updates are frequent. | Suitable for low-concurrency scenarios where simplicity is preferred over performance. |

**ConcurrentHashMap**.

The ConcurrentHashMap class is part of the Java Collections Framework and extends the AbstractMap class. It implements the ConcurrentMap and Serializable interfaces. Below is the hierarchy:
```
java.lang.Object
   └── java.util.AbstractMap<K, V>
     └── java.util.concurrent.ConcurrentHashMap<K, V>
        ├── ConcurrentMap<K, V> (Interface)
        └── Serializable (Interface)
```
**How ConcurrentHashMap Works Internally?**

It works on mainly 3 parts.

**Segmented Locking** - The map is divided into segments (buckets) internally.
<br/>Locking occurs at the segment level rather than the whole map, ensuring high concurrency.

**CAS (Compare-And-Swap)** - Used for atomic updates without locks. Improves performance in high-concurrency scenarios.

**Read-Write Operations** - Reads are generally lock-free, allowing for high throughput. Writes use fine-grained locking or CAS to minimize blocking.

ConcurrentHashMap is not distributed data structure meaning Service A and Service B will point to different Concurrent hashmap. It gives thread safety within the JVM or process. IN distributed system it is Redis or Db.

```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
// Thread 1 - User123
// Thread 2 - User123
// Thread 3 - User123
// The value of User123 will be 3. In HashMap There will be race condition.

if(!mp.containsKey("User123")) {
    mp.put("User123", 1);
} else {
    mp.put("User123", mp.get("User123") + 1);   
}
// Initial value = 0 
// Thread1.get() = 0
// Thread2.get() = 0
// Thread1.put() = 1
// Thread2.put() = 1
// Expected value 2.

// ConcurrentHashMap we use the Atomic operation
map.merge("User123", 1, Integer::sum); // User123 - 3.
```


**Explain CAS**

Compare-And-Swap (CAS) is an atomic operation used in concurrent programming to achieve synchronization without locks. It enables threads to update shared variables safely without the overhead and contention caused by traditional locking mechanisms.

**Memory Location** - CAS reads the value at the memory location.

**Expected Value** - It compares the read value with the expected value.
If the current value matches the expected value, CAS updates the variable with the new value.
If the current value does not match the expected value, CAS fails, and no update occurs.

**New Value** - The operation returns a status indicating whether the swap was successful.

Example - Thread A. Memory location 5. Value = 5.   
Expected value = 5. New Value = 10. The value is updated to 10.

Example - Thread B. Memory location 5. Value = 10. Expected value = 5.   
New Value = 15. The value is not updated as current value is 10. 

```java
import java.util.concurrent.atomic.AtomicInteger;

public class CasExample {
    public static void main(String[] args) {
        AtomicInteger value = new AtomicInteger(5);

        // Thread A
        boolean successA = value.compareAndSet(5, 10);
        System.out.println("Thread A success: " + successA + ", value: " + value.get());

        // Thread B
        boolean successB = value.compareAndSet(10, 15);
        System.out.println("Thread B success: " + successB + ", value: " + value.get());
    }
}
//Thread A success: true, value: 10
//Thread B success: true, value: 15
```


**Advantages of CAS**.

Non-blocking - CAS ensures only the thread that successfully updates the variable proceeds. Other threads retry until they succeed, avoiding the need for locks.

High Performance - Eliminates contention and overhead associated with locking. Particularly useful in high-concurrency scenarios.

Atomicity - The comparison and update occur as a single, indivisible operation. Ensures thread safety.

**Disadvantages of CAS**.

ABA Problem - If a variable changes from value A to B and back to A, CAS may incorrectly assume nothing changed.
Solution: Use a version number or timestamp alongside the variable.

Busy-Waiting - If many threads are competing, repeated retries can cause performance degradation.

Limited Use - Works well for single variable updates but becomes complex for larger data structures or multiple variables.


Iteration in ConcurrentHashMap does not throw a `ConcurrentModificationException` even if the map is modified during the iteration.

```java
public static void main(String[] args) {
    ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
    map.put("A", 1);
    map.put("B", 2);
    map.put("C", 3);
    // Thread to modify the map
    new Thread(() -> map.put("D", 4)).start();
    // Iterating over the map.
    // It can modify the map while iterating.
    for (String key : map.keySet()) {
        System.out.println("Key: " + key + ", Value: " + map.get(key));
        // Simulate adding a new key during iteration
        if (key.equals("B")) {
            map.put("E", 5); // Adding during iteration
        }
    }
    System.out.println("Final Map: " + map);
}
```

Synchronized HashMap.

Iteration in SynchronizedHashMap requires explicit synchronization when accessed by multiple threads. Modifying the map during iteration will throw a ConcurrentModificationException unless you use explicit synchronization.

```java

public static void main(String[] args) {
    Map<String, Integer> map = Collections.synchronizedMap(new HashMap<>());
    map.put("A", 1);
    map.put("B", 2);
    map.put("C", 3);
    // Thread to modify the map
    new Thread(() -> {
        synchronized (map) {
            map.put("D", 4);
        }
    }).start();
    // Iterating over the map
    synchronized (map) { // Explicit synchronization required
        for (String key : map.keySet()) {
            System.out.println("Key: " + key + ", Value: " + map.get(key));
            // Attempting to modify during iteration
            if (key.equals("B")) {
                map.put("E", 5); // This may throw ConcurrentModificationException
            }
        }
    }
    System.out.println("Final Map: " + map);
}
```


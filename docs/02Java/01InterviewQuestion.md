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

>

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



### What is the difference between HashMap and HashTable in Java?
Hashmap is not synchronized meaning its faster but not threads safe and hash table is synchronized making it thread safe but slower additionally hashmap allows null key and values while hash table does not.

# Java Quiz - Stream and Kafka

### Streams
<div class="quiz-box">
<b>Stream problems - Find sum of even numbers from array.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

``` java
int num[] = {1,2,3,4,4,5,6,7,8};
int sum = Arrays.stream(nums).filter(n->n%2==0).sum();
System.out.println(sum);
// `filter` keeps only even values and `sum()` adds them. 
```
</details>

</div>

<div class="quiz-box">
<b>Count the occurrence "apple" in the list.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

```java
List<String> list = Arrays.asList("apple","banana","orange","apple","apple");
long count = list.steam().filter(word->words.equalOrIgnoreCase("apple")).count();
System.out.println(count); // 3
```
</details>
</div>

<div class="quiz-box">
<b>Given a list of list put all the elements in the same list.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

```java
List<List<String>> skills = Arrays.asList(
    Arrays.asList("java","Spring","SpringBoot"),
    Arrays.asList("React","Kafka","Microservice"),
    Arrays.asList("MVC","Design Pattern");
);
List<String> allSkills = skills.stream()
                               .flatMap(skillsSets -> skillsSet.stream())
                               .collect(Collectors.toList());
// With stream first will get one list, flatmap will combine the list in one list. `flatMap` converts nested lists into one continuous stream.
System.out.println(allSkills);
```
</div>

<div class="quiz-box">
<b>Find the skills starting with character 's'.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

```java
List<String> skillsStartsWithS = allSkills.stream().filter(s -> s.charAt(0)=='s').collect(Collectors.toList());
// s.charAt(0)=='s' and s.startsWith("S") does same thing.
System.out.println(skillsStartsWithS);
```
</details>
</div>

<div class="quiz-box">
<b>Age of an employee above 30.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

```java
List<Integer> l = Arrays.asList(1,2,3,4,5);
List<Integer> list - l.stream().filter(x->x>3).collect(Collectors.toList());
System.out.println(list);
```

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

Apply `filter(x -> x > threshold)` to keep only higher ages.

</details>

</div>

<div class="quiz-box">
<b>Count to get the frequency of the string in the list.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

```java
List<String> list = Arrays.asList("Hello","Hello","World");

// Output - "Hello" - 2, World - 1.
```

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

Frequency counting groups equal values and counts each group.

</details>

</div>

<div class="quiz-box">
<b>Reverse a list using stream.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
 Stream with index (does not mutate original list).

```java
List<Integer> list = Arrays.asList(10, 20, 30, 40, 50);
List<Integer> reversed = IntStream.range(0, list.size())
                .mapToObj(i -> list.get(list.size() - 1 - i))
                .collect(Collectors.toList());
```
Stream + reduce (functional style)

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);

LinkedList<Integer> reversed = list.stream().reduce(
    new LinkedList<>(),
    (acc, x) -> { acc.addFirst(x); return acc; },
    (left, right) -> { right.addAll(left); return right; }
);

System.out.println(reversed); // [5, 4, 3, 2, 1]
```
</details>
</div>

<div class="quiz-box">

<b>Find employee with highest salary using Java 8.</b>

<details class="quiz-toggle">
<summary>Reveal Answer</summary>

```java
class Employee {
    int id;
    String name;
    double salary;

    Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    double getSalary() {
        return salary;
    }
}

List<Employee> empList = Arrays.asList(
        new Employee(101, "Aman", 55000.0),
        new Employee(102, "Riya", 72000.0),
        new Employee(103, "Karan", 68000.0),
        new Employee(104, "Neha", 91000.0),
        new Employee(105, "Vijay", 88000.0)
);

Opional<Employee> empWithHigestSalary = empList.stream()
                .sorted(Comparator.comparingDouble(Employee::getSalary)
                .reversed())
                .findFirst();
// Sort salaries in descending order and take the first employee.
```

</div>

<div class="quiz-box">

**Q9. Find employee with second highest salary.**

<details class="quiz-toggle">
<summary>Reveal Answer</summary>

```java
Opional<Employee> empWithHigestSalary = empList.stream().sorted(Comparator.comparingDouble(Employee::getSalary).reversed()).skip(1).findFirst();
```

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

After sorting descending, skip one record and read the next.

</details>

</div>

<div class="quiz-box">

**Q10. Why are streams called lazy?**

<details class="quiz-toggle">
<summary>Reveal Answer</summary>

Stream are called lazy because intermediate operations are not evaluated unless terminal operation is invoked. They are only evaluated when a terminal operation is invoked. The operations are lazy, meaning they do not executed immediately.

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

Intermediate operations build a pipeline, and terminal operations trigger execution.

</details>

</div>

<div class="quiz-box">

**Q11. How does streams work in Java 8?**

<details class="quiz-toggle">
<summary>Reveal Answer</summary>

Java Stream is a pipeline of functions that can be evaluated. Java Stream is not a data structure and cannot mutate data, they can only transform data. Streams are built around its main interface, the Stream interface which was released in JDK 8.  
Three phases - Splitting, Applying and Combining.  
Elements of a stream is processed individually and then tey finally get collected.

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

A stream flows from source to intermediate steps to final terminal result.

</details>

</div>

<div class="quiz-box">

**Q12. What is Zookeeper in Kafka.**

<details class="quiz-toggle">
<summary>Reveal Answer</summary>

Storig metadate - Zookeeper stores essential data for running a Kafka cluster, such as registered brokers, topic configuration and the current controller.

Providing distributed coordination - Zooker acts as a centralized service that provides distributed coordination for applications deployed in a distributed system.

Maintaining configurayion information - Zookeepers keeps tract of data related to Kafka topics, brokers, consumers.

Fault tolerant - Zookeeper is highly available and can tolerate node failures.

Consistency - Zookeper offers a cosistent view of the cluster to all clients.

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

ZooKeeper has been used for metadata and broker coordination in classic Kafka architecture.

</details>

</div>

<div class="quiz-box">
<b>How many partition we can create in kafka.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Not fixed. Max pqrtitions per broker - 4000 partitions per broker. <br>Under the cluster there is broker. Max partition per cluster - 200000 partitions per cluster. Minimum partitions per topic - 1 partition per topic. Partition count is not fixed and depends on broker capacity and cluster sizing.
</div>

<div class="quiz-box">
<b>What are nodes and how it scales up with number of nodes.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
There are nodes inside Kafka.
Node are server that can be added to a cluster to scale up processing power and capacity.<br>

Scaling Up - Adding new nodes to a Kafka cluster increaes processing power, throughput, reduces latency. This is known as horizontal scaling.

Scaling Down - Removing nodes from a Kafka cluster decreases processing power.

Node Ids - Strimzi automatocally assigns node Ids starting from 0 and incrementing by one. You can also assign node Id ranges for each node pool.

Node pools - You can configure node pools usng a custom resource called KafkaNodePool. This resource supports configuration options such as the number of replicas, storage configuration and resource requirements.

Managed disk - You can use multiple disks to achieve 16Tb for each node in the cluster.

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

Adding nodes improves throughput and capacity, but requires balancing partitions and resources.

</details>

</div>

<div class="quiz-box">
<b>What is Insync Replica?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
In kafka way to achive data consistency and fault tolerance is by using replication to make sure tat messages are not lost if a broker fails. Every partition of a Kafka topic is replicated across multiple brokers. An insync replica ISR is a set of replicas that are fully in sync and replica with the leader replica of a partition. To put it simple, ISRs are replicas that have fully uptodate with the leader and have the same data as the leader.

Kafka replication models has leaders, followers, replication factor, ISR list.

</details>

<details class="quiz-toggle">
<summary>Reveal Explanation</summary>

ISR members are replicas that are sufficiently caught up with the leader.

</details>
</div>

<div class="quiz-box">
<b>How do you decide on how much memory your application will require on production?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
To find application memory usage with JMeter you can -
Go to Free Memory to check memory usage in a test
Use the PerfMon Metrics Collector Listener to monitor more than 75 PerfMon metrics, including memory
Calculate memory usage using the formula: (Used Memory/Total Memory) * 100
You can also use JMeter to: Identify an application's maximum operating capacity, Find bottlenecks, and Determine which element is causing system degradation
</details>
<details class="quiz-toggle">
<summary>Reveal Explanation</summary>
Use load testing plus memory metrics to estimate steady-state usage and production headroom.
</details>
</div>

<div class="quiz-box">
<b>What happens if we do not override the equals() method and the hashCode() method in Java?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
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
</details>
</div>

<div class="quiz-box">
<b>Why do we need Spring Boot when there is Spring.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Spring Boot is used to make Spring application development faster, more consistent, and production-ready.<br>
<b>Faster bootstrap</b> - Auto-configuration reduces manual setup for common concerns such as web, data source, JPA, security, and messaging.<br>
<b>Less boilerplate</b> - It follows convention over configuration, so developers spend less time wiring infrastructure and more time writing business logic.<br>
Standalone deployment - Embedded servers such as Tomcat, Jetty, or Undertow allow the application to run as a self-contained service without managing a separate application server.<br>
Dependency simplification - Starter dependencies such as `spring-boot-starter-web` and `spring-boot-starter-data-jpa` provide opinionated, compatible dependency sets.<br>
Production readiness - Actuator adds health checks, metrics, monitoring endpoints, and operational visibility, which are essential in real systems.<br>
Microservice friendliness - It works well with externalized configuration, containerized deployment, and Spring Cloud patterns such as config management, service discovery, and resilience.<br>
Strong ecosystem alignment - It integrates cleanly with Spring Security, Spring Data, Spring Batch, Kafka, Redis, and other enterprise components.<br>
In short, Spring Boot removes setup friction, standardizes application structure, and gives teams a faster path from development to production.
</details>
</div>

### OOPs

<div class="quiz-box">
<b>Explain the polymorphism.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Compile-time Polymorphism - Achieved using method overloading (methods with the same name but different parameter lists). The method to be called is determined at compile-time.

Runtime Polymorphism - Achieved using method overriding (methods with the same name and signature in a parent and child class). The method to be called is determined at runtime, based on the object's actual type. It is Dynamic Binding.
</details>
</div>

<div class="quiz-box">
<b>Method Overloading.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
More than one method with same name as long as the method has different parameter lists (different number of parameter or different types of parameters). It is a compile time polymorphism.<br>
Return type can be same or different, method signature (name and parameter) should be unique. Java determines which method to call based on the method signature at compile time.
</details>
</div>

<div class="quiz-box">
<b>Explain Encapsulation.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Encapsulation is the concept of bundling data (fields) and methods (functions) that operate on the data into a single unit, typically a class. It also involves restricting direct access to some of the object's components, ensuring data security and integrity through Getter and Setter methods.
</details>
</div>

<div class="quiz-box">
<b>Method Overloading.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
More than one method with same name as long as the method has different parameter lists (different number of parameter
or different types of parameters). It is a compile time polymorphism.

Return type can be same or different, method signature (name and parameter) should be unique. Java determines which
method to call based on the method signature at compile time.
</details>
</div>

<div class="quiz-box">
Explain Encapsulation.
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Encapsulation is the concept of bundling data (fields) and methods (functions) that operate on the data into a single unit, typically a class. It also involves restricting direct access to some of the object's components, ensuring data security and integrity. Getter and setter.
</details>
</div>

<div class="quiz-box">
<b>Generics</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Wildcards in Java are a feature of generics that provide flexibility in specifying the type of elements a generic class or method can operate on. Wildcards allow you to relax the type constraints when working with generics, enabling more flexible and reusable code.

Types of WildCard.

Unbounded Wildcard.

Syntax - <?> - Represents any type. Useful when the type is not required and it is just reading data or calling methods that don't depend on the type.
```java
public void printList(List<?> list) {
    for (Object obj : list) {
        System.out.println(obj);
    }
}

List<Integer> intList = List.of(1, 2, 3);
List<String> strList = List.of("A", "B", "C");
printList(intList); // Works
printList(strList); // Works
```

Upper-Bounded Wildcard.

Syntax - (<? extends Type>) - Restricts the unknown type to be a subtype (or the same type) of the specified class or interface. Useful when you want to read data of a specific type or its subtypes but do not want to modify the collection.
```java
public double sumOfNumbers(List<? extends Number> list) {
    double sum = 0;
    for (Number num : list) {
        sum += num.doubleValue();
    }
    return sum;
}

List<Integer> intList = List.of(1, 2, 3);
List<Double> doubleList = List.of(1.1, 2.2, 3.3);
System.out.println(sumOfNumbers(intList));     // Outputs: 6.0
System.out.println(sumOfNumbers(doubleList));  // Outputs: 6.6
```

Lower-Bounded Wildcard

Syntax - <? super Type> - Restricts the unknown type to be a superclass (or the same type) of the specified class or interface.
<br>Useful when you want to write data of a specific type or its subtypes into a collection.<br/>
```java
public void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}

List<Number> numberList = new ArrayList<>();
addNumbers(numberList);  // Works
System.out.println(numberList); // Outputs: [1, 2]
```

WildCard Use Cases.

Generic Class.
```java
class Box<T> {
    private T value;
    public void setValue(T value) { this.value = value; }
    public T getValue() { return value; }
}
Box<? extends Number> box = new Box<>();
```
Generic Method.
```java
public static void copy(List<? super Integer> dest, List<? extends Integer> src) {
    for (Integer i : src) {
        dest.add(i);
    }
}
```
PECS Principle (Producer Extends, Consumer Super).<br>

Producer: Use extends when you want to fetch data from a collection (Producer Extends)
<br/>Consumer: Use super when you want to insert data into a collection (Consumer Super)

```java
List<? extends Number> producer = List.of(1, 2, 3); // Produces data
List<? super Integer> consumer = new ArrayList<>();  // Consumes data
```
</details>
</div>

<div class="quiz-box">
<b>Difference between HashSet and TreeSet.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
The HashSet and TreeSet classes in java are the implementation of Set interface.

<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>HashSet</th>
            <th>TreeSet</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Performance</b></td>
            <td>Faster (O(1))</td>
            <td>Faster (O(log n))</td>
        </tr>
        <tr>
            <td><b>Order</b></td>
            <td>Unordered. No guarantee of order; it depends on the hash function.</td>
            <td>Sorted in natural order or custom order.<br/>The order depends on the natural ordering of elements via <code>Comparable</code> or a custom <code>Comparator</code>.</td>
        </tr>
        <tr>
            <td><b>Iteration</b></td>
            <td>Iterates in no specific order.</td>
            <td>Iterates in ascending sorted order, or in the order defined by a custom comparator.</td>
        </tr>
        <tr>
            <td><b>Null Values</b></td>
            <td>Allows one <code>null</code> value.</td>
            <td>Does not allow <code>null</code>.</td>
        </tr>
        <tr>
            <td><b>Internal Data Structure</b></td>
            <td>Uses <code>HashMap</code> internally for storage.</td>
            <td>Uses a Red-Black Tree (self-balancing binary search tree) internally.</td>
        </tr>
        <tr>
            <td><b>Sorting</b></td>
            <td>Custom sorting is not supported.</td>
            <td>Custom sorting is supported via <code>Comparator</code>.</td>
        </tr>
    </tbody>
</table>

```java
TreeSet<Integer> treeSet = new TreeSet<>((a, b) -> b - a); // Descending order
        treeSet.add(10);
        treeSet.add(5);
        treeSet.add(20);
System.out.println(treeSet); // Output: [20, 10, 5]

TreeSet<String> treeSet = new TreeSet<>();
        treeSet.add("Apple");
        treeSet.add("Banana");
        treeSet.add("Orange");
System.out.println("TreeSet: " + treeSet); // Sorted order

HashSet<String> hashSet = new HashSet<>();
        hashSet.add("Apple");
        hashSet.add("Banana");
        hashSet.add("Orange");
System.out.println("HashSet: " + hashSet); // Order not guaranteed
```
</details>
</div>

<div class="quiz-box">
<b>Difference between HashMap and TreeMap.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>HashMap</th>
            <th>TreeMap</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Performance</b></td>
            <td>O(1) average time complexity for basic operations.</td>
            <td>O(log n) time complexity for basic operations.</td>
        </tr>
        <tr>
            <td><b>Order</b></td>
            <td>No fixed order.</td>
            <td>Maintains keys in sorted order, either natural ordering or via a custom comparator.</td>
        </tr>
        <tr>
            <td><b>Null Key / Values</b></td>
            <td>Allows one <code>null</code> key and multiple <code>null</code> values.</td>
            <td>Does not allow <code>null</code> keys, but allows multiple <code>null</code> values.</td>
        </tr>
        <tr>
            <td><b>Internal Data Structure</b></td>
            <td>Hash table.</td>
            <td>Red-Black Tree (self-balancing binary search tree).</td>
        </tr>
    </tbody>
</table>
</details>
</div>

<div class="quiz-box">
<b>Difference between HashMap and Hashtable.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>HashMap</th>
            <th>Hashtable</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Thread Safety</b></td>
            <td>Not thread-safe. It needs external synchronization, such as <code>Collections.synchronizedMap()</code>, in multithreaded environments.</td>
            <td>Thread-safe because synchronization is built into its methods.</td>
        </tr>
        <tr>
            <td><b>Null Key / Values</b></td>
            <td>Allows one <code>null</code> key and multiple <code>null</code> values.</td>
            <td>Does not allow <code>null</code> keys or <code>null</code> values and throws <code>NullPointerException</code>.</td>
        </tr>
        <tr>
            <td><b>Performance</b></td>
            <td>Usually faster because there is no synchronization overhead.</td>
            <td>Usually slower because synchronization adds overhead.</td>
        </tr>
        <tr>
            <td><b>Package / History</b></td>
            <td>Introduced in Java 1.2 as part of the Java Collections Framework in <code>java.util</code>. Preferred in modern code.</td>
            <td>Legacy class from Java 1.0 in <code>java.util</code>. Generally not recommended in new code.</td>
        </tr>
        <tr>
            <td><b>Usage</b></td>
            <td>Preferred in single-threaded code or when synchronization is handled externally.</td>
            <td>Rarely used in modern applications; <code>ConcurrentHashMap</code> is usually preferred for thread-safe access.</td>
        </tr>
    </tbody>
</table>

</details>
</div>

<div class="quiz-box">
<b>Difference between Map and FlatMap in streamAPI.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
MAP.

**Purpose**: Transforms each element in the stream into another element.  
**Output**: Produces a new stream of the same size but with transformed elements.  
**Behavior**: Applies the given function to each element, and the result is a stream of transformed values.  
**Use Case**: When you need to transform each element into a single element.

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");

list.stream()
    .map(String::toUpperCase)  // Transforms each string to uppercase
    .forEach(System.out::println);  // Outputs: APPLE, BANANA, CHERRY
```

FLATMAP.

**Purpose**: Transforms each element into a stream of elements and then flattens these multiple streams into a single stream.  
**Output**: Produces a new stream where each element may expand into multiple elements (or none).  
**Behavior**: Each element is mapped to a stream, and the resulting streams are merged into a single stream.  
**Use Case**: When you need to handle nested structures or when each element can be expanded into multiple elements (like splitting strings or unwrapping lists).
```java
List<String> list = Arrays.asList("apple", "banana", "cherry");

list.stream()
    .flatMap(s -> Arrays.stream(s.split("")))  // Splits each string into a stream of characters
    .forEach(System.out::println);  // Outputs: a, p, p, l, e, b, a, n, a, n, a, c, h, e, r, r, y
```
Using map and flatmap.
```java
List<String> list = Arrays.asList("apple", "banana");

// Using map to convert each word to uppercase
list.stream()
    .map(word -> word.toUpperCase())
    .forEach(System.out::println);  // APPLE BANANA

// Using flatMap to split each word into its individual characters
list.stream()
    .flatMap(word -> Arrays.stream(word.split("")))
    .forEach(System.out::println);  // a p p l e b a n a n a
```


Map transform each item in a collection into something else and produces a collection of the same size.
Transforms each element of the stream into another form (1-to-1 mapping). The result is a Stream of Streams if the transformation returns a Stream.
```java
List<String> words = Arrays.asList("hello", "world");
List<Stream<Character>> result = words.stream()
                                       .map(word -> word.chars().mapToObj(c -> (char) c))
                                       .collect(Collectors.toList());
// Result: [Stream[h, e, l, l, o], Stream[w, o, r, l, d]]
```
Use when you want to transform each element independently, and the transformation results in a single output per input. Example: Converting a list of strings to their lengths.
```java
List<String> words = Arrays.asList("hello", "world");
List<Integer> lengths = words.stream()
                             .map(String::length)
                             .collect(Collectors.toList());
// Result: [5, 5]
```

Flatmap transforms each item but can combine items from nested collections into a single flat collections.
Transforms each element into a Stream and flattens all these Streams into a single Stream (1-to-many mapping).
```java
List<String> words = Arrays.asList("hello", "world");
List<Character> result = words.stream()
                              .flatMap(word -> word.chars().mapToObj(c -> (char) c))
                              .collect(Collectors.toList());
// Result: [h, e, l, l, o, w, o, r, l, d]
```
Use when each element needs to be transformed into multiple elements (or a stream of elements) and you want a flat result. Example: Splitting a list of sentences into words.
```java
List<String> sentences = Arrays.asList("hello world", "java streams");
List<String> words = sentences.stream()
                              .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
                              .collect(Collectors.toList());
// Result: [hello, world, java, streams]
```
</details>
</div>

<div class="quiz-box">
<b>Difference between normal stream and parallel stream.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>Normal Stream</th>
            <th>Parallel Stream</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Execution</b></td>
            <td>Processed sequentially, one element at a time, in source order.</td>
            <td>Processed concurrently by splitting the source into chunks handled by multiple threads.</td>
        </tr>
        <tr>
            <td><b>Performance</b></td>
            <td>Best for smaller datasets or lightweight operations where parallel overhead is not worth it.</td>
            <td>Can improve performance for large datasets or CPU-intensive work, but may be slower for small tasks because of parallel overhead.</td>
        </tr>
        <tr>
            <td><b>Thread Management</b></td>
            <td>Runs in a single thread, usually the main thread.</td>
            <td>Uses the common <code>ForkJoinPool</code>, typically with worker threads based on available CPU cores.</td>
        </tr>
        <tr>
            <td><b>Order</b></td>
            <td>Maintains encounter order.</td>
            <td>May not preserve order unless explicitly handled, for example with <code>forEachOrdered()</code>.</td>
        </tr>
        <tr>
            <td><b>Usage</b></td>
            <td>Best for simple and small operations where parallelism is unnecessary.</td>
            <td>Best for large collections or time-consuming operations that benefit from parallel execution.</td>
        </tr>
        <tr>
            <td><b>Processing</b></td>
            <td>Processes all elements in order and commonly uses the <code>main</code> thread.</td>
            <td>Processes elements concurrently using multiple threads such as <code>ForkJoinPool.commonPool-worker-*</code>.</td>
        </tr>
    </tbody>
</table>

The example of a normal stream.

```java
List<String> items = Arrays.asList("A", "B", "C", "D", "E");
System.out.println("Normal Stream:");
items.stream()
     .forEach(item -> {
         System.out.println(Thread.currentThread().getName() + " processes " + item);
     });
```
The output is given.
```xml
Normal Stream:
main processes A
main processes B
main processes C
main processes D
main processes E
```
```java
System.out.println("\nParallel Stream:");
// Parallel Stream (Multi-Threaded)
items.parallelStream()
     .forEach(item -> {
         System.out.println(Thread.currentThread().getName() + " processes " + item);
     });
```
The output.
```java
Parallel Stream:
ForkJoinPool.commonPool-worker-1 processes A
main processes B
ForkJoinPool.commonPool-worker-3 processes C
ForkJoinPool.commonPool-worker-2 processes D
main processes E
```
</details>
</div>

<div class="quiz-box">
<b>Difference between stream and collection.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>Stream</th>
            <th>Collection</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Nature</b></td>
            <td>A stream is a sequence of elements that supports sequential or parallel data processing operations.</td>
            <td>A collection is a data structure that stores objects in memory, such as <code>List</code>, <code>Set</code>, or <code>Map</code>.</td>
        </tr>
        <tr>
            <td><b>Storage</b></td>
            <td>Does not store data; it operates on a source such as a collection or array.</td>
            <td>Stores data in memory.</td>
        </tr>
        <tr>
            <td><b>Operation</b></td>
            <td>Mainly used for transformation and processing operations like <code>map</code>, <code>filter</code>, and <code>reduce</code>.</td>
            <td>Manipulated through methods like <code>add</code>, <code>remove</code>, <code>get</code>, and iteration APIs.</td>
        </tr>
        <tr>
            <td><b>Lazy Evaluation</b></td>
            <td>Operations are lazy; execution happens only when a terminal operation is invoked.</td>
            <td>Eager by nature; data is already stored and available immediately.</td>
        </tr>
        <tr>
            <td><b>Consumption</b></td>
            <td>Can be consumed only once; a new stream must be created for reuse.</td>
            <td>Can be accessed multiple times without recreation.</td>
        </tr>
        <tr>
            <td><b>Parallelism</b></td>
            <td>Can be processed in parallel easily using <code>parallelStream()</code>.</td>
            <td>Does not provide built-in parallel processing in the same way streams do.</td>
        </tr>
        <tr>
            <td><b>Use Case</b></td>
            <td>Designed for data transformation and processing.</td>
            <td>Designed for storing and managing data.</td>
        </tr>
    </tbody>
</table>
</details>
</div>

<div class="quiz-box">
<b>Difference between ConcurrentHashMap and SynchronizedHashMap.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>ConcurrentHashMap</th>
            <th>SynchronizedHashMap</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Synchronization Mechanism</b></td>
            <td>Uses segment-based locking (Java 7) or bucket-level locking (Java 8+).</td>
            <td>Entire map is locked for each operation using synchronized blocks.</td>
        </tr>
        <tr>
            <td><b>Concurrency</b></td>
            <td>Allows concurrent reads and writes by multiple threads; only writes to the same bucket are blocked.</td>
            <td>Allows only one thread to access the map at a time.</td>
        </tr>
        <tr>
            <td><b>Performance</b></td>
            <td>Higher performance in multithreaded environments due to finer-grained locking.</td>
            <td>Lower performance due to coarse-grained locking (locks the entire map).</td>
        </tr>
        <tr>
            <td><b>Null Values</b></td>
            <td>Does not allow <code>null</code> keys or values.</td>
            <td>Allows a single <code>null</code> key and multiple <code>null</code> values.</td>
        </tr>
        <tr>
            <td><b>Thread Safety</b></td>
            <td>Thread-safe for concurrent access with better scalability.</td>
            <td>Thread-safe, but less efficient in high-concurrency scenarios.</td>
        </tr>
        <tr>
            <td><b>Locking Granularity</b></td>
            <td>Fine-grained locks improve throughput by reducing contention.</td>
            <td>Coarse-grained locks block all threads accessing the map, even for independent operations.</td>
        </tr>
        <tr>
            <td><b>Iteration Behavior</b></td>
            <td>Does not throw <code>ConcurrentModificationException</code> during iteration; reflects changes made by other threads.</td>
            <td>Throws <code>ConcurrentModificationException</code> if the map is modified during iteration.</td>
        </tr>
        <tr>
            <td><b>Use Case</b></td>
            <td>Best suited for high-concurrency applications where reads and updates are frequent.</td>
            <td>Suitable for low-concurrency scenarios where simplicity is preferred over performance.</td>
        </tr>
    </tbody>
</table>

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

**Explain CAS**

Compare-And-Swap (CAS) is an atomic operation used in concurrent programming to achieve synchronization without locks. It enables threads to update shared variables safely without the overhead and contention caused by traditional locking mechanisms.

**Memory Location** - CAS reads the value at the memory location.

**Expected Value** - It compares the read value with the expected value.
If the current value matches the expected value, CAS updates the variable with the new value.
If the current value does not match the expected value, CAS fails, and no update occurs.

**New Value** - The operation returns a status indicating whether the swap was successful.

Example - Thread A. Memory location 5. Value = 5. Expected value = 5. New Value = 10. The value is updated to 10.

<br/>Example - Thread B. Memory location 6. Value = 5. Expected value = 10. New Value = 15. The value is not updated as current value is 10.

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

</details>
</div>

<div class="quiz-box">
<b>Difference between StringBuilder and StringBuffer and String.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

**String**.

Immutable. Any change will create a new String.
<br/>Not Thread safe and not synchronized.
<br/>Slower as it will create new string.

**StringBuilder**.

Mutable. Allows in place modification in string.
<br/>Not thread safe and not synchronized.
<br/>Faster than StringBuffer as no synchronization.
<br/>Used in single threaded environment with frequent string modification.

**StringBuffer**.
<br/>Mutable and Thread safe. Synchronization ensures thread safety.
<br/>

</details>
</div>

<div class="quiz-box">
<b>Difference between default and static method.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>

<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>Default Method</th>
            <th>Static Method</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Definition</b></td>
            <td>A method with a body in an interface, invoked on an instance.</td>
            <td>A method declared with the <code>static</code> keyword, invoked on the class.</td>
        </tr>
        <tr>
            <td><b>Purpose</b></td>
            <td>Provides a default implementation for interface methods, ensuring backward compatibility. Works with instance variables and methods.</td>
            <td>Defines utility or helper methods unrelated to instance-specific behavior. Works only with static data and does not access instance variables or methods.</td>
        </tr>
        <tr>
            <td><b>Inheritance</b></td>
            <td>Can be inherited and overridden by implementing classes.</td>
            <td>Cannot be overridden but can be hidden (if declared in a class).</td>
        </tr>
    </tbody>
</table>

Default Method.

```java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle is starting...");
    }
}

class Car implements Vehicle {}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.start(); // Output: Vehicle is starting...
    }
}
```

Static Method.
```java
interface Utility {
    static void log(String message) {
        System.out.println("Log: " + message);
    }
}

public class Main {
    public static void main(String[] args) {
        Utility.log("This is a static method."); // Output: Log: This is a static method.
    }
}
```
</details>
</div>

<div class="quiz-box">
<b>Difference between Future and CompletableFuture, Runnable and Callable.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>Future</th>
            <th>CompletableFuture</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Core Idea</b></td>
            <td>Represents the result of an asynchronous computation.</td>
            <td>Extends <code>Future</code> with composition and callback APIs for async workflows.</td>
        </tr>
        <tr>
            <td><b>Result Handling</b></td>
            <td>Mostly blocking retrieval via <code>get()</code>.</td>
            <td>Provides non-blocking callbacks like <code>thenAccept()</code>, <code>thenApply()</code>, and <code>thenCompose()</code>.</td>
        </tr>
        <tr>
            <td><b>Composition</b></td>
            <td>No built-in fluent chaining for multiple async stages.</td>
            <td>Supports fluent chaining, combining, and dependent async operations.</td>
        </tr>
        <tr>
            <td><b>Exception Handling</b></td>
            <td>No rich built-in exception pipeline; handling is typically done around <code>get()</code>.</td>
            <td>Built-in exception handling via <code>exceptionally()</code>, <code>handle()</code>, and <code>whenComplete()</code>.</td>
        </tr>
    </tbody>
</table>

<table>
    <thead>
        <tr>
            <th>Feature</th>
            <th>Runnable</th>
            <th>Callable</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><b>Return Value</b></td>
            <td>Returns no result (<code>void</code>).</td>
            <td>Returns a result of type <code>V</code>.</td>
        </tr>
        <tr>
            <td><b>Exception Support</b></td>
            <td>Cannot throw checked exceptions from <code>run()</code>.</td>
            <td>Can throw checked exceptions from <code>call()</code>.</td>
        </tr>
        <tr>
            <td><b>Typical Use</b></td>
            <td>Best for fire-and-forget tasks.</td>
            <td>Best for tasks that compute and return a value.</td>
        </tr>
        <tr>
            <td><b>Method Signature</b></td>
            <td><code>public abstract void run();</code></td>
            <td><code>V call() throws Exception;</code></td>
        </tr>
    </tbody>
</table>
</details>
</div>

<div class="quiz-box">
<b>What is finally, finalize and final.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<b>finally</b> - A block in a try-catch statement that always executes, regardless of whether an exception is thrown or not. <br>
<b>Purpose</b> - Used for cleanup actions like closing files, releasing resources, or disconnecting from a database. <br>
<b>Key Points</b> - The finally block executes even if the try block contains a return statement.
It does not execute if the JVM terminates abruptly (e.g., with System.exit())

```java 
public class FinallyExample {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // Will throw ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Exception caught: " + e.getMessage());
        } finally {
            System.out.println("This block always executes.");
        }
    }
}
```
The output.
```
Exception caught: / by zero
This block always executes.
```
<br>
<b>finalize</b> - A method in the Object class that can be overridden to clean up resources before an object is garbage collected. <br>
<b>Purpose</b> - Used for resource management (like closing files or releasing memory), though it's not recommended for modern applications.<br>
<b>Deprecated</b> - As of Java 9, finalize() is deprecated due to performance issues and unpredictability.<br>
<b>Key Points</b> - Called by the garbage collector before the object is destroyed. Not guaranteed to execute promptly or even at all.

```java
public class FinalizeExample {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Finalize method called.");
    }

    public static void main(String[] args) {
        FinalizeExample obj = new FinalizeExample();
        obj = null; // Make the object eligible for garbage collection
        System.gc(); // Suggest garbage collection
        System.out.println("End of main method.");
    }
}

```
The output
```
End of main method.
Finalize method called.
```
final - A keyword that can be applied to variables, methods, or classes to restrict their behavior.
Purpose: Used to define constants, prevent method overriding, and prevent inheritance.
Key Points:

Final Variables: Once assigned, their value cannot change.
Final Methods: Cannot be overridden in subclasses.
Final Classes: Cannot be extended by other classes

```java
public class FinalVariableExample {
    public static void main(String[] args) {
        final int CONSTANT = 10;
        // CONSTANT = 20; // Compilation error: cannot assign a value to final variable
        System.out.println("Final variable value: " + CONSTANT);
    }
}
```

</details>
</div>

<div class="quiz-box">
<b>Collection Hierarchy.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
<img src="/images/Java/CollectionHierarchy.png" alt="Collection Hierarchy" style="max-width:100%; display:block; margin:auto;" />
</details>
</div>
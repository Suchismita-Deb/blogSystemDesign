## Java Quiz - Stream and Kafka

## Streams

### Find sum of even numbers from array.

``` java
int num[] = {1,2,3,4,4,5,6,7,8};
int sum = Arrays.stream(nums).filter(n->n%2==0).sum();
System.out.println(sum);
// `filter` keeps only even values and `sum()` adds them. 
```

### Count the occurrence "apple" in the list.

```java
List<String> list = Arrays.asList("apple","banana","orange","apple","apple");
long count = list.steam().filter(word->words.equalOrIgnoreCase("apple")).count();
System.out.println(count); // 3
```

### List of employee sort by salary then sort by name.
```java

employees.stream()
  .sorted(Comparator.comparing(Employee::getSalary)
  .thenComparing(Employee::getName))
        .toList();
```

### Given a list of list put all the elements in the same list.
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

### Find the skills starting with character 's'.
```java
List<String> skillsStartsWithS = allSkills.stream().filter(s -> s.charAt(0)=='s').collect(Collectors.toList());
// s.charAt(0)=='s' and s.startsWith("S") does same thing.
System.out.println(skillsStartsWithS);
```

### Age of an employee above 30.
```java
List<Integer> l = Arrays.asList(1,2,3,4,5);
List<Integer> list - l.stream().filter(x->x>3).collect(Collectors.toList());
System.out.println(list);
```

Apply `filter(x -> x > threshold)` to keep only higher ages.

### Count to get the frequency of the string in the list.
```java
List<String> list = Arrays.asList("Hello","Hello","World");

// Output - "Hello" - 2, World - 1.
```
### Frequency counting groups equal values and counts each group.

### Reverse a list using stream.
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

### Find employee with highest salary using Java 8.
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
### Find employee with second highest salary.
```java
Opional<Employee> empWithHigestSalary = empList.stream().sorted(Comparator.comparingDouble(Employee::getSalary).reversed()).skip(1).findFirst();
```

### Why are streams called lazy?
Stream are called lazy because intermediate operations are not evaluated unless terminal operation is invoked. They are only evaluated when a terminal operation is invoked. The operations are lazy, meaning they do not executed immediately.

### How does streams work in Java 8?
Java Stream is a pipeline of functions that can be evaluated. Java Stream is not a data structure and cannot mutate data, they can only transform data. Streams are built around its main interface, the Stream interface which was released in JDK 8.  
Three phases - Splitting, Applying and Combining.  
Elements of a stream is processed individually and then tey finally get collected.


### Array contains duplicate element. Print the distinct element.

```java
list.stream().distinct().collect(Collectors.toList());
```

### Java Stream collector.
Collectors in the `java.util.stream.Collectors` class.
`partitioningBy` - Splits elements into two groups based on a boolean predicate (true / false) Example - Even and odd number.<br>

```java
List<Integer> numbers = Arrays.asList(1,2,3,4,5,6);

Map<Boolean, List<Integer>> partitioned = numbers
                                          .stream()
                                          .collect(Collectors.partitioningBy(n -> n % 2 == 0));

System.out.println(partitioned);
// {false=[1, 3, 5], true=[2, 4, 6]}
```
`groupingBy` - Groups elements into multiple categories based on a classifier function. Example - group student by departments.

```java
List<String> words = Arrays.asList("apple", "bat", "ball", "cat");

Map<Integer, List<String>> grouped = words.stream()
                                          .collect(Collectors.groupingBy(String::length));

System.out.println(grouped);
// {3=[bat, cat], 4=[ball], 5=[apple]}
```

```java
List<String> words = Arrays.asList("apple", "bat", "ball", "cat", "dog", "ant");

// 1. toList
List<String> listResult = words.stream().collect(toList());
System.out.println("toList: " + listResult);

// 2. toSet
Set<String> setResult = words.stream().collect(toSet());
System.out.println("toSet: " + setResult);

// 3. toMap
Map<String, Integer> mapResult = words.stream()
                                      .collect(toMap(w -> w, String::length));
System.out.println("toMap: " + mapResult);

// 4. joining
String joined = words.stream().collect(joining(", "));
System.out.println("joining: " + joined);

// 5. counting
long count = words.stream().collect(counting());
System.out.println("counting: " + count);

// 6. partitioningBy (split into two groups)
Map<Boolean, List<String>> partitioned = words.stream()
                                              .collect(partitioningBy(w -> w.length() > 3));
System.out.println("partitioningBy: " + partitioned);


// 7. groupingBy (group by length)
Map<Integer, List<String>> grouped = words.stream()
                                          .collect(groupingBy(String::length));
System.out.println("groupingBy: " + grouped);

// 8. summarizingInt (statistics)
IntSummaryStatistics stats = 
            words.stream().collect(summarizingInt(String::length));
        System.out.println("summarizingInt: " + stats);
```

## Kafka
### How many partition we can create in kafka.
Not fixed. Max pqrtitions per broker - 4000 partitions per broker. <br>Under the cluster there is broker. Max partition per cluster - 200000 partitions per cluster. Minimum partitions per topic - 1 partition per topic. Partition count is not fixed and depends on broker capacity and cluster sizing.

### What are nodes and how it scales up with number of nodes.
There are nodes inside Kafka.
Node are server that can be added to a cluster to scale up processing power and capacity.<br>

Scaling Up - Adding new nodes to a Kafka cluster increaes processing power, throughput, reduces latency. This is known as horizontal scaling.

Scaling Down - Removing nodes from a Kafka cluster decreases processing power.

Node Ids - Strimzi automatocally assigns node Ids starting from 0 and incrementing by one. You can also assign node Id ranges for each node pool.

Node pools - You can configure node pools usng a custom resource called KafkaNodePool. This resource supports configuration options such as the number of replicas, storage configuration and resource requirements.

Managed disk - You can use multiple disks to achieve 16Tb for each node in the cluster.

### What is Insync Replica?
### Reveal Answer
In kafka way to achive data consistency and fault tolerance is by using replication to make sure tat messages are not lost if a broker fails. Every partition of a Kafka topic is replicated across multiple brokers. An insync replica ISR is a set of replicas that are fully in sync and replica with the leader replica of a partition. To put it simple, ISRs are replicas that have fully uptodate with the leader and have the same data as the leader.

Kafka replication models has leaders, followers, replication factor, ISR list.

### Reveal Explanation

ISR members are replicas that are sufficiently caught up with the leader.

### How do you decide on how much memory your application will require on production?
### Reveal Answer
To find application memory usage with JMeter you can -
Go to Free Memory to check memory usage in a test
Use the PerfMon Metrics Collector Listener to monitor more than 75 PerfMon metrics, including memory
Calculate memory usage using the formula: (Used Memory/Total Memory) * 100
You can also use JMeter to: Identify an application's maximum operating capacity, Find bottlenecks, and Determine which element is causing system degradation
### Reveal Explanation
Use load testing plus memory metrics to estimate steady-state usage and production headroom.

### Why do we need Spring Boot when there is Spring.
Spring Boot is used to make Spring application development faster, more consistent, and production-ready.<br>
### Faster bootstrap
Auto-configuration reduces manual setup for common concerns such as web, data source, JPA, security, and messaging.<br>
### Less boilerplate
It follows convention over configuration, so developers spend less time wiring infrastructure and more time writing business logic.<br>
Standalone deployment - Embedded servers such as Tomcat, Jetty, or Undertow allow the application to run as a self-contained service without managing a separate application server.<br>
Dependency simplification - Starter dependencies such as `spring-boot-starter-web` and `spring-boot-starter-data-jpa` provide opinionated, compatible dependency sets.<br>
Production readiness - Actuator adds health checks, metrics, monitoring endpoints, and operational visibility, which are essential in real systems.<br>
Microservice friendliness - It works well with externalized configuration, containerized deployment, and Spring Cloud patterns such as config management, service discovery, and resilience.<br>
Strong ecosystem alignment - It integrates cleanly with Spring Security, Spring Data, Spring Batch, Kafka, Redis, and other enterprise components.<br>
In short, Spring Boot removes setup friction, standardizes application structure, and gives teams a faster path from development to production.

### OOPs

### Explain the polymorphism.
Compile-time Polymorphism - Achieved using method overloading (methods with the same name but different parameter lists). The method to be called is determined at compile-time.

Runtime Polymorphism - Achieved using method overriding (methods with the same name and signature in a parent and child class). The method to be called is determined at runtime, based on the object's actual type. It is Dynamic Binding.

### Method Overloading.
More than one method with same name as long as the method has different parameter lists (different number of parameter or different types of parameters). It is a compile time polymorphism.  
Return type can be same or different, method signature (name and parameter) should be unique. Java determines which method to call based on the method signature at compile time.


### Explain Encapsulation.
Encapsulation is the concept of bundling data (fields) and methods (functions) that operate on the data into a single unit, typically a class. It also involves restricting direct access to some of the object's components, ensuring data security and integrity through Getter and Setter methods.

### Method Overloading.
More than one method with same name as long as the method has different parameter lists (different number of parameter
or different types of parameters). It is a compile time polymorphism.

Return type can be same or different, method signature (name and parameter) should be unique. Java determines which
method to call based on the method signature at compile time.

### Explain Encapsulation.
Encapsulation is the concept of bundling data (fields) and methods (functions) that operate on the data into a single unit, typically a class. It also involves restricting direct access to some of the object's components, ensuring data security and integrity. Getter and setter.

### Generics.
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

### Difference between Map and FlatMap in streamAPI.
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


### Difference between normal stream and parallel stream.
| Feature | Normal Stream | Parallel Stream |
|---------|---------------|-----------------|
| **Execution** | Processed sequentially, one element at a time, in source order. | Processed concurrently by splitting the source into chunks handled by multiple threads. |
| **Performance** | Best for smaller datasets or lightweight operations where parallel overhead is not worth it. | Can improve performance for large datasets or CPU-intensive work, but may be slower for small tasks due to parallel overhead. |
| **Thread Management** | Runs in a single thread, usually the main thread. | Uses the common `ForkJoinPool`, typically with worker threads based on available CPU cores. |
| **Order** | Maintains encounter order. | May not preserve order unless explicitly handled (e.g., with `forEachOrdered()`). |
| **Usage** | Best for simple and small operations where parallelism is unnecessary. | Best for large collections or time-consuming operations that benefit from parallel execution. |
| **Processing** | Processes all elements in order using the `main` thread. | Processes elements concurrently using multiple threads such as `ForkJoinPool.commonPool-worker-*`. |

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
```
Parallel Stream:
ForkJoinPool.commonPool-worker-1 processes A
main processes B
ForkJoinPool.commonPool-worker-3 processes C
ForkJoinPool.commonPool-worker-2 processes D
main processes E
```

### Difference between stream and collection.
| Feature | Stream | Collection |
|---------|--------|------------|
| **Nature** | A stream is a sequence of elements that supports sequential or parallel data processing operations. | A collection is a data structure that stores objects in memory, such as `List`, `Set`, or `Map`. |
| **Storage** | Does not store data; it operates on a source such as a collection or array. | Stores data in memory. |
| **Operation** | Mainly used for transformation and processing operations like `map`, `filter`, and `reduce`. | Manipulated through methods like `add`, `remove`, `get`, and iteration APIs. |
| **Lazy Evaluation** | Operations are lazy; execution happens only when a terminal operation is invoked. | Eager by nature; data is already stored and available immediately. |
| **Consumption** | Can be consumed only once; a new stream must be created for reuse. | Can be accessed multiple times without recreation. |
| **Parallelism** | Can be processed in parallel easily using `parallelStream()`. | Does not provide built-in parallel processing in the same way streams do. |
| **Use Case** | Designed for data transformation and processing. | Designed for storing and managing data. |


### Difference between StringBuilder and StringBuffer and String.

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


### Difference between default and static method.
| Feature | Default Method | Static Method |
|---------|----------------|---------------|
| **Definition** | A method with a body in an interface, invoked on an instance. | A method declared with the `static` keyword, invoked on the class. |
| **Purpose** | Provides a default implementation for interface methods, ensuring backward compatibility. Works with instance variables and methods. | Defines utility or helper methods unrelated to instance-specific behavior. Works only with static data and does not access instance variables or methods. |
| **Inheritance** | Can be inherited and overridden by implementing classes. | Cannot be overridden but can be hidden (if declared in a class). |

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
### What is finally, finalize and final.

### finally
A block in a try-catch statement that always executes, regardless of whether an exception is thrown or not. <br>
### Purpose
Used for cleanup actions like closing files, releasing resources, or disconnecting from a database. <br>
### Key Points
The finally block executes even if the try block contains a return statement.
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
### finalize
A method in the Object class that can be overridden to clean up resources before an object is garbage collected. <br>
### Purpose
Used for resource management (like closing files or releasing memory), though it's not recommended for modern applications.<br>
### Deprecated
As of Java 9, finalize() is deprecated due to performance issues and unpredictability.<br>
### Key Points
Called by the garbage collector before the object is destroyed. Not guaranteed to execute promptly or even at all.

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

### Collection Hierarchy.
<img src="/images/Java/CollectionHierarchy.png" alt="Collection Hierarchy" style="max-width:100%; display:block; margin:auto;" />


### String Interning.
String interning is a process of reusing strings to optimize memory usage. Strings are immutable in java in order to
avoid the duplicate string with same value, Java uses string pool. String pool is a special area in the heap memory.<br>

### How string intern works.

A pool of unique string literals is maintained in the JVM. This pool is part of the heap memory often called the String
Intern Pool or String Constant pool.
We can manually ensure a string is part of the pool using the `String.intern()` method. String already present in the
pool then the intern() returns a referenced to the pooled string. Not present then the string is added to the pool and
the referenced to the pooled string is returned.

```java
String str = "hello"; // Added to the pool
String str1 = "hello"; // Refering to the same object as str.
// str and str1 points to the same object inside the pool.
String str3 = new String("hello");
// String object is created inside the heap memory outside the pool.
// We can move it to the pool.
str3 = str3.intern(); // Add str3 to the pool or returns reference to the pooled string.

```
Comparison.

```java
String s = "hello";
String str = new String("hello");
System.out.println(s==str1); // false different reference pool and heap.

str =str.intern(); // Now str is pointing to the pool string.
System.out.println(str1==str3);
```
When the string is interned then == is faster and string not interned then we have to use .equals

### How the `@Autowired`, `@Resource` and `@Inject` differs from each other.
### Reveal Answer
Used for dependency injection and they differs in terms of usage, behaviour and source.

| `@Autowired`                                                                                              | `@Resource`                                                                                                                                                                                                                                                            | `@Inject`                                                                  |
|---|---|---|
| Spring Specific and it does not works outside. Comes from Spring Framework. Works by **type**(bean type). | Java Standard and works both in spring and Java EE frameworks. Works by **name** first and then by type. | Java Standard and works with Java framework and Spring. Works by **type**. |
|Behaviour - Spring attempts to match the bean type for injection. If multiple beans of the same type exists, it requires additional qualifiers(@Qualifier) to resolve ambiguity. Can be used on contructors, fields or setter method. Required Behaviour - By default @Autowired is required. If no matching bean is found it throws an exception. `@Autowired(required = false)` to make it optional. | When the name is specified (`@Resource(name = "beanName")`) then it searched for the bean with that name. No name is specified then it falls back to the field name. When not resolved then it falls back to teh type based injection. It does not supports @Qualifier. | Optional and no bean is found then it does not throw an exception by default. Does not supports @Qualifier but works with the @Named qualifier for ambiguity.|

```java
public static void main(String[] args) {
    Integer num = 10;
    modify(num);
    System.out.println(num);
}

public static void modify(Integer num) {
num = 200;
}
```
### Reveal Answer

The output is 10. Java is pass by value. Primitive are pass by value. Wrapper class will work but not Integer as Integer
is immutable. Wrapper class with immutable like `AtomicInteger` or custom Wrapper class will work.

We can reassign. The object will be created. We cannot see the memory of the object. The hashcode is the unique for the
object.

```java
public static void main(String[] args) {
    Integer num = 10;
    modify(num);
    System.out.println(System.identityHashCode(num)); // 617901222
    num = 100;
    System.out.println(System.identityHashCode(num)); // 1159190947
    System.out.println(num);
}
```

```java
class Employee {
    Address address;
    String name;
    int age;
}

class Address {
String streetNAme;
String place;
int pinCode;
}

public static void main(String[] args) {
    // Sample Data
    List<Employee> employees = Arrays.asList(
            new Employee("John", 28, new Address("1st Main", "CityA", 560001)),
            new Employee("Alice", 32, new Address("2nd Cross", "CityB", 560002)),
            new Employee("Bob", 45, new Address("3rd Lane", "CityA", 560001)),
            new Employee("Eve", 25, new Address("4th Street", "CityC", 560003))
    );
}
```
Write the employee based on the PinCode.<br>
Group the employee based on the age.
### Reveal Answer

```java
// Group employees by pinCode
Map<Integer, List<Employee>> groupedByPinCode = employees.stream()
            .collect(Collectors.groupingBy(e -> e.address.pinCode));

    // Print grouped employees
groupedByPinCode.forEach((pinCode, employeeList) -> {
        System.out.println("PinCode: " + pinCode);
        employeeList.forEach(System.out::println);
        System.out.println();
});
```
The output.

```md
PinCode: 560001
Employee{name='John', age=28, address=Address{streetName='1st Main', place='CityA', pinCode=560001}}
Employee{name='Bob', age=45, address=Address{streetName='3rd Lane', place='CityA', pinCode=560001}}

PinCode: 560002
Employee{name='Alice', age=32, address=Address{streetName='2nd Cross', place='CityB', pinCode=560002}}

PinCode: 560003
Employee{name='Eve', age=25, address=Address{streetName='4th Street', place='CityC', pinCode=560003}
```

<br>
Group the employee based on age.

```java
Map<Integer, List<Employee>> groupedByAge = employees.stream()
            .collect(Collectors.groupingBy(e -> e.age));

    // Print grouped employees
groupedByAge.forEach((age, employeeList) -> {
        System.out.println("Age: " + age);
        employeeList.forEach(System.out::println);
        System.out.println();
});
```

What is the output?

```java
public class A1 {
    public static void addToInt(int x, int amountToAdd) {
        x = x + amountToAdd;
    }

    public static void main(String[] args) {
        var a = 15;
        var b = 10;
        A1.addToInt(a,b);
        System.out.println(a);
    }
}
```
### Reveal Answer
The output of the code - 15.<br>
The method is getting the value and not the reference so the value of the varaiable will not change.

### What are the new features introduced in Java 8.
### Reveal Answer
The new features revolutionizes how java applications are written and optimized.
Lambda Expression.

Enable functional programming with concise code for implementing functional interfaces.

It has 3 parts - Parameters, Arrow tokens, Body.

```java
List<String> list = Arrays.asList("Java", "Spring");
list.forEach(name->System.out.println(name));
```

It works in Functional Interfaces example Runnable, Callable, Comparator.

Java 8 introduces `@FunctionalInterface` to
enforce it.

```java

@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

// Lambda implementation
MathOperation addition = (a, b) -> a + b;
System.out.println(addition.operate(5, 3)); // Output: 8
```

Before Java 8 there was anonymous inner class to implement functional interface.
```java
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running in a thread");
    }
};
new Thread(runnable).start();
```
New one.
```java
Runnable runnable = () -> System.out.println("Running in a thread");
new Thread(runnable).start();
```
**Difference between lambda and anonymous class.**

Lambda - Short ideal for single method implementations of functional interfaces like Runnable, Callable, Comparator.
`Runnable r = () -> System.out.println("Lambda Example");`
It donot create a new class file. It use invoke dynamic bytecode instruction for functional interface implementation.
Lambda expressions work only with functional interfaces, they can't be used to extend classes or implement multiple methods.
Lambda in inline comparator.
```java
Collections.sort(names, (o1, o2) -> o2.compareTo(o1)); // Reverse alphabetical order
System.out.println(names); // Output: [Zara, John, Jane, Adam]

Collections.sort(names, Comparator.reverseOrder());
```


Anonymous class - Verbose code. It is used to implement interfaces with multiple methods or extends classes.
```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Anonymous Class Example");
    }
};
```
Anonymous class extend class.
```java
class Animal {
    void sound() {
        System.out.println("Some generic animal sound");
    }
}

Animal dog = new Animal() {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
};
dog.sound(); // Output: Dog barks
```
Anonymous class in sorting comparaators.
```java
import java.util.*;

List<String> names = Arrays.asList("John", "Jane", "Adam", "Zara");

Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return o2.compareTo(o1); // Reverse alphabetical order
    }
});

System.out.println(names); // Output: [Zara, John, Jane, Adam]
```
It creates a separate inner class file at runtime.

### Stream API.

Provides a functional approach to process collections, making operations like filtering, mapping and reduction easier.

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 4, 56, 6, 7, 78, 89);
list.stream().filter(x->x%2==0).forEach(System.out::println);
```
**Default and static method in interfaces.**
```java
interface MyInterface {
    default void show() {
        System.out.println("Default Method");
    }
}
```

### Optional Class.

Provides a container to handle nullable values and avoid NullPointerException.
```java
Optional<String> optional = Optional.ofNullable("Hello");
optional.ifPresent(System.out::println);

```
It encourages functional programming like map, filter, ifPresent.
```java
// Empty optional.
Optional<String> empty = Optional.empty();
// Non Empty Optional.
Optional<String> name = Optional.of("John");
// Optional with Nullable value.
Optional<String> nullableName = Optional.ofNullable(null);
```

Methods in Optional.

isPresent() and ifPresent()
```java
Optional<String> name = Optional.of("John");

// Check if value is present
if (name.isPresent()) {
    System.out.println("Name: " + name.get());
}

// Perform action if value is present
name.ifPresent(value -> System.out.println("Name: " + value));
```
orElse() and orElseGet() and orElseThrow()
```java
String value = nullableName.orElse("Default Name");
System.out.println(value); // Output: Default Name

String lazyValue = nullableName.orElseGet(() -> "Generated Default Name");
System.out.println(lazyValue);

String value = nullableName.orElseThrow(() -> new IllegalArgumentException("Value is missing!"));

```
get(), filter(), map() and flatMap().
```java
String nameValue = name.get(); // It throws NoSuchElementException.

Optional<String> filtered = name.filter(n -> n.startsWith("J"));
filtered.ifPresent(System.out::println); // Output: John

Optional<Integer> length = name.map(String::length);
length.ifPresent(System.out::println); // Output: 4

Optional<Optional<String>> nestedOptional = Optional.of(Optional.of("Nested Value"));
Optional<String> flattened = nestedOptional.flatMap(value -> value);
flattened.ifPresent(System.out::println); // Output: Nested Value
```

Example of using Optional.
```java
public String getName(Person person) {
    if (person != null) {
        Address address = person.getAddress();
        if (address != null) {
            return address.getCity();
        }
    }
    return "Unknown";
}
```
Using Optional.
```java
public String getName(Person person) {
    return Optional.ofNullable(person)
                   .map(Person::getAddress)
                   .map(Address::getCity)
                   .orElse("Unknown");
}
```

### Date and Time API.
It is in java.time.Package It replaces the outdated `java.util.Date` and `java.util.Calendar` package.

```java
LocalDate today = LocalDate.now();
LocalTime now = LocalTime.now();
```

### Parallel Array Sorting.
Adds the Arrays.parallelSort() method for faster sorting using multiple thread.
```java
int[] array = {3, 2, 1};
Arrays.parallelSort(array);
```
### Adding new collector in Stream API.
Add utilities like `Collectors.toMap`, `Collectors.groupingBy`, and `Collectors.partitioningBy` for aggregations.
```java
Map<Boolean, List<Integer>> partitioned = numbers.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));
```
### Concurrency Enhancement.

Introduces CompletableFuture for Asynchronous programming.

```java
CompletableFuture.runAsync(() -> System.out.println("Running in a separate thread"));
```

### Base 64 encoding and decoding.

Provides utility classes for Base 64 encoding and decoding.
```java
String encoded = Base64.getEncoder().encodeToString("Java8".getBytes());
```
### Collection Hierarchy.


### Compare 2 json values.
The correct way to compare two JSON strings logically in Java is to parse them into structured objects (like Jackson’s JsonNode) and then perform a deep equality check, rather than relying on raw string comparison which fails due to differences in whitespace or key order.

```java
ObjectMapper mapper = new ObjectMapper();
JsonNode node1 = mapper.readTree(jsonString1);
JsonNode node2 = mapper.readTree(jsonString2);

boolean isEqual = node1.equals(node2); // true if logically equal
```
In Java, a JsonNode is a fundamental class from the Jackson library that represents a node in a JSON tree. Instead of treating JSON as raw text, Jackson parses it into a hierarchical tree structure where each element (object, array, field, value) is a JsonNode.  
Tree Model Representation: JSON is parsed into a tree of JsonNode objects, allowing traversal and manipulation.

Immutable Structure: Once created, a JsonNode is read-only. For modifications, you use ObjectNode or ArrayNode (mutable subclasses).

Type Awareness: Each node knows its type — object, array, string, number, boolean, or null.

Convenient Accessors: Methods like get(), path(), fields(), elements() let you navigate deeply nested JSON easily.
``` java
ObjectMapper mapper = new ObjectMapper();
JsonNode root = mapper.readTree(jsonString);

// Access fields
String name = root.get("name").asText();
int age = root.get("age").asInt();

// Navigate nested objects
JsonNode address = root.path("address");
String city = address.get("city").asText();
```


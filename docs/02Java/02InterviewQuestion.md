Java is designed to be platform-independent, meaning that code written in Java can run on any device or operating system that has a Java Virtual Machine (JVM) installed. This is achieved through the use of bytecode, which is an intermediate representation of the code that can be executed by the JVM, allowing developers to write code once and run it anywhere.

### What is the Java Virtual Machine (JVM) and how does it work?

The JVM is an abstract computing machine that enables a computer to run Java programs. It works by converting Java bytecode into machine code that can be executed by the host operating system. The JVM provides a runtime environment that includes memory management, garbage collection, and security features, allowing Java applications to run consistently across different platforms.

### What are the differences between JDK, JRE and JVM?

JDK - Toolkit for java development.
JRE - Environment to run Java program.
JVM - Engine that executes the Java bytecode.


### Anyways to Install the JDK without the JRE or does JDK already condenser required runtime parts ?

JDK already contains the required runtime parts to run the Java application and JDK is mainly for development but it also has runtime benefits and so we don't need separate JRE when JDK is installed.

- **JDK (Java Development Kit)**: It is a software development kit that provides tools for developing Java applications, including the JRE, compilers, and other development tools. It is used by developers to write, compile, and debug Java programs.
### Explain Java garbage collection process and how it helps in memory management.

Java garbage collection is an automatic memory management process that helps in reclaiming memory occupied by objects that are no longer in use. The garbage collector (GC) identifies and removes these unreferenced objects from the heap memory, freeing up space for new object allocations. This process helps prevent memory leaks and ensures efficient memory utilization.  

Memory leak happens when object are not needed but still some reference is pointing them. The garbage collector removes only unreachable objects. Garbage collector removes unreachable objects and in case the object is reachable then garbage collector will not remove it. 

### Which garbage collection algorithms are used for JVM to clean up on new subjects from the memory? 
JVM uses different garbage collection algorithms like mark and sweep, mark and compact, generational garbage collection. The actual algorithm depends on which value which collector you are using. 
### What is Inheritance?

Inheritance allows one class to inherit fields and methods from another class and promote code reuse. It model "is-a" relationship like the Manager is an Employee. 
```java
class User {
    String name;
    String email;

    User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    void login() {
        System.out.println(name + " logged in with email " + email);
    }

    void displayRole() {
        System.out.println("I am a generic user");
    }
}
class Admin extends User {
  Admin(String name, String email) {
    super(name, email);
  }

  @Override
  void displayRole() {
    System.out.println("I am an Admin, I manage users");
  }

  void manageUsers() {
    System.out.println("Managing users...");
  }
}

class Teacher extends User {
  Teacher(String name, String email) {
    super(name, email);
  }

  @Override
  void displayRole() {
    System.out.println("I am a Teacher, I create courses");
  }

  void createCourse() {
    System.out.println("Creating a new course...");
  }
}

class Student extends User {
  Student(String name, String email) {
    super(name, email);
  }

  @Override
  void displayRole() {
    System.out.println("I am a Student, I enroll in courses");
  }

  void enrollCourse() {
    System.out.println("Enrolling in a course...");
  }
}
public class InheritanceDemo {
  public static void main(String[] args) {
    User u1 = new Admin("Alice", "alice@company.com");
    User u2 = new Teacher("Bob", "bob@school.com");
    User u3 = new Student("Charlie", "charlie@student.com");

    u1.login(); u1.displayRole();
    u2.login(); u2.displayRole();
    u3.login(); u3.displayRole();
  }
}

```
There are many type of Inheritance - Single Inheritance - One subclass extends one superclass.  
Multilevel inheritance - A class extends another class which itseld extends a third.  
Hierarchical Inheritance - Multiple subclasses extend a single superclass.  
Multiple inhertitance with interface - Achieved via interface, class extends multiple parents not possible.

Inheritance is applicable in IS-A relationship (Manager IS-A Employee) and HAS-A relationship (Car HAS-A Engine) prefer composition.  

Polymorphism: Inheritance enables runtime polymorphism via overriding.

Access Modifiers: Subclasses inherit non-private members. The difference between public and protected is important. Subclass can use public and protected members not private.      
public → Inherited by subclasses. It is accessible everywhere(same package, different package, subclasses, external classes).  
protected → Inherited by subclasses. It is accessible in the same package and by subclasses (even in different packages).  
default/package-private → Inherited only within the same package. Not accessible outside the package, even by subclasses.   
private - Not inherited. Accessible only within the same class. Subclasses cannot directly access private members.

Diamond Problem: Java avoids multiple class inheritance to prevent ambiguity. Interfaces solve this safely.


### Why does Java allow multiple inheritance via interfaces but not via classes?
Multiple class inheritance is not possible - It causes ambiguity.

When 2 parent classes have the same method signature and a child class inherits from both, it is unclear which method to call. This is known as the "Diamond Problem." 
```java
class A {
    void show() { System.out.println("A"); }
}

class B {
    void show() { System.out.println("B"); }
}

// ❌ Not allowed in Java
// class C extends A, B { }
```
Multiple Interface solves the issue as Interface are contracts and it tells what must be done not how. There is no state conflict. When there are two interface have default method s with the same signature Java forces to resolve it explicitly.
```java
interface A {
    default void show() { System.out.println("A"); }
}

interface B {
    default void show() { System.out.println("B"); }
}

class C implements A, B {
    @Override
    public void show() {
        // Explicit resolution
        A.super.show();
        B.super.show();
        System.out.println("C resolves the diamond problem");
    }
}
```

### What is Polymorphism ?
Polymorphism allows methods to perform different tasks based on the object that act upon, implemented through method overloading and overriding.

### What is the difference between == and equals() methos in Java?
== verifies the references points to the same object in memory and equals() verifies the content of the object.
### What is the static keyword in Java?
The static keyword indicates that a member belongs to the class rather than an instance of the class. Static methods and variables can be accessed without creating an object of the class.
### What is the constructor in Java and how it is different from a method?
A constructor initializes a new object and has no return type.
Constructors are called automatically when an object is created.
### What is method overloading?
Method of loading allows multiple methods in a class to have the same name but with different parameter lists.
It is compile time polymorphism offering flexibility when defining methods.
### What is Abstract class in Java?
An abstract class cannot be instantiated and it's meant to be subclassed. It may contain both abstract method or a concrete method. Abstract classes are used to define a base for other classes.
### What is the difference between an abstract class and an interface in Java?
An interface is a reference type that can contain abstract methods constant default methods and static methods.  
Unlike an abstract class the class can have multiple interfaces. Abstract classes can have constructors and instance fields whereas interface cannot.

### What is the Difference between final finally and finalize in Java next thing  
Final is a keyboard used to declare constant prevent method of writing or prohibit class inheritance  
Finally the block that ensures execution after a try catch whether or not an exception occurs  
Finalize is a method called by the garbage collector before reclaiming an object's memory 

### Explain the Exception Handling in Java?
It is an **event** that occurs during the execution of the program. When executing the program the event will disrupt the program normal flow.
When an exception happens the runtime system creates an Exception object which contains the information about the error like - 
Its type of exception and message.  
Stack trace.  

Runtime system use this exception object and find the class which can handle it. When exception comes then internally one exception object is created.

Example - Program starts from main. Main calls Method 1 -> Method 2 -> Method 3. There is an exception in Method 3 then internally it will create an Exception Object.

Now the runtime use the Exception Object and it will check the class that can handle the exception.

First it will check Method 3 if it can handle the exception. Then it will go to Method 2 to handle the exception. Similarly it will ask main. If the exception is not handled then the runtime will terminate the program abruptly.

```java
public class ExceptionHandling {
    public static void main(String[] args) {
        method1();
    }
    private static void method1(){
        method2();
    }
    private static void method2() {
        method3();
    }
    private static void method3() {
        int a = 8/0;
    }
}
```
The output will look like.
```bash
Exception in thread "main" java.lang.ArithmeticException: / by zero
        at designPattern.ExceptionHandling.method3(ExceptionHandling.java:20)
        at designPattern.ExceptionHandling.method2(ExceptionHandling.java:16)
        at designPattern.ExceptionHandling.method1(ExceptionHandling.java:12)
        at designPattern.ExceptionHandling.main(ExceptionHandling.java:8)
```
The first line in the output shows the exception and the next lines shows the stack trace. Stack Trace - Starting the place where exception to the main.  
When exception happened in one method say 3 then it see the method declaring the method 3 say method 2 if it can handle then good else it will go to the method that declared method 2 say method 1.


<img src="/images/Java/ExceptionHierarchy.png">
Object is parent of all and its child class is Throwable and it has Error and Exception.  
Error - You cannot control. Like OutOfMemoryError, StackOverflowError. These are related to JVM issue. Like JVM can not able to create any new object in heap and heap is full then OutOfMemoryError. Error is unchecked exception as it will compile when running I will get the out of memory error. Error will be in runtime.

```java
public class ExceptionHandling {
    public static void main(String[] args) {
        String[] arr = new String[900000000 * 90000000 * 900000000 * 90000000];
    }
}
//The output 
// Exception in thread "main" java.lang.OutOfMemoryError: Java heap space at designPattern.ExceptionHandling.main(ExceptionHandling.java:9)
```
Error is JVM issue and we cannot able to control that. Exception is on the basis of our code. We can handle Exception.
### What is the difference between checked and unchecked exceptions in Java?  
Checked exceptions must be declared in the method signature or handled using a try catch block like IO exception.
Unchecked exceptions like NullPointerException do not need to be declated or explicitly caught.

### Give a scenario when to create a custom checked exception and when to create a custom unchecked exception in Java?
- **Custom Checked Exception**: Create a custom checked exception when you want to enforce the caller to handle the exception explicitly. For example, if you are developing a library that interacts with a database, you might create a custom checked exception like `DatabaseConnectionException` to indicate that the connection to the database failed. This forces the caller to handle the exception, ensuring that they are aware of the potential issue and take appropriate action. The custom checked exception is created when we want to enforce error handling by the caller of the method and when the error is recoverable and can be handled gracefully in response to the exception.
- **Custom Unchecked Exception**: Create a custom unchecked exception when the error is a result of a programming mistake or a condition that should not be recoverable. For example, if you are developing a utility class that processes user input, you might create a custom unchecked exception like `InvalidUserInputException` to indicate that the input provided by the user is invalid. This allows the caller to choose whether to handle the exception or let it propagate, as it is considered a programming error that should be fixed rather than handled. The custom unchecked exceptions are usually created for programming errors that the application itself should catch where no specific recovery action is expected from the caller.

### How the exception propagation works in Java?
When an exception occurs in a method, the runtime system searches for an appropriate exception handler in the current method. If it doesn't find one, it propagates the exception to the calling method, and this process continues up the call stack until a suitable handler is found or the program terminates. 
Example - Method 1 calls Method 2, which calls Method 3. If an exception occurs in Method 3 and is not handled there, it propagates to Method 2, and if not handled there, it propagates to Method 1, and so on.

### How do you handle a centralised exception handling in Spring?
Global exception handling in Spring can be achieved using the `@ControllerAdvice` annotation along with `@ExceptionHandler` methods. This allows you to define a centralized exception handling mechanism that applies to all controllers in your application. You can create a class annotated with `@ControllerAdvice` and define methods annotated with `@ExceptionHandler` to handle specific exceptions and return appropriate responses. 
Example
```java
// Centralized exception handler
@ControllerAdvice
public class GlobalExceptionHandler {

  // Handle custom ResourceNotFoundException
  @ExceptionHandler(ResourceNotFoundException.class)
  public ResponseEntity<String> handleResourceNotFound(ResourceNotFoundException ex) {
    return ResponseEntity
            .status(HttpStatus.NOT_FOUND)
            .body("Resource not found: " + ex.getMessage());
  }

  // Handle IllegalArgumentException
  @ExceptionHandler(IllegalArgumentException.class)
  public ResponseEntity<String> handleIllegalArgument(IllegalArgumentException ex) {
    return ResponseEntity
            .status(HttpStatus.BAD_REQUEST)
            .body("Invalid input: " + ex.getMessage());
  }

  // Handle all other exceptions (fallback)
  @ExceptionHandler(Exception.class)
  public ResponseEntity<String> handleGeneralException(Exception ex) {
    return ResponseEntity
            .status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body("An unexpected error occurred: " + ex.getMessage());
  }
}
```
`@ControllerAdvice` to centralize exception handling across all controllers.
Each `@ExceptionHandler` method maps a specific exception type to a structured HTTP response.
For example, `ResourceNotFoundException` returns a `404 NOT_FOUND, IllegalArgumentException` returns a `400 BAD_REQUEST`, and any unhandled exception falls back to a `500 INTERNAL_SERVER_ERROR`.
This ensures consistency in error responses and keeps controller code clean.

### Explain how to design a fault-tolerant system using Java exception handling for distributed system.
Use the resilience patterns like Retry, Circuit Breaker, and Fallback to handle transient failures in distributed systems. Implement custom exceptions to represent specific failure scenarios and use centralized exception handling to log errors and provide meaningful responses. For example, when a service call fails due to a network issue, you can retry the operation a few times before falling back to a default response or an alternative service.

### What is the difference between retries, fallbacks and circuit breakers?
- **Retries**: Automatically re-attempting a failed operation a specified number of times before giving up. Useful for transient failures like network timeouts.
- **Fallbacks**: Providing an alternative response or behavior when the primary operation fails. This ensures that the system can continue to function even when a specific service is unavailable.
- **Circuit Breakers**: Monitoring the success and failure rates of operations and temporarily halting requests to a failing service to prevent cascading failures. Once the service is deemed healthy again, the circuit breaker allows requests to resume.


### What is multithreading in Java ?
Multithreading allows multiple throws to execute simultaneously maximizing the Super utilizations and it can be implemented by extending the thread class or implementing through another interface.

### What is the purpose of the Super keyboard in Java?
The Super keyword refers to the immediate parent class it is used to access the parent class method constructor of variable that are hidden by the chat class.

### What is the difference between String, StringBuilder and StringBuffer in Java?
- String is immutable, meaning once created, its value cannot be changed.
- StringBuilder is mutable and not synchronized, making it faster for single-threaded operations.
- StringBuffer is mutable and synchronized, making it thread-safe but slower than StringBuilder.


### What is the difference between throw and throws in Java?
- `throw` is used to explicitly throw an exception in a method or block of code.
- `throws` is used in a method signature to declare the exceptions that a method can throw

### What is teh Singleton design attern in java?

The single independent ensures that a class has only one instance and provides a global point of access to it. This is typically implemented with the private constructor and a static instance variable.

### What is the difference between wait and sleep in Java?

Wait is used in multithreading to pause the current thread until it's notified and it releases the monitor lock.  
Sleep pauses the current thread for a specified time but does not release any locks.

### What is Java collection framework?
Java collection framework is a set of interfaces and class for storing and processing group of objects common examples like set them up list and the implementation and get a list and  hashmap.

### What is the purpose of the volatile keyboard in Java?
The virtual keyboard ensures that a variable's value is always read from the main memory not from the thread's local cache. It helps maintain a consistency in multithreaded environment.

### What is the thread in Java how it is different from process ?
A thread is a lightweight sub process the smallest unit of CPU scheduling. Multiple threads within the same process shared memory while processes are independent and do not share memory.

### What is teh difference between notify and notifyAll in Java?
Notify wakes up a single thread that is waiting on the object's monitor, while notifyAll wakes up
    all threads that are waiting on the object's monitor. Notify is used when only one thread needs to be awakened, while notifyAll is used when multiple threads may need to proceed.

### What is the difference between break and continue in Java?
- `break` is used to exit a loop or switch statement prematurely, terminating the current iteration and moving control to the next statement after the loop or switch.  
  - `continue` is used to skip the current iteration of a loop and proceed to the next iteration, allowing the loop to continue 
  
### What is the difference between a shallow copy and a deep copy in Java?
- A shallow copy creates a new object but copies references to the original object's fields, meaning changes to mutable fields in the original object will affect the shallow copy.  
  - A deep copy creates a new object and recursively copies all fields, ensuring that the new object is completely independent of the original, including any mutable objects it contains.      

### What is the difference between call by value and call by reference in Java?
- Java uses call by value for primitive data types, meaning that a copy of the value is passed to methods, and changes to the parameter do not affect the original variable.  
  - For objects, Java uses call by reference for object references, meaning that a copy of the reference is passed, allowing methods to modify the object's state, but the reference itself cannot be changed to point to a different object.

### What is the difference between a static method and an instance method in Java?
- A static method belongs to the class and can be called without creating an instance of the class
  - An instance method belongs to an object and can only be called on an instance of the class, allowing it to access instance variables and methods.   

### What is the difference between a static variable and an instance variable in Java?
- A static variable is shared among all instances of a class and belongs to the class itself, while an instance variable is unique to each instance of the class and belongs to the object. 

### What is the difference between a static block and an instance block in Java?
- A static block is executed when the class is loaded and is used for static initialization, while  
an instance block is executed when an instance of the class is created and is used for instance initialization.

### What is the difference between a static nested class and an inner class in Java?    
- A static nested class is a static member of the outer class and can be instantiated without an instance of the outer class, while an inner class is associated with an instance of the outer class and can access its members directly.   

### What is the difference between a final class and an abstract class in Java?
- A final class cannot be subclassed, meaning no other class can extend it, while an    abstract class is intended to be subclassed and can contain abstract methods that must be implemented by its subclasses.

### What is the difference between a final method and an abstract method in Java?
- A final method cannot be overridden by subclasses, ensuring that its implementation remains unchanged, while an       abstract method is declared without an implementation and must be implemented by subclasses, allowing for polymorphic behavior. 

### What is the difference between a final variable and a static variable in Java?
- A final variable is a constant whose value cannot be changed once assigned, while a static variable       is shared among all instances of a class and can be modified, but it belongs to the class rather than any specific instance.

### What is the difference between a final variable and an instance variable in Java?
- A final variable is a constant whose value cannot be changed once assigned, while an instance variable    is unique to each instance of a class and can be modified, allowing each object to maintain its own state.  

### What is the difference between equals() and hashCode() methods in Java?
- The `equals()` method is used to compare the contents of two objects for equality, while the `hashCode()` method returns an integer representation of the object's memory address or a computed hash value.  
### What is the difference between callable and runnable in Java?
- A `Runnable` is an interface that represents a task that can be executed by a thread, but it does not return a result or throw checked exceptions.  
  - A `Callable` is a similar interface that represents a task that can be executed by a thread, but it can return a result and throw checked exceptions, making it more flexible for concurrent programming.


### What is the difference between synchronized and lock in Java?

The synchronized keyword provides a special mechanism to control access to critical sections. Lock is more flexible and powerful mechanism from the Java.util.concurrent package offering additional features like time locks and interruptible locks.

### What is deadlock in java?

Adidas cockles went two or more threads are waiting for each other's resources causing them to remain in a waiting state for a wrapper it is a commonly threatening issue that should be avoided.

### What is reflection in Java?
Reflection is a feature that allows a program to inspect and modify its own structure and behaviour at runtime. It is often used for dynamic method invocation and inspecting classes methods and fields.

### What is the difference between comparator and comparable in Java?
- The `Comparable` interface is used to define the natural ordering of objects by implementing the `compareTo()` method, allowing objects to be sorted based on their inherent properties.  
- The `Comparator` interface is used to define custom ordering of objects by implementing the `compare()` method,
  - allowing for multiple sorting criteria and flexibility in sorting objects that do not have a natural order.

### What is eh fork/join framework in Java?
The Fork/Join framework is a parallel programming framework introduced in Java 7 that allows for efficient.

### What is the executor framework in Java?
The Executor framework is a high-level API introduced in Java 5 that provides a way to manage and control thread execution, allowing developers to submit tasks for execution without having to manage thread creation and lifecycle.

### What are Java annotations and how are they used in Java programming?
Java annotations are metadata that provide additional information about the code to the compiler or runtime environment. They are used to influence the behavior of the code, provide documentation, and enable frameworks to process the annotated elements. Annotations can be applied to classes, methods, fields, parameters, and other program elements.
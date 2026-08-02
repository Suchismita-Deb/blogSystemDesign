### What is Java and why it is platform independent?

It is a high-level, object-oriented programming language developed by Sun Microsystems (now owned by Oracle Corporation). Java is designed to be platform-independent, meaning that code written in Java can run on any device or operating system that has a Java Virtual Machine (JVM) installed. This is achieved through the use of bytecode, which is an intermediate representation of the code that can be executed by the JVM, allowing developers to write code once and run it anywhere.

### What is the Java Virtual Machine (JVM) and how does it work?

The JVM is an abstract computing machine that enables a computer to run Java programs. It works by converting Java bytecode into machine code that can be executed by the host operating system. The JVM provides a runtime environment that includes memory management, garbage collection, and security features, allowing Java applications to run consistently across different platforms.

### What are the differences between JDK, JRE and JVM?

JDK - Toolkit for java development.
JRE - Environment to run Java program.
JVM - Engine that executes the Java bytecode.

- **JDK (Java Development Kit)**: It is a software development kit that provides tools for developing Java applications, including the JRE, compilers, and other development tools. It is used by developers to write, compile, and debug Java programs.
### Explain Java garbage collection process and how it helps in memory management.

Java garbage collection is an automatic memory management process that helps in reclaiming memory occupied by objects that are no longer in use. The garbage collector (GC) identifies and removes these unreferenced objects from the heap memory, freeing up space for new object allocations. This process helps prevent memory leaks and ensures efficient memory utilization.

### What is Inheritance?

Inheritance allows one class to inherit fields and methods from another class and promote code reuse.

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


### What is the difference between checked and unchecked exceptions in Java?  
Checked exceptions must be declared in the method signature or handled using a try catch block like IO exception.
Unchecked exceptions like NullPointerException do not need to be declated or explicitly caught.

### What is multithreading in Java ?
Multithreading allows multiple throws to execute simultaneously maximizing the Super utilizations and it can be implemented by extending the thread class or implementing through another interface.

### What is the purpose of the Super keyboard in Java?
The Super keyword refers to the immediate parent class it is used to access the parent class method constructor of variable that are hidden by the chat class.

### What is the diffeerence between String, StringBuilder and StringBuffer in Java?
- String is immutable, meaning once created, its value cannot be changed.
- StringBuilder is mutable and not synchronized, making it faster for single-threaded operations.
- StringBuffer is mutable and synchronized, making it thread-safe but slower than StringBuilder.

### What is the difference between HashMap and HashTable in Java?
Hashmap is not synchronised meaning its faster but not threads safe and hash table is synchronised making it thread safe but slower additionally hashmap allows null key and values while hash table does not.

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

### What is the trade in Japan how it is different from process ?
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
- The `equals()` method is used to compare the contents of two objects for equality, while the `hashCode()` method returns an integer representation of the object's memory address or a computed hash value    

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
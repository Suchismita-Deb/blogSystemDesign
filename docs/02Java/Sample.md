**Securing the application** - Using APIGEE to secure the connection of the pods. Server to server using OAuth 2.
**Feature deployment** - the feature available to specific user to see the work.

**What encapsulation work in real code base.**  
It protects the object internal state from unintended modifications. Example when a bank account balance field is public then another field may change the value to negative. So we will be able to write a validation in setter that we will not be adding any negative value.


**In your project how are you using the principle of object oriented programming.**
There are many abstract class base class and with the help of builder we are making the objects.
Record and sealed class are introduced in Java 17.

**Feature introduced in Java 21 and not in Java 17.**

Overloading resolve at compile time or runtime. 
Overriding in run time.
**API versioning. Deploy the updated API to user.**

Deploy like the location basis then 100k in specific location the user the user name hashmap to get the range.

**Design pattern you work - Factory, Builder.**

**There are 2 method getData(String s) and getData(Object obj) - When null is passed to the 2 overloaded method which one is called.**

When passing the value null then there will be ambiguity and the compiler will not understand which method to call. It will be compile time error.


**Example when you used super and this keyword?**
Can a constructor call an overridable instance method?
Yes but its risky - the method is already overridden the subclass will be executes before the subclass constructor completes.

**Can an abstract class have a constructor if it is never be instantiated.**

**Differences between abstract class and interface.**

**Spring version.**

**Spring Projects** - what are thing used like spring boot spring security.
There is a legacy code how would you update? Create a new API.
**What the @ComponentScanAnnotation do in package scanning? What will be the problem in case the main is not in the root package.**

Give project structure.

Create a custom starter that other application will use it.   Spring data jpa starter.

Can a user be authenticated but not authorized for a specific access?

What are the roles and authorities in spring security.

What are the use case in synchronous and asynchronous design patter?
What is teh orchestration and choreography microservcie design pattern.

Git branch 30 commits and need only 5-6 commits then merge only the 6 commit and they are not in the master branch. How to do it?

Cherry pick the hashcode.


Does polymorphism applies to private and static method.  
No. They are hidden. Runtime polymorphism work in overriden instance method. Private method are not inherited and static method are resolve at compile time using reference type. So dynamic method dispatch does not apply to private and static method.

Explain solid principle to real use case.

Polymorphism.

A class implements2 interface having the same default method. What will happen? The class must override the method and provide its own implementation. The compiler will not understand which method to use so it will give compile time error. The solution is to implement.

Design true immutable class.

How springboot understand what to configure.
Springboot analyse the class path, existing beana and configuration properties. There might be conditinal properties like @ConditionOnClass.

how to disable a specific auto configuration class in springboot.

We use @ExcludeAutoConfiguration or spring.autoconfigure.exclude property in application.properties.
### Java Lambda.

Pass function as parameter.
```java

@Data
public class Hotel{
    public int pricePerNight;
    public int rating;
    public HotelType hotelType;
}
public enum HotelType{
    BUDGET, LUXURY, BUSINESS
}

@Service
public class HotelService{
    List<Hotel> hotels = new ArrayList<>();
    public HotelService(){
        hotels.add(new Hotel(100, 3, HotelType.BUDGET));
        hotels.add(new Hotel(200, 4, HotelType.BUSINESS));
        hotels.add(new Hotel(300, 5, HotelType.LUXURY));
    }
}

```

Why StringBuilder and not stringBuffer?

Stringbuffer slow nut thread safe.

A webserver handling 1000s of request that involves string manipulation. Which one to use StringBuilder or StringBuffer?

StringBuilder - A web server that changes the string in a single thread. Faster - its non-synchronized mutable nature. StringBuffer - A web server that changes the string in multiple threads.

Why String immutable in Java? Name predefined immutable class in Java.

Integer, String, BigDecimal, BigInteger, LocalDateTime, LocalDate, LocalTime, ZonedDateTime, Instant, Duration, Period, UUID.
What security thread arise when the string is not immutable in multithreaded env?

What is teh difference between == and equals() in Java?
== - It checks the reference equality. It checks if both references point to the same object in memory.  
equals() - It checks the value equality. It checks if the values of the objects are the same. The equals() method can be overridden in a class to provide custom equality logic based on the object's state. For example, two different String objects with the same sequence of characters will be considered equal when using equals(), but they will not be the same object in memory, so == will return false.


What happens internally when we concat 2 string using the + operator in Java?

When you use the + operator to concatenate two strings in Java, the compiler internally converts it into a StringBuilder (or StringBuffer in older versions) operation. Here's what happens step by step:
1. **StringBuilder Creation**: The compiler creates a new instance of StringBuilder.
2. **Append Operations**: It appends the first string to the StringBuilder, then appends the second string.
3. **Conversion to String**: Finally, it converts the StringBuilder back to a String using the `toString()` method.
For example, the expression:
```java
String result = "Hello, " + "World!";
```
is internally transformed by the compiler to:
```java
StringBuilder sb = new StringBuilder();
sb.append("Hello, ");
sb.append("World!");
String result = sb.toString();
```
What is Serialization in Java? How to make a class serializable?
Serialization in Java is the process of converting an object into a byte stream, which can then be saved to a file, sent over a network, or stored in a database. This allows the object's state to be preserved and reconstructed later. To make a class serializable, you need to implement the `java.io.Serializable` interface. This is a marker interface, meaning it does not contain any methods. It simply indicates that the class can be serialized. Here's an example:

What happens when a class does not implement a serializable interface and we try to serialize it and the object is serialized?

When we try to implement a object of class that does not implement the Serializable interface, the Java runtime will throw a `java.io.NotSerializableException`. This exception indicates that the class is not eligible for serialization. The serialization process requires that the class explicitly declare its intent to be serializable by implementing the `Serializable` interface. If this is not done, any attempt to serialize an instance of that class will fail, and the object cannot be converted into a byte stream for storage or transmission.

How to prevent certain fields in the class from being serialized?
When you want to prevent certain fields in a class from being serialized, you can use the `transient` keyword. Fields marked as `transient` will not be included in the serialization process, meaning their values will not be saved when the object is serialized. Here's an example:

```java
import java.io.Serializable;        
public class User implements Serializable {
    private String username;
    private transient String password; // This field will not be serialized

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    // Getters and setters
}
```
In this example, the `password` field is marked as `transient`, so when an instance of the `User` class is serialized, the `password` field will not be included in the byte stream. When the object is deserialized, the `password` field will be initialized to its default value (null for objects, 0 for numeric types, false for boolean, etc.).

How to make a class immutable?
The class is declared final, so it cannot be subclassed. All fields are private and final, ensuring they can only be set once. The constructor initializes all fields, and no setter methods are provided, preventing modification after construction. The access to the object is limited to getter methods, which return the values of the fields without allowing any changes. If the class contains mutable objects, defensive copies are made in the constructor and getter methods to prevent external modification.

The advantages and disadvantage of immutable object?
Advantage - Thread safety. The immutable objects are inherently thread-safe, as their state cannot be changed after creation. This eliminates the need for synchronization when accessing immutable objects in a multi-threaded environment. 
Disadvantage - Memory overhead. Creating new instances of immutable objects for every change can lead to increased memory usage, especially if the objects are large or frequently modified. This can result in higher garbage collection overhead and potential performance issues in memory-constrained environments.

Why String boot and not spring?

In the application when there are so many dependencies then how the dependency injection helps in development and maintenance? How the IOC improves the modularity and testability of the application? How the dependency injection helps in decoupling the components of the application?

IOC provides modularity by providing loose coupling between components. Components dont directly instantiate or operate or directly depends on each other. Instead, they rely on abstractions and interfaces, which allows for easier swapping of implementations without affecting the rest of the system. This modularity makes it easier to maintain and extend the application over time and enhance testability as the components can be tested in isolation with mock implementation.

What are the type of dependency injections supported in Spring framework?

Why the constructor injection is preferred over setter injection? When to use setter injection?

Constructor injection is preferred over setter injection because it ensures that the dependencies are provided at the time of object creation, making the object immutable and ensuring that all required dependencies are available. This leads to better encapsulation and makes the class easier to test. Setter injection, on the other hand, allows for optional dependencies and can lead to partially constructed objects if not handled carefully. Setter injection is typically used when a dependency is optional or when circular dependencies exist.

Constructor injection is preferred when the dependency is mandatory and should be provided at the time of object creation like user db,access object. It ensures that the object is always in a valid state and reduces the risk of null pointer exceptions. Setter injection is preferred when the dependency is optional or when there are circular dependencies between beans, allowing for more flexibility in configuring the beans.

What is the use of @RestControllerAnnotation in Spring Boot? How is it different from @Controller?
The `@RestController` annotation in Spring Boot is a specialized version of the `@Controller` annotation. It is used to create RESTful web services and combines the functionality of `@Controller` and `@ResponseBody`. When a class is annotated with `@RestController`, it indicates that the class is a controller where every method returns a domain object instead of a view. The `@ResponseBody` annotation is automatically applied to all methods, meaning that the return values of the methods will be serialized directly into the HTTP response body, typically in JSON or XML format.

HTTP methos?
GET,PUT,POST,Delete.

How the Spring MVC handle file upload and what configuration are needed?

Spring MVC provides built-in support for handling file uploads through the use of the `MultipartFile` interface. To enable file upload functionality, you need to configure a multipart resolver in your Spring configuration. Here's how you can handle file uploads in Spring MVC:
1. **Add Dependencies**: Ensure you have the necessary dependencies in your `pom.xml`
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
</dependency>
<dependency>
    <groupId>commons-io</groupId>           
    <artifaceId>commons-io</artifactId>
</dependency>
```
2. **Configure Multipart Resolver**: In your Spring Boot application, you can configure the multipart resolver by adding the following properties to your `application.properties` file:
```properties
spring.servlet.multipart.enabled=true
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```
3. **Create a Controller Method**: Create a controller method to handle the file upload.
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.http.ResponseEntity;

@RestController
@RequestMapping("/api/files")
public class FileUploadController {
    @PostMapping("/upload")
    public ResponseEntity<String> uploadFile(@RequestParam("file") MultipartFile file) {
        // Handle the file upload logic here
        String fileName = file.getOriginalFilename();
        // Save the file to the server or process it as needed
        return ResponseEntity.ok("File uploaded successfully: " + fileName);
    }
}
```
4. **Handle File Storage**: Implement the logic to store the uploaded file on the server or process it as needed. You can save the file to a specific directory or store it in a database, depending on your application's requirements.
5. **Testing the Upload**: You can test the file upload functionality using tools like Postman or curl. Make a POST request to the `/api/files/upload` endpoint with a file attached in the request body.
With these steps, you can handle file uploads in a Spring MVC application. The `MultipartFile` interface provides methods to access the file's content, name, and other metadata, allowing you to process the uploaded files as needed.

How do you test an API?
How do you secure the REST API?


What is JWT token?
Tokens contains the user information and the claims. The token is signed using a secret key or a public/private key pair. The server can verify the token's authenticity and integrity by checking the signature. The token is usually sent in the HTTP Authorization header as a Bearer token. The server can then extract the user information and claims from the token to authenticate and authorize the user for specific actions or resources.    


Why maven clean before maven install?

What is the way microservice communicate with each othe?

Eureka service discovery, API gateway, REST API, gRPC, message broker (Kafka, RabbitMQ), event-driven architecture, and asynchronous messaging. 

Java 8 theoretical. Java 8 features lambda, stream.

Design Pattern Code.  
Singleton Design Pattern.  
Stream API.
Spring Data JPA.

Multithreading executor framework.  
Comparator and comparable.
HashMap internal working. Hashcode and equals method.
Why string immutable.
Microservice design pattern.

HR Round.

OOPS and strong design pattern. Spring - IOC, Bean, AOP, Spring Data JPA, Spring Security.
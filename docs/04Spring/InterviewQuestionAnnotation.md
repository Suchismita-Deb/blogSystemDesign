### Autowiring.

#### What is Autowiring in Spring?

Autowiring is a feature in the Spring Framework that enables the automatic injection of dependencies into a bean. Instead of explicitly configuring the dependencies in a Spring configuration file, the container automatically resolves and injects them based on a specified strategy.

**Uses**.

**Reduces Boilerplate Code** - Eliminates the need to manually specify bean dependencies.  
**Simplifies Configuration** - Container automatically manages relationships between beans.  
**Improves Readability** - Makes the code cleaner and easier to maintain.

Types of Autowiring Modes.

**no (Default)**  
No autowiring is performed. Dependencies must be explicitly defined using property or constructor-arg.
```xml
<bean id="userService" class="com.example.UserService">
<property name="userRepository" ref="userRepository" />
</bean>
```
**byName**  
Autowires a bean by matching its property name with a bean name in the configuration.
```xml
<bean id="userService" class="com.example.UserService" autowire="byName" />
<bean id="userRepository" class="com.example.UserRepository" />
```
**byType**  
Autowires a bean if a single bean of the matching type exists in the container.
```xml
<bean id="userService" class="com.example.UserService" autowire="byType" />
<bean id="userRepository" class="com.example.UserRepository" />
```
**constructor**  
Autowires dependencies by matching constructor parameters with bean types in the container.
```xml
<bean id="userService" class="com.example.UserService" autowire="constructor" />
<bean id="userRepository" class="com.example.UserRepository" />
```
**autodetect (Deprecated in Spring 4.3)**  
Spring attempts to autowire using constructor. If that fails, it falls back to byType

Modern Spring applications prefer annotations over XML for autowiring.

`@Autowired` - Automatically injects the required bean by type.  
`@Qualifier` - Resolves conflicts when multiple beans of the same type exist by specifying the bean name.  
`@Primary` - Marks a bean as the primary candidate for autowiring when multiple beans of the same type exist.

Tradeoffs of Autowiring.

**Ambiguities** - Can cause issues when multiple beans of the same type exist.  
**Hidden Dependencies** - Makes it harder to track bean relationships.  
**Testing Challenges** - Autowired dependencies may complicate unit testing.

#### What is @RestController = @Controller + @ResponseBody.

@ResponseBody Meaning the return type is the http response.
@RequestParamter - In the path there is ? and & and contain the value.
@RequestBody - Data in the body will be mapped to the dto.



### How to implement a centralized exception handling mechanism in spring Boot.

## Spring profiling.



```markmap
## Spring Topics
### Spring Structure.
### Spring Annotations.
### System Scalability.
### Operations and logging.
### Security and monitoring.
### API and database.
```

### How the annotation is created in Java? What is the way to create a custom annotation in Java?

In Spring Boot, you create a custom annotation using Java's `@interface`.

**Basic custom annotation**
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}
```
@interface → declares your custom annotation.
@Target(ElementType.METHOD) → annotation can be applied only to methods.
@Retention(RetentionPolicy.RUNTIME) → annotation is available at runtime, so Spring/AOP can read it.

We use the annotion in the Service.
```java
@Service
public class UserService {

   @LogExecutionTime
   public void createUser() {
   // logic
   }
}
```
**Annotation with parameters.**  
You can define attributes inside the annotation.
```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {

   String value() default "";
}
```
Usage.
```java
@LogExecutionTime("User creation")
public void createUser() {
}

// Alternate way to use default value.

@LogExecutionTime(value = "User creation")
public void createUser() {
}
```
**Reading the annotation.**  
To read it using Java reflection.
```java
Method method = UserService.class.getMethod("createUser");

if (method.isAnnotationPresent(LogExecutionTime.class)) {
    LogExecutionTime annotation = method.getAnnotation(LogExecutionTime.class);
    System.out.println(annotation.value());
}
```
**Spring Boot + AOP — the common real-world use**
Custom annotations become particularly useful with Spring AOP.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}
# Then create an aspect:
@Aspect
@Component
public class LoggingAspect {

   @Around("@annotation(LogExecutionTime)")
   public Object logExecutionTime(ProceedingJoinPoint joinPoint)
   throws Throwable {

        long start = System.currentTimeMillis();

        Object result = joinPoint.proceed();

        long end = System.currentTimeMillis();

        System.out.println(
            joinPoint.getSignature().getName()
            + " took "
            + (end - start)
            + " ms"
        );

        return result;
   }
}

// The service to use the annotation.
   
@Service
public class UserService {
   @LogExecutionTime
   public void createUser() {
   // business logic
   }
}
```
Whenever `createUser()` is called through Spring's bean, the aspect intercepts it and measures the execution time.
Important distinction - There are two separate concepts.
```java
@Target(...)
@Retention(...)
public @interface MyAnnotation {
}
```
It defines the annotation.
```java
@Aspect
@Component
public class MyAspect {
}
```

It defines the behavior that happens because of the annotation.
## Spring Boot Actuator and Health.


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


### What is the difference between retries, fallbacks and circuit breakers?
- **Retries**: Automatically re-attempting a failed operation a specified number of times before giving up. Useful for transient failures like network timeouts.
- **Fallbacks**: Providing an alternative response or behavior when the primary operation fails. This ensures that the system can continue to function even when a specific service is unavailable.
- **Circuit Breakers**: Monitoring the success and failure rates of operations and temporarily halting requests to a failing service to prevent cascading failures. Once the service is deemed healthy again, the circuit breaker allows requests to resume.
### How the `@Autowired`, `@Resource` and `@Inject` differs from each other.

Used for dependency injection and they differs in terms of usage, behaviour and source.

| `@Autowired`| `@Resource`|`@Inject`|
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
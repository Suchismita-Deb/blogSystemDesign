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



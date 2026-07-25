<div class="quiz-box">
<b>Possible to overload 2 methods only by changing the return type while keeping the same parameter?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
No we cannot overload a method only by changing the return type in Java method overloading depends on the parameter and the number of parameter types and order must be different.

When we call an overloaded method Java compiler verifies the argument that we are passing is this the number of arguments and the type of argument and then completely select the method.
</details>
</div>

<div class="quiz-box">
<b>How is the method overloading connected to the polymorphism in Java?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Method of overloading is connected with compile time polymorphism - In this case same method name can do different work based on parameter.
</details>
</div>

<div class="quiz-box">
<b>The static method be overridden in Java or they are hidden?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
No static method cannot be overridden in Java . Study method belongs to the class center to the object. 

In case you write the same static method in giant class then it is called method hiding and not method overriding. 
</details>
</div>
<div class="quiz-box">
<b>What happens in case a superclass method is overridden by more than one subclass in java?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
In this many subclasses overwrite the same method from the parent class then every subclass will have its own version of that method and at runtime Java will call the method based on the actual object.
</details>
</div>


<div class="quiz-box">
<b>What will happen if we create the main method in Java without the static keyword? </b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
JVM will not be able to start the program JVM calls the main method without creating an object so without the main method being static it will be not able to start.
</details>
</div>

<div class="quiz-box">
<b>Can you overload the main method in Java by changing the parameters of the main method ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
The main method is static and the main method similarly cannot be overridden in any other place. To do method overriding child class should have same method name, same parameter and same return type. The method in channel in parent class at runtime.
Yes we can overload the main method in Java we can create more main methods with different parameters but JP will only look at the standard main method which is static and within a parameter of String args[].
To perform the method overloading the method name should be same but the parameter should be different so we can call number of parameters type of parameters or any order of the parameter to be different and changing the return type without any other change is not enough.
</details>
</div>

<div class="quiz-box">
<b>What will happen if you write a return statement inside a try or catch block while the final block still run or will it be skipped ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
The final block will still run even if the return statement is there in the trial the catch block finally will be executed before the method actually returns.
</details>
</div>

<div class="quiz-box">
<b>Can a study block through an exception while the class is loading? </b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Yes starting block and throw an exception but it should be handled properly if exceptions is not handled class loading can fail and program can stop with error.
</details>
</div>

Autoboxing - Wrapper to Primitive.
Unboxing - Primitive to wrapper.
In the auto boxing from wrapper to primitive say the wrapper integer is having value null in it to convert it to primitive it will be error sometimes autoboxing and gives error.

<div class="quiz-box">
<b>Anyways to Install the JDK without the JD or Dust JDK already condenser required runtime parts ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
JDK already contains the required runtime parts to run the Java application and JDK is mainly for development but it also has runtime benefits and so we don't need separate JRE when JDK is installed.
</details>
</div>

<div class="quiz-box">
<b>Which garbage collection algorithms are used by JVM to clean our newest objects from memory ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
The remaining garbage collection algorithms like Mark and sweep, mark and compact and generational garbage collection.
</details>
</div>

<div class="quiz-box">
<b>Java already has automatic garbage collection and how memory it happens in Java application ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Memory can happen when objects are not needed but they are still referenced for example garbage collector can only remove those of the seats and not reachable you talk you have to stay with each other garbage collector will not remove it.
</details>
</div>
<div class="quiz-box">
<b>Why should we use getter and setter method when we can directly make variables public and access them ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Getter and setter gives control over data we can add validation before setting any values and also we can add internal logic later without changing other code.
</details>
</div>
The top class cannot be private or protected it should be default or public.  
 We can use private constructor in Java only in case of singleton class or utility class. Private constructor stops other classes from creating objects directly.

<div class="quiz-box">
<b>how to create singleton class so hat one object of that class can be created?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
To create single term class we make constructor private. Create one private static object of the same class and then provide the public static method to run that object.
</details>
</div>




<div class="quiz-box">
<b>What problem can happen in Singleton classic 2 threads try to create the objective at the same time ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Singleton is not threat safe, two threads can create two different objects of the same class at the same time. To make it threat safe we can use synchronized keyword in the method, synchronized block to eager initialization.
</details>
</div>

<div class="quiz-box">
<b>Why are immutable objects considered safe and useful when multiple threads are working together ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Immutable objects cannot be changed after creation so many threads can use the same object safely.
To create an immutable class make the class final and the fields private and final and do not provide any setter, initialise values using constructor and in case field is mutable object return copy instead of original object.

The final keyboard help in making the opportunity mutable and safe for multiple threads.  When a variable is declared is assigned as final then it cannot be changed. When it cannot be changed it is safe to share with multiple threads.
</details>
</div>


<div class="quiz-box">
<b>What happens if a class contains even one abstract method inside it ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
If a glass is even one abstract method then the class must be declared abstract. We cannot create object of the abstract class directly. Child class has to implement the abstract method.
</details>
</div>

<div class="quiz-box">
<b>Why does Java not support multiple inheritance using glasses ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Java multiple entherence with classes because it can create confusion if two parent classes have the same method a Java will not know which method to use. This is called diamond problem.
</details>
</div>

<div class="quiz-box">
<b>When should you use an interface and when should you extend an existing class </b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Interface when we want to defend rules that different laws should follow.  
Use class extention when we want to reuse the code from the parent class.
</details>
</div>

<div class="quiz-box">
<b>Interface in Java with static method and how do we call those static methods </b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Yes interface can have static methods and we can call them using the interface name we do not interpret objects to call the static methods.
</details>
</div>

Functional interface can implement another interface the only catch here is that it total it should be only one abstract method there should not be 2 abstract method.

<div class="quiz-box">
<b>What is dynamic method dispatch and how does a job decides which overridden method to call at runtime ?</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Dynamic method dispatch means Java decides at runtime which overridden method should run.  
It depends on the actual object not on the reference type. It is runtime polymorphism.
</details>
</div>




























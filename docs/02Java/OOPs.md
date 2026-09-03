Java is not 100% public oriented programming language because it uses primitive data type like int, boolean, long, float.

<div class="quiz-box">
<b>Method Overloading and Method Overriding.</b>
<details class="quiz-toggle">
<summary>Reveal Answer</summary>
Method Overloading - Same method name with different parameter(compile time polymorphism). <br>
Method Overriding - Redefining a parent class method in a subclass with the same signature(runtime polymorphism).


</details>
</div>

<br><br>
<b>Overloading</b> - The parameter number, type or order should differ. The return type alone cannot distinguish the method. It is decided at compile time to class the method based on the signature(parameter) of the method.

```java

static int sum(int a, int b) {
    return a + b;
}
static int sum(int a, int b, int c) {
    return a + b + c;
}

static double sum(double a, double b) {
    return a + b;
}
```

Overriding - Same method name, same parameter and same return type. The access level should not be stricter than the parent class method. It is decided at runtime the decision like which version of the method to execute is done at the runtime. 
```java
// Parent class
class Calculator {
    public int sum(int a, int b) {
        System.out.println("Parent class sum:");
        return a + b;
    }
}

// Child class
class AdvancedCalculator extends Calculator {
    @Override
    public int sum(int a, int b) {
        System.out.println("Child class overridden sum:");
        return a + b + 10;
    }
}

```
### How is the method overloading connected to the polymorphism in Java?
Method of overloading is connected with compile time polymorphism - In this case same method name can do different work based on parameter.

### The static method be overridden in Java or they are hidden?
No static method cannot be overridden in Java . Study method belongs to the class center to the object. 

In case you write the same static method in giant class then it is called method hiding and not method overriding. 

### What happens in case a superclass method is overridden by more than one subclass in java?
In this many subclasses overwrite the same method from the parent class then every subclass will have its own version of that method and at runtime Java will call the method based on the actual object.

### What will happen if we create the main method in Java without the static keyword? 
JVM will not be able to start the program JVM calls the main method without creating an object so without the main method being static it will be not able to start.

Can you overload the main method in Java by changing the parameters of the main method ?
The main method is static and the main method similarly cannot be overridden in any other place. To do method overriding child class should have same method name, same parameter and same return type. The method in channel in parent class at runtime.
Yes we can overload the main method in Java we can create more main methods with different parameters but JP will only look at the standard main method which is static and within a parameter of String args[].
To perform the method overloading the method name should be same but the parameter should be different so we can call number of parameters type of parameters or any order of the parameter to be different and changing the return type without any other change is not enough.

### What will happen if you write a return statement inside a try or catch block while the final block still run or will it be skipped ?
The final block will still run even if the return statement is there in the trial the catch block finally will be executed before the method actually returns.

### Can a study block through an exception while the class is loading? 
Yes starting block and throw an exception but it should be handled properly if exceptions is not handled class loading can fail and program can stop with error.

Autoboxing - Wrapper to Primitive.
Unboxing - Primitive to wrapper.
In the auto boxing from wrapper to primitive say the wrapper integer is having value null in it to convert it to primitive it will be error sometimes autoboxing and gives error.


### Which garbage collection algorithms are used by JVM to clean our newest objects from memory ?
The remaining garbage collection algorithms like Mark and sweep, mark and compact and generational garbage collection.

### Why should we use getter and setter method when we can directly make variables public and access them ?
Getter and setter gives control over data we can add validation before setting any values and also we can add internal logic later without changing other code.
The top class cannot be private or protected it should be default or public.  
 We can use private constructor in Java only in case of singleton class or utility class. Private constructor stops other classes from creating objects directly.

### Can we declare a top level class as private and protected in Java or is this only allowed for inner classes? 
No top level classes cannot be private or protected. Top level class can only be public or default and inner classes can be private or protected.
### how to create singleton class so that one object of that class can be created?
To create single term class we make constructor private. Private constructor stops other class from creating object directly. Create one private static object of the same class and then provide the public static method to run that object.

### What problem can happen in Singleton classic 2 threads try to create the objective at the same time ?
Singleton is not threat safe, two threads can create two different objects of the same class at the same time. To make it threat safe we can use synchronized keyword in the method, synchronized block to eager initialization.

### Why are immutable objects considered safe and useful when multiple threads are working together ?
Immutable objects cannot be changed after creation so many threads can use the same object safely.
To create an immutable class make the **class final** and **the fields private and final** and **do not provide any setter, initialise values using constructor** and in case field is **mutable object return copy instead** of original object.

The final keyboard help in making the opportunity mutable and safe for multiple threads.  When a variable is declared is assigned as final then it cannot be changed. When it cannot be changed it is safe to share with multiple threads.

### What happens if a class contains even one abstract method inside it ?
If a class is even one abstract method then the class must be declared abstract. We cannot create object of the abstract class directly. Child class has to implement the abstract method.


### Why does Java not support multiple inheritance using classes ?
Java multiple inheritance with classes because it can create confusion if two parent classes have the same method a Java will not know which method to use. This is called diamond problem.

### When should you use an interface and when should you extend an existing class.
Interface when we want to defend rules that different laws should follow.  
Use class extention when we want to reuse the code from the parent class.

### Interface in Java with static method and how do we call those static methods.
Yes interface can have static methods and we can call them using the interface name we do not interpret objects to call the static methods.

Functional interface can implement another interface the only catch here is that it total it should be only one abstract method there should not be 2 abstract method.

### What is dynamic method dispatch and how does a job decides which overridden method to call at runtime ?

Dynamic method dispatch means Java decides at runtime which overridden method should run.  
It depends on the actual object not on the reference type. It is runtime polymorphism.

### How the inheritance and polymorphism works together?


### What happens internally when you create an object using the new keyword?

When an object is created using a new keyword Java does a few steps behind the scene - 
Memories allocated - Java creates the space in the heap memory for the new objects.  
Default values are set - all variable of the objects are given divorced values like 0 for the numbers false for booleans and all for objects. 
Constructor is called - the constructor runs and it sets the value we pass and run and any setup code.
Reference is returned - the address and the reference of this object is written and stored in the variable. 

### Can we create an object without using the new keyword? 
Yes there are several methods like using clone, reflection, deserialization, factory method.

### Can a class be declared without any variable or methods? 
Yes the class can be declared without any variables or method Java still creates this dot class file and it still has a default constructor and we can create this object. When we need a class just to represent its type so we create an empty class like this and this is also called as marker class.

### What is the difference between an object and object reference? 
Object and object reference are not the same thing period object is the real data stored in the heap and created using the new keyword whereas object reference is a variable that stores the address of the object, stores in the stack memory and used to access the object.  
Example if we have Student s = new object(). s is the reference and its point to an actual object in the heap.  

### Does assigning null to a reference delete the opposite? 

No assigning null to a reference does not directly delete that object. Example `Student s = new Student()` and when we do s=null then the reference is removed and the object is still there but unreachable. The object is not immediately destroyed garbage collector will remove it later. 

### Can a class have only static members? If yes then it still be object oriented? 
Yes a class can have only static members but conceptually object oriented is having object with data + behavior and static members belong to the class and not to the object and this type of class is more procedural style than pure object oriented.


### Encapsulation is only about making variables private? 
No encapsulation is not only about making variables private it means keeping the data safe by making variable private and controlling how it is accessed by getter and setter.  

Example If a class has private fields but public sectors then it is actually partially encapsulated and not truly encapsulated. If setter method is allowing any variable it means data is not really protected. 
True encapsulation is validation and business rules in the settle method. For example if there is field age and set age method then we shouldn't put any negative value in the set age attribute so that validation needs to be there in the setter method and that's what makes it a truly encapsulated class.

### What are anonymous class? 
An anonymous class is a class without a name. It is created and used at the same time usually when we need a small piece of custom behaviour only once. Instead of creating a separate class files we would write the class directly where we needed. For example we want to handle a button click. Instead of making a new class like MyButtonHandler we write the code directly in the button click listener.


### What is the difference between encapsulation and abstraction? 
Encapsulation meaning hiding the data and controlling access to it. We wrap the data (variable) and the code (method) together in the class and decide who can access them. For example the bank account hide the balance variable and allow changes through deposit or withdrawal methods.  

Abstraction meaning hiding internal working and showing only what is needed. User knows what an object does not how it does. Example when we use a remote TV we press a button to change the channel we do not know the internal electronics. In simple word encapsulation protects data and abstraction hides the complexity.

### Why do we need to use abstract class when you already have interfaces? 
Abstract classes and interfaces would support abstractions but they are different and used in different cases. 
We should use episode class when classes are closely related and share code. Example There is a vehicle class, car and bike are extending it. Car and Bike and share the common methods in vehicle like accelerate().

We should use interface when many unrelated classes need to follow the same rule. For example we use payment interface and every payment type has a unique implementation like cardPayment, UPIPayment, bankingPayment they may not share the code.
In Java 8 default, private and static method were introduced and two or more classes can reuse the default method but still in interface we do not have constructed an instance variable. 

In the Java 8 if we need constructor and instance variable we must use abstract classes otherwise the interface.

### Why an interface does not have an instance variable? 
Interface is an contract and not an objective globin instance variable belongs to objects instances interface does not create objects. So it cannot hold any object state.
The variable in the interface are public, static and final by default.

### Why default, static and private methods are introduced in interface after Java 8? 

Default method- in the application if we add a new method in interface before Java 8 all existing class would break so that's very different methods were introduced. We have this new method as default in interface without breaking an existing functionality. 

Static method - Static methods are just helper methods. They avoid creating extra utility classes. 
 
Private method - They were introduced in Java nine. They are used to share common logic between default methods and avoid duplications.

### Why Java supports multiple inheritance using interface but not classes? 
Java does not allow multiple inheritance with classes because it can create confusion and errors. If two parent classes have the same method or variable name that child class will not know which one to use. This is called the diamond problem period 
Java allows multiple inheritance with interfaces because interfaces mainly define the method name and not the full executable method code. So there is less confusion example even there are two interfaces with the same method that joint class must write its own implementation so Java clearly knows which code to use.

### Can any interface exist without any methods? 
Yes it is called marker interface. A marker interface is an interface in Java that has no methods. It is used to marker class with some special meaning. Example serializable and clonable. 
### What is a functional interface?

Interface becomes a functional interface where it has exactly 1 abstract method. This single method represents the main work that the interface is supposed to do and because there is only one such method it can easily be used as Lambda expression. If any interface has more than one abstract method then it cannot be called as functional interface.

The functional entities can have default methods and static methods. 

### Why the @FunctionalInterface optional but recommended?

The compiler can automatically detect whether interface has only one abstract method. It is recommended because it will clearly tell other developers that this interface is functional interface and it will protect the code by giving compile time error in case if anyone tries to add another abstract method.

### How to default methods let an interface evolve without breaking existing implementing classes ?
Adding a method into the interface everyone has to implement it.
Default method let up add a method wth a body directly in the interface.
The entire implementing classes compiled and worked without being forced override.   


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

### What is the purpose of the Super keyboard in Java?
The Super keyword refers to the immediate parent class it is used to access the parent class method constructor of variable that are hidden by the chat class.



### What is teh Singleton design pattern in java?

The single independent ensures that a class has only one instance and provides a global point of access to it. This is typically implemented with the private constructor and a static instance variable.






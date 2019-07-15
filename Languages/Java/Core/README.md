- [Source File Declaration Rules](#Source-File-Declaration-Rules)
- [Data Types](#Data-Types)
  - [Primitive Types](#Primitive-Types)
    - [Enum](#Enum)
      - [values()](#values)
  - [Non-Primitive (Reference) Types](#Non-Primitive-Reference-Types)
    - [String](#String)
      - [Concat](#Concat)
      - [Comparison](#Comparison)
      - [Converting to string](#Converting-to-string)
    - [Array](#Array)
    - [Number Wrapper Classes](#Number-Wrapper-Classes)
    - [Character Wrapper Classes](#Character-Wrapper-Classes)
- [Variable Types](#Variable-Types)
- [Modifiers](#Modifiers)
  - [Access Modifiers](#Access-Modifiers)
  - [Non-Access Modifiers](#Non-Access-Modifiers)
- [Methods](#Methods)
  - [Anonymous Methods (Lambda Expressions)](#Anonymous-Methods-Lambda-Expressions)
- [Classes](#Classes)
  - [Inheritance](#Inheritance)
    - [super keyword](#super-keyword)
    - [instanceof operator](#instanceof-operator)
  - [Overriding](#Overriding)
    - [Virtuality](#Virtuality)
  - [Polymorphism](#Polymorphism)
    - [Method Overloading](#Method-Overloading)
  - [Encapsulation](#Encapsulation)
    - [Static Nested Class](#Static-Nested-Class)
    - [Non-static Nested Classes](#Non-static-Nested-Classes)
      - [Inner Class](#Inner-Class)
      - [Method-Local Inner Class](#Method-Local-Inner-Class)
      - [Anonymous Inner Class](#Anonymous-Inner-Class)
- [Interfaces](#Interfaces)
- [Generics](#Generics)
  - [Bounded Generic Types](#Bounded-Generic-Types)
- [Packages](#Packages)
  - [import](#import)
  - [CLASSPATH](#CLASSPATH)
- [Handling Exception](#Handling-Exception)
  - [Try Catch Block](#Try-Catch-Block)
  - [Try with Resource Statement](#Try-with-Resource-Statement)
  - [Throwing Exception](#Throwing-Exception)
  - [User Defined Exceptions](#User-Defined-Exceptions)
- [Multithreading](#Multithreading)
  - [Main Thread](#Main-Thread)
  - [Lifecycle](#Lifecycle)
  - [Priority](#Priority)
  - [Thread Class](#Thread-Class)
    - [Construct](#Construct)
    - [Sleep](#Sleep)
    - [Join](#Join)
  - [Synchronization](#Synchronization)
    - [Synchronized Method](#Synchronized-Method)
    - [Synchronized Block](#Synchronized-Block)
  - [Thread Communication](#Thread-Communication)
    - [Object.wait()](#Objectwait)
    - [Object.notify()](#Objectnotify)
    - [Object.notifyAll()](#ObjectnotifyAll)
  - [Example](#Example)
- [IO Operations](#IO-Operations)
- [Collections](#Collections)
  - [Accessing Elements](#Accessing-Elements)
    - [Iterator](#Iterator)
    - [ListIterator](#ListIterator)
    - [for-each Loop](#for-each-Loop)

# Source File Declaration Rules

These rules are applied for:
* declaring classes
* import statements and 
* package statements in a source file.

Rules:

* One public class per source file.
* Can have multiple non-public classes.
* Public class name and source file name have to be matched.
  * Class name: Employee
  * File name: Employee.java
* Package statement must be the first statement in the source file.
* Import statement must be written between the package statement and the class declaration.
* Import and package statements affect to all source file (for ex. multiple classes)




# Data Types

## Primitive Types
type | storing | min | max | default | example
--- | --- | --- | --- | --- | ---
byte | 8 bit signed | -128 |  127 | 0 | byte b = 5
short | 16 bit signed | -32768 |  32767 | 0 | short s = 6
int | 32 bit signed | -2 ^ 31  |  (2 ^ 31) - 1 | 0 | int i = 7
long | 64 bit signed | -2 ^ 63  |  (2 ^ 63) - 1 | 0 | long l = 8
float | 32 bit floating point | - |  - | 0.0f | float f = 9.10
double | 64 bit floating point | - |  - | 0.0d | double d = 11.12
boolean | single bit | 0 |  1 | false | boolean b = true;
char | 16 bit character | '\u0000'  | '\uffff' | - | char c = 'a'

### Enum
* Java Enumerations can be thought as classes that have fixed set of constants.
* Enumerations can have:
  * instance variables
  * constructors
  * methods
* Each enumeration constant is public, static and final by default. 
* Enumerations may implement many interfaces
* Enumerations cannot extend any class because it internally extends **java.lang.Enum** class.
* Enumerations are not instantiated using new keyword.

// TODO: needs more info

```java
enum Season { WINTER, SPRING, SUMMER, FALL };

Season s = Season.SUMMER;
```

#### values()
* returns an array containing all the values of the enum.



## Non-Primitive (Reference) Types
* Class objects
* Default is **null**
* Some of them is like below:

### String
* String class is immutable, so it cannot be changed.
* If you want to do make changes on string you should use:
  * StringBuffer
    * It represents growable and writable character sequence.
    * Thread-safe
  * StringBuilder
    * Not thread-safe
    * Fast
* See Also:
  *  [https://docs.oracle.com/javase/7/docs/api/java/lang/String.html](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html)
  *  [https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html](https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html)
  *  [https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html](https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuffer.html)


#### Concat
* **String.concat()** method
* **\+** operator

#### Comparison
* **String.equals()** method.

#### Converting to string
* **String.valueOf()** method.

```java
int x = 345;
String xStr = String.valueOf(x);
```

### Array
* definition:

```java
Integer[] varName = {1, 2, ...};
```

```java
Integer[] varName = new Integer[10];
```

* has length property
```java
for (int i = 0; i < varName.length; i++0) {
    System.out.println(varName[i] + " ");
}
```

### Number Wrapper Classes
![Number Classes](./number_classes.jpg)

* All the wrapper classes (Integer, Long, Byte, Double, Float, Short) are subclasses of the abstract class Number.
* All the wrapper classes are immutable. So it cannot be changed.
* See Also [https://docs.oracle.com/javase/7/docs/api/java/lang/Number.html](https://docs.oracle.com/javase/7/docs/api/java/lang/Number.html)

### Character Wrapper Classes
* All the wrapper classes are immutable. So it cannot be changed.
* See Also [https://docs.oracle.com/javase/7/docs/api/java/lang/Character.html](https://docs.oracle.com/javase/7/docs/api/java/lang/Character.html)






# Variable Types
* Local variables
* Instance variables
  * Value can be assigned during the declaration.
* Class/Static variables
  * define with **static** keyword
  * Access with **class name**, not instance
  * <pre> public static int name = 1; </pre>
* Constant variables
  * define with **static final** keywork 
  * Access with **class name**, not instance
  * <pre> public static final int NAME = 1; </pre>



# Modifiers
## Access Modifiers
* **Private** can be accessible only within the **class**.
* **Default**(empty modifier) can be accessible only within the **same package**.
* **Public** can be accesible from **everywhere**.
* Access Table:


. | default | private | protected | public
--- | --- | --- | --- | --- |
Same Pkg | Yes | No | Yes | Yes
Different Pkg & Subclass | No | No | Yes | Yes
Different Pkg & Non-Subclass | No | No | No | Yes

## Non-Access Modifiers
* static
  * Local variables cannot be declared static.
  * Method/Variable of the instance that static method belongs to can not be used within the static method.
* final
  * final variable means its value can not be changed.
  * final method cannot be overrided at subclasses.
  * final class cannot be a parent class.
* abstract
  * abstract class 
    * connot be instantiated.
    * cannot be final.
    * can contain method with body.
    * can contain constructor and static methods.
    * can contain overloaded abstract methods.
  * abstract method 
    * does not contain body, must be implemented at subclass.
    * connot be declared in non abstract class.
    * cannot be final, private, static.
* synchronized
  * synchronized method can be accessible from just one thread at a time.
* transient
  * transient variable value doesn't persist when an object is serialized.
* volatile
  * tells the compiler that the volatile variable can be changed unexpectedly by other parts of your program.



# Methods
* Can be overloaded. 
  * Same name same arg count with different type of args.
  * Same name different arg count.
* JDK 1.5 enables you to pass a variable number of arguments of the same type to a method.

```java
void printInt (int... args) {
    for (int i = 0; i < args.length; i++) {
        System.out.print(args[i] + " ");
    }
}
```

## Anonymous Methods (Lambda Expressions)
// TODO: this will be written asap.






# Classes
* Class can be public, private.
* Class can be final, abstract.
* Can extend **just one** class.
* Can implement **any number** of interface.
* Each .java file can contain **just one** public class, **any number** private class.
* Source file name **must** match the public class name.
* Constructor has to have same name with the class.

```java
// class definition
public class Employee {

    // class variable (static)
    private static int unique = 0;

    // instance variable
    private int id;
    private String name;

    // constructor
    public Employee (String name) {
        this.id = this.unique++;
        this.name = name;
    }

    // constructor chaining
    public Employee () {
        this("unnamed");
    }

    // destructor
    protected finalize () {
        // do clean-up before object destruction
    }

    public int getId () {
        return this.id;
    }

    public String getName () {
        return this.name;
    }

    // file main function
    public static void main (String[] args) {
        // local variable
        Employee[] employees = {
            new Employee("Emp 0"),
            new Employee("Emp 1"),
            new Employee("Emp 2")
        };
        
        for (int i = 0; i < employees.length; i++) {
            System.out.println(employees[i].getId() + " - " + employees[i].getName());
        }
    }
}
```


## Inheritance
![Types of Inheritance](./types_of_inheritance.jpg)

* Java **does not support** multiple inheritance.
* Can be done with **extends** keyword for classes.
* Can be done with **implements** keyword for interfaces.

```java
class Base {
}

public class Derived extends Base {
}
```

### super keyword
* call base class constructor with **super()**
* access base class members with **super.memberName**

```java
class Base {
    Base (String msg) {
        System.out.println("This is base class constructor.");
        this.printMsg(msg);
    }

    public void printMsg (String msg) {
        System.out.println("Msg:" + msg);
    }
}

public class Derived extends Base {
    Derived () {
        super("Hello base.");
        System.out.println("This is derived class constructor");
    }

    protected finalize () {
        super.printMsg("Goodbye base.");
    }
}
```



### instanceof operator
* objects that instantiate from subclass are returns true of the below statements:

```java
Derived obj = new Derived();
System.out.println(obj instanceof Base); // true
System.out.println(obj instanceof DerivedBase); // true
```



## Overriding
* Method signature must be same at child class.
  * Return type can be subtype of the original method's return type.
* Overridden method must not be static.
* Overridden method must not be final.
* Overridden method must not be private.
* Overridden method must not be constructor.
* Method accessiblity must not be more restrictive than the overridden method.
* Original method can call with:

```java
...
@Override
public void methodName () {
    super.methodName();
    // do other stuff
}
...
```

### Virtuality
* Compile time: reference time will be checked
* Runtime: object type will be checked

```java
class Animal {
    public void walk () {
        System.out.println('Animal walks');
    }
}

class Dog extends Animal {
    @Override
    public void walk () {
        System.out.println('Dog walks');
    }

    public static void main (String[] args) {
        Dog d1 = new Dog();
        // this line is IMPORTANT!
        Animal d2 = new Dog();

        d1.walk(); // prints: Dog walks
        d2.walk(); // prints: Dog walks
    }
}
```

## Polymorphism

### Method Overloading
* Same method name, different argument types or count.

```java
class Math {
    public Integer sum (Integer a, Integer b) {
        return a + b;
    }
    public Float sum (Float a, Float b) {
        return a + b;
    }
}

```

## Encapsulation

### Static Nested Class
* It is defined like a regular static class member.
* It can be accessed without instantiating the outer class.
* Does not have access to instance members of outer class.

```java
public class Outer {
    static class Nested_Demo {
        public void my_method() {
            System.out.println("This is my nested class");
        }
    }
   
    public static void main(String args[]) {
        Outer.Nested_Demo nested = new Outer.Nested_Demo();	 
        nested.my_method();
    }
}
```

### Non-static Nested Classes

#### Inner Class
* Normal class definition inside another class.
* Can be private
* Private inner class can be accessed from outer class.
* Inner class can access to outer class (even privates)

```java
class Outer_Demo {
    // inner class
    private class Inner_Demo {
        public void print() {
            System.out.println("This is an inner class");
        }
    }
   
    // Accessing he inner class from the method within
    void display_Inner() {
        Inner_Demo inner = new Inner_Demo();
        inner.print();
    }
}
```
#### Method-Local Inner Class
* Defined inside the method block scope.
* Can be only instantiated inside the method.

```java
public class Outerclass {
    // instance method of the outer class 
    void my_Method () {

        // method-local inner class
        class MethodInner_Demo {
            public void print() {
                System.out.println("This is method inner class ");	   
            }   
        }
	   
        MethodInner_Demo inner = new MethodInner_Demo();
        inner.print();
    }
   
    public static void main(String args[]) {
        Outerclass outer = new Outerclass();
        outer.my_Method();	   	   
    }
}
```

#### Anonymous Inner Class
* Generally used to override the method of an exist class.

```java
abstract class AnonymousInner {
    public abstract void mymethod();
}

public class Outer_class {

    public static void main(String args[]) {
        AnonymousInner inner = new AnonymousInner() {
            public void mymethod() {
                System.out.println("This is an example of anonymous inner class");
            }
        };
        inner.mymethod();	
    }
}
```





# Interfaces
* It is like an abstract class.
  * All methods are **implicitly** abstract.
  * Interface is **implicitly** abstract.
* Can contain:
  * static final variables
  * default methods with body
  * static methods with body
  * nested types
* Can not contain:
  * constuctor
  * instance variables
  * public abstract methods
* File naming is same exactly with class files.
* Can not instatiate.
* Interfaces are **implement**ed by class, not extend.
* Can extend multiple interfaces.
* If interface that is implemented by class is not declare all methods that belongs the interface, the class must be defined as abstract.
* Method signatures have to be matched.
* A class can implement multiple interfaces at a time.
* An interface can extend another interface with using **extends** keyword.

```java
// Filename : Animal.java
interface Animal {
    public void eat();
    public void travel();
}

// Filename : FlyingAnimal.java
// extending interface
interface FlyingAnimal extends Animal {
    public void fly();
}

// Filename : Bird.java
// implementing multiple interface
class Bird implements FlyingAnimal, Event {
    Bird () {
    }

    public void eat() {
        System.out.println('Bird eats');
    }

    public void travel() {
        System.out.println('Bird travels');
    }

    public void fly () {
        System.out.println('Bird flies');
    }

    public static void main (String[] args) {
        Bird b = new Bird();
        b.eat();
        b.travel();
    }
}
```



# Generics
* Same as C++ template classes.
* Generics work **only with object types**. Primitive types are not allowed.
  * You can use Integer, Char etc. classes instead of primitive data types.
* Generics work with interface.
* Generic typed array is not allowed.
* Generic data type can be more than one.
* Generic class example:

```java
// Filename: CustomCollection.java
import java.util.ArrayList;

public class CustomCollection <T> {
    private ArrayList<T> _storage = new ArrayList<>();

    public CustomCollection() {
    }

    public CustomCollection<T> add (T element) {
        this._storage.add(element);
        return this;
    }

    public int size () {
        return this._storage.size();
    }
}

// Filename: App.java
public class App
{
    public static void main(String[] args)
    {
        CustomCollection<Integer> myList = new CustomCollection<>();
        myList.add(1).add(2).add(3);

        System.out.println(myList.size());
    }
}
```

* Generic method example:

```java
// Filename: App.java
public class App
{
    public static <T> void print(T msg) {
        System.out.println("[" + (new Date()).toString() + "] " + msg);
    }

    public static void main(String[] args)
    {
        App.print("Custom print");
    }
}
```

## Bounded Generic Types
* Specify the parameter type.
* Generic type can be extended class of the bounded type.
* use **extends** keyword.

```java
// Filename: App.java
public class App
{
    public static <T extends String> void print(T msg) {
        System.out.println("[" + (new Date()).toString() + "] " + msg);
    }

    public static void main(String[] args)
    {
        App.print("Custom print"); // valid
        App.print(5); // invalid
    }
}
```

# Packages

```java
package pkg;
package dir1.dir2.dir3; // dir1/dir2/dir3
package com.apple.phone; // com/apple/phone
```

* It is a way of categorizing the classes and interfaces. 
* It is like a **namespace** in C++;
* Prevents name conflicts.
* Groups related elements. (class, interface, enumaration and annotation)
* Package name should be lowercase to prevent conflict with class/interface.
* Package name structure and package directory structure must be matched.
* All classes within the package must have the package statement as its first line

## import
* accessing class from another package with these:

```java
// Filename: Boss.java
package payroll;
public class Boss {
    public void payEmployee(Employee e) {
        e.mailCheck();
    }
}

// Filename: Example.java
package example;

// use one of these
import payroll.*;
import payroll.Boss;

public class Example {
    public static void main (String[] args) {
        Boss b2 = new Boss();

        // or that
        payroll.Boss b1 = new payroll.Boss();
    }
}
```

## CLASSPATH
* classpath is an system variable.
* may include multiple path
* the compiler and JVM will look for .class files in:
  * CLASSPATH/package_name_to_dir_path
  * Ex: 
    * CLASSPATH = /home/yigit/javatest/project1/src/main/java
    * package_name_to_dir_path = com.yigityuce.project1
  * Result:
    * /home/yigit/javatest/project1/src/main/java/com/yigityuce/project1



# Handling Exception
![](./exception-class-hierarchy-java.jpg)

* **Exception** class is for exceptional conditions that program should catch. This class can be extended to create **user specific** exception classes.
* **RuntimeException** is a subclass of Exception. Exceptions under this class are automatically defined for programs.
* **Exceptions of type Error** are used by the Java run-time system to indicate errors having to do with the run-time environment, itself. Stack overflow is an example of such an error.


## Try Catch Block

```java
class ExceptionTutorial
{
    public static void main (String args[])
    {
        try {
            int a = 0;
            int b = 10;
            int c = b/a;
        }
        catch (ArithmeticException e) {
            System.out.println("Divided by zero");
        }
    }
}
```

## Try with Resource Statement
* Since JDK 7.
* Referred as **automatic resource management**.
* A resource is an object that is used in program and must be **closed after the program** is finished.
* Any object that implements **java.lang.AutoCloseable** or **java.io.Closeable**, can be passed as a parameter to try statement.

```java
class Test
{
    public static void main(String[] args)
    {
        try (BufferedReader br = new BufferedReader(new FileReader("./myfile.txt")))
        {
            String str;
            while ((str = br.readLine()) != null) {
                System.out.println(str);
            }
        }
        catch (IOException ioe) { System.out.println("exception");  }
    }
}
```

## Throwing Exception

* Exceptions can be thrown with **throw \<exception-instance\>** 
* Thrown exceptions must be defined in function signature with **throws** keyword.

```java
class Test
{
    static void test () throws Exception {
        try {
            throw new Exception("demo");
        }
        catch (Exception e) {
            System.out.println("Exception caught");
        }
    }

    public static void main (String args[]) {
        test();
    }
}
```

## User Defined Exceptions
* You don't have to implement anything inside it, no methods are required.
* Override the toString() function, to display customized message.

```java
class ExceptionWithCode extends Exception
{
    private int errorCode;

    ExceptionWithCode (int c) {
        errorCode = c;
    }

    public String toString () {
        return "Error occured. Code: " + this.errorCode;
    }
}
```

# Multithreading

## Main Thread
* Main thread can be obtained with **Thread.currentThread()**

```java
class Example
{
    public static void main(String[] args)
    {
        Thread t = Thread.currentThread();
        t.setName("Example Main");
        System.out.println("Name of thread is " + t);
    }
}
```

## Lifecycle

![thread-life-cycle](./thread-life-cycle.jpg)

* **New**: A thread begins its life cycle in the new state. It remains in this state until the start() method is called on it.
* **Runnable**: After invocation of start() method on new thread, the thread becomes runnable.
* **Running**: A thread is in running state if the thread scheduler has selected it.
* **Waiting**: A thread is in waiting state if it waits for another thread to perform a task. In this stage the thread is still alive.
* **Terminated**: A thread enter the terminated state when it complete its task.


## Priority
* It is used to help the operating system to determine which threads are scheduled for execution.
* Thread priority ranges between 1 to 10.


## Thread Class

### Construct
* You can create new thread:
  * extending **Thread class** or 
  * implementing **Runnable interface**

```java
// constructors
Thread ()
Thread (String str)
Thread (Runnable r)
Thread (Runnable r, String str)
```

### Sleep
* While using **sleep()** method, always handle the exception it throws.

```java
static void sleep(long milliseconds) throws InterruptedException
```

### Join
* Using **join()** method, we tell our thread to wait until the specified thread completes its execution.

```java
final void join() throws InterruptedException;

final void join(long milliseconds) throws InterruptedException;
```


## Synchronization
* It is like Mutex in C++.
* When more than one thread try to access a shared resource, we need to ensure that resource will be used by only one thread at a time.

### Synchronized Method
```java
public synchronized void writeToResource (String msg);
```

### Synchronized Block
* Way more efficent compared to synchronized method.
```java
public void writeToResource (String msg) {
    System.out.println("Writing is started");
    String resourcePath = "./example.txt";
    // ... do something else

    synchronized (resourceObj) {
        // ... do something critical with resourceObj
    }
}
```

## Thread Communication

### Object.wait()
* The wait() method causes the current thread to wait indefinitely until another thread either invokes **notify()** for this object or **notifyAll()**.
* Can be called only inside:
  * synchronized blocks
  * synchronized methods
* The current thread must own this object's monitor. (synchronized block's scope)
* When wait is called, the thread **releases ownership** of this monitor and waits until another thread notifies threads waiting on this object's monitor.
* After notify() or notifyAll() called the thread can **re-obtain ownership** of the monitor and resumes execution.

```java
final void wait() throws InterruptedException;

final void wait(long timeout) throws InterruptedException;

final void wait(long timeout, int nanos) throws InterruptedException;
```

### Object.notify()
* Wakes up a single thread that is waiting on this object's monitor. 
* If any threads are waiting on this object, one of them is chosen to be awakened. 
* The choice is arbitrary and depends to the implementation.
* The awakened thread will not be able to proceed until the current thread relinquishes the lock on this object. 


### Object.notifyAll()
* Wakes up all threads that are waiting on this object's monitor.



## Example
* If you try to start same thread more than once, **IllegalThreadStateException** will be thrown.

```java
class Job1 implements Runnable {
    private int counter = 0;
    
    public void run() {
        while (this.counter++ < 5) {
            System.out.println("Job1.counter is " + this.counter);
            try { Thread.sleep(1000); }
            catch (InterruptedException e) {}
        }
    }
}


class Job2 extends Thread {
    private int counter = 0;
    
    public void run() {
        while (this.counter++ < 1000) {
            System.out.println("Job2.counter is " + this.counter);
            try { Thread.sleep(1000); }
            catch (InterruptedException e) {}
        }
    }
}

class Example
{
    public static void main( String args[] )
    {
        Thread t1 = new Thread(new Job1());
        Job2 t2 = new Job2();
        t1.start();

        try {
  			t1.join();	//Waiting for t1 to finish
		}
        catch (InterruptedException ie) {}

        // then start t2
        t2.start();
    }
}
```


# IO Operations


# Collections

![collection-heirarchy.jpg](./collection-heirarchy.jpg)

* List
  * Elements can be duplicated.
  * Random access and insertions based on a position.
* Set
  * Elements are unique.
* Queue
  * It is a FIFO. (first in first out)
* Map
  * Stores data in key and value association.
  * Both key and values are objects. 
  * Key must be unique.
* Vector
  * synchronized
  * contains many legacy methods that ArrayList does not have
  * Iterable with for-each loop
* Stack
  * Extends vector class.
  * LIFO (Last in first out)

## Accessing Elements
* You can access all elements of all type of collection by these methods.

### Iterator
* Iterate over one direction (next)
* Usage:
  * Call collection's **iterator()** method.
  * Set up a loop that checks the **hasNext()** method
  * Within the loop, obtain each element by calling **next()** method.
  * If needed you can call to **remove()** method to remove current element from collection.

```java

class Test_Iterator
{
    public static void main(String[] args)
    {
        ArrayList<String> ar = new ArrayList<String>();
        ar.add("ab");
        ar.add("bc");
        ar.add("cd");
        ar.add("de");
        Iterator it = ar.iterator();

        while (it.hasNext()) System.out.print(it.next() + " ");
    }
}
```
  

### ListIterator
* ListIterator Interface is used to traverse a list in both **forward** and **backward** direction. 
* It is available to only those collections that implements the **List** Interface.

```java
class Test_Iterator
{
    public static void main(String[] args)
    {
        ArrayList<String> ar = new ArrayList<String>();
        ar.add("ab");
        ar.add("bc");
        ar.add("cd");
        ar.add("de");
        ListIterator litr = ar.listIterator();
        while(litr.hasNext()) System.out.print(litr.next() + " ");
        while(litr.hasPrevious()) System.out.print(litr.previous() + " ");
    }
}
```

### for-each Loop 
* This can only be used if we don't want to **modify** the contents of a collection and we don't want any **reverse access**.

```java
class ForEachDemo
{
    public static void main(String[] args)
    {
        LinkedList<String> ls = new LinkedList<String>();
        ls.add("a");
        ls.add("b");
        ls.add("c");
        ls.add("d");

        for(String str : ls) System.out.print(str + " ");
    }
}
```

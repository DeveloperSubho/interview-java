## Singleton Pattern

In Java, Singleton is a design pattern that ensures that a class can only have one object.

To create a singleton class, a class must implement the following properties:

- Create a private constructor of the class to restrict object creation outside of the class.
- Create a private attribute of the class type that refers to the single object.
- Create a public static method that allows us to create and access the object we created. Inside the method, we will create a condition that restricts us from creating more than one object.

Example: Java Singleton Class Syntax

```
class SingletonExample {

// private field that refers to the object
private static SingletonExample singleObject;

private SingletonExample() {
// constructor of the SingletonExample class
}

public static SingletonExample getInstance() {
// write code that allows us to create only one object
// access the object as per our need
}
}
```

In the above example,

- private static SingletonExample singleObject - a reference to the object of the class.
- private SingletonExample() - a private constructor that restricts creating objects outside of the class.
- public static SingletonExample getInstance() - this method returns the reference to the only object of the class. Since the method static, it can be accessed using the class name.

### Use of Singleton in Java

Singletons can be used while working with databases. They can be used to create a connection pool to access the database while reusing the same connection for all the clients. For example,

class Database {
private static Database dbObject;

private Database() {  
 }

public static Database getInstance() {

      // create object if it's not already created
      if(dbObject == null) {
         dbObject = new Database();
      }

       // returns the singleton object
       return dbObject;

}

public void getConnection() {
System.out.println("You are now connected to the database.");
}
}

class Main {
public static void main(String[] args) {
Database db1;

      // refers to the only object of Database
      db1= Database.getInstance();

      db1.getConnection();

}
}

In our above example,

- We have created a singleton class Database.
- The dbObject is a class type field. This will refer to the object of the class Database.
- The private constructor Database() prevents object creation outside of the class.
- The static class type method getInstance() returns the instance of the class to the outside world.
- In the Main class, we have class type variable db1. We are calling getInstance() using db1 to get the only object of the Database.
- The method getConnection() can only be accessed using the object of the Database.
- Since the Database can have only one object, all the clients can access the database through a single connection.

Advantage of Singleton design pattern
Saves memory because object is not created at each request. Only single instance is reused again and again.
Usage of Singleton design pattern
Singleton pattern is mostly used in multi-threaded and database applications. It is used in logging, caching, thread pools, configuration settings etc.

1. Eager initialization: This is the simplest method of creating a singleton class. In this, object of class is created when it is loaded to the memory by JVM. It is done by assigning the reference of an instance directly.
   It can be used when program will always use instance of this class, or the cost of creating the instance is not too large in terms of resources and time.

// Java code to create singleton class by
// Eager Initialization
public class GFG
{
// public instance initialized when loading the class
private static final GFG instance = new GFG();

private GFG()
{
// private constructor
}
public static GFG getInstance(){
return instance;
}
}

Pros:
Very simple to implement.
May lead to resource wastage. Because instance of class is created always, whether it is required or not.
CPU time is also wasted in creation of instance if it is not required.
Exception handling is not possible.

Using static block: This is also a sub part of Eager initialization. The only difference is object is created in a static block so that we can have access on its creation, like exception handling. In this way also, object is created at the time of class loading.
It can be used when there is a chance of exceptions in creating object with eager initialization.

// Java code to create singleton class
// Using Static block
public class GFG
{
// public instance
public static GFG instance;

private GFG()
{
// private constructor
}
static
{
// static block to initialize instance
instance = new GFG();
}
}
Pros:
Very simple to implement.
No need to implement getInstance() method. Instance can be accessed directly.
Exceptions can be handled in static block.
May lead to resource wastage. Because instance of class is created always, whether it is required or not.
CPU time is also wasted in creation of instance if it is not required.
Lazy initialization: In this method, object is created only if it is needed. This may prevent resource wastage. An implementation of getInstance() method is required which return the instance. There is a null check that if object is not created then create, otherwise return previously created. To make sure that class cannot be instantiated in any other way, constructor is made final. As object is created with in a method, it ensures that object will not be created until and unless it is required. Instance is kept private so that no one can access it directly.
It can be used in a single threaded environment because multiple threads can break singleton property as they can access get instance method simultaneously and create multiple objects.

//Java Code to create singleton class
// With Lazy initialization
public class GFG
{
// private instance, so that it can be
// accessed by only by getInstance() method
private static GFG instance;

private GFG()
{
// private constructor
}

//method to return instance of class
public static GFG getInstance()
{
if (instance == null)
{
// if instance is null, initialize
instance = new GFG();
}
return instance;
}
}
Pros:
Object is created only if it is needed. It may overcome wastage of resource and CPU time.
Exception handling is also possible in method.
Every time a condition of null has to be checked.
instance can’t be accessed directly.
In multithreaded environment, it may break singleton property.
Thread Safe Singleton: A thread safe singleton is created so that singleton property is maintained even in multithreaded environment. To make a singleton class thread safe, getInstance() method is made synchronized so that multiple threads can’t access it simultaneously.

// Java program to create Thread Safe
// Singleton class
public class GFG
{
// private instance, so that it can be
// accessed by only by getInstance() method
private static GFG instance;

private GFG()
{
// private constructor
}

//synchronized method to control simultaneous access
synchronized public static GFG getInstance()
{
if (instance == null)
{
// if instance is null, initialize
instance = new GFG();
}
return instance;
}
}
Pros:
Lazy initialization is possible.
It is also thread safe.
getInstance() method is synchronized so it causes slow performance as multiple threads can’t access it simultaneously.
Lazy initialization with Double check locking: In this mechanism, we overcome the overhead problem of synchronized code. In this method, getInstance is not synchronized but the block which creates instance is synchronized so that minimum number of threads have to wait and that’s only for first time.

// Java code to explain double check locking
public class GFG
{
// private instance, so that it can be
// accessed by only by getInstance() method
private static GFG instance;

private GFG()
{
// private constructor
}

public static GFG getInstance()
{
if (instance == null)
{
//synchronized block to remove overhead
synchronized (GFG.class)
{
if(instance==null)
{
// if instance is null, initialize
instance = new GFG();
}

      }
    }
    return instance;

}
}
Pros:
Lazy initialization is possible.
It is also thread safe.
Performance overhead gets reduced because of synchronized keyword.
First time, it can affect performance.

_References_
https://www.programiz.com/java-programming/singleton#:~:text=In%20Java%2C%20Singleton%20is%20a,creation%20outside%20of%20the%20class.
https://www.javatpoint.com/singleton-design-pattern-in-java

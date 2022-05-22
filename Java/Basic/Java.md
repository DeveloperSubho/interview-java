**1. What is the difference between an Interface and an Abstract class?**
Java provides and supports the creation both of abstract classes and interfaces. Both implementations share some common characteristics, but they differ in the following features:

All methods in an interface are implicitly abstract. On the other hand, an abstract class may contain both abstract and non-abstract methods.
A class may implement a number of Interfaces, but can extend only one abstract class.
In order for a class to implement an interface, it must implement all its declared methods. However, a class may not implement all declared methods of an abstract class. Though, in this case, the sub-class must also be declared as abstract.
Abstract classes can implement interfaces without even providing the implementation of interface methods.
Variables declared in a Java interface is by default final. An abstract class may contain non-final variables.
Members of a Java interface are public by default. A member of an abstract class can either be private, protected or public.
An interface is absolutely abstract and cannot be instantiated. An abstract class also cannot be instantiated, but can be invoked if it contains a main method.

**2. Interface variables are static and final by default in Java, Why?**
An interface defines a protocol of behaviour and not how it should be implemented. A class that implements an interface adheres to the protocol defined by that interface.

Interface variables are static because java interfaces cannot be instantiated on their own. The value of the variable must be assigned in a static context in which no instance exists.
The final modifier ensures the value assigned to the interface variable is a true constant that cannot be re-assigned. In other words, interfaces can declare only constants, not instance variables.
Template :
interface interfaceName{
    // Any number of final, static variables
    datatype variableName = value;
    // Any number of abstract method declarations
    returntype methodName(list of parameters or no parameters);
}

**3. What differences exist between HashMap and Hashtable?**
There are several differences between HashMap and Hashtable in Java:

Hashtable is synchronized, whereas HashMap is not. This makes HashMap better for non-threaded applications, as unsynchronized Objects typically perform better than synchronized ones.

Hashtable does not allow null keys or values. HashMap allows one null key and any number of null values.

One of HashMap's subclasses is LinkedHashMap, so in the event that you'd want predictable iteration order (which is insertion order by default), you could easily swap out the HashMap for a LinkedHashMap. This wouldn't be as easy if you were using Hashtable.

**4. What is the difference between Exception and Error in java?**
An Error "indicates serious problems that a reasonable application should not try to catch."
An Exception "indicates conditions that a reasonable application might want to catch."

**5. How does Garbage Collection prevent a Java application from going out of memory?**
It doesn’t! Garbage Collection simply cleans up unused memory when an object goes out of scope and is no longer needed. However an application could create a huge number of large objects that causes an OutOfMemoryError.

**6. What is reflection and why is it useful?**
The name reflection is used to describe code which is able to inspect other code in the same system (or itself) and to make modifications at runtime.

For example, say you have an object of an unknown type in Java, and you would like to call a 'doSomething' method on it if one exists. Java's static typing system isn't really designed to support this unless the object conforms to a known interface, but using reflection, your code can look at the object and find out if it has a method called 'doSomething' and then call it if you want to.

**7. How can I synchronize two Java processes?**
It is not possible to do something like you want in Java. Different Java applications will use different JVM's fully separating themselves into different 'blackbox'es. However, you have 2 options:

Use sockets (or channels). Basically one application will open the listening socket and start waiting until it receives some signal. The other application will connect there, and send signals when it had completed something. I'd say this is a preferred way used in 99.9% of applications.
You can call winapi from Java (on windows).

**8. What is difference between fail-fast and fail-safe Iterator?**
The major difference is fail-safe iterator doesn’t throw any Exception, contrary to fail-fast Iterator.This is because they work on a clone of Collection instead of the original collection and that’s why they are called as the fail-safe iterator.

Iterators in java are used to iterate over the Collection objects.Fail-Fast iterators immediately throw ConcurrentModificationException if there is structural modification of the collection. Structural modification means adding, removing or updating any element from collection while a thread is iterating over that collection. Iterator on ArrayList, HashMap classes are some examples of fail-fast Iterator.

Fail-Safe iterators don’t throw any exceptions if a collection is structurally modified while iterating over it. This is because, they operate on the clone of the collection, not on the original collection and that’s why they are called fail-safe iterators. Iterator on CopyOnWriteArrayList, ConcurrentHashMap classes are examples of fail-safe Iterator.
If you remove an element via Iterator remove() method, exception will not be thrown. However, in case of removing via a particular collection remove() method, ConcurrentModificationException will be thrown.


**9. What is the tradeoff between using an unordered array versus an ordered array?**
The major advantage of an ordered array is that the search times have time complexity of O(log n) by using Binary Search, compared to that of an unordered array, which is O (n). The disadvantage of an ordered array is that the insertion operation has a time complexity of O(n), because the elements with higher values must be moved to make room for the new element. Instead, the insertion operation for an unordered array takes constant time of O(1).

**10. What is structure of Java Heap?**
It is a shared runtime data area and stores the actual object in a memory. It is instantiated during the virtual machine startup.
This memory is allocated for all class instances and array. Heap can be of fixed or dynamic size depending upon the system’s configuration.
JVM provides the user control to initialize or vary the size of heap as per the requirement. When a new keyword is used, object is assigned a space in heap, but the reference of the same exists onto the stack.
There exists one and only one heap for a running JVM process.

**11. What is the difference between throw and throws?**
The throw keyword is used to explicitly raise a exception within the program. On the contrary, the throws clause is used to indicate those exceptions that are not handled by a method. Each method must explicitly specify which exceptions does not handle, so the callers of that method can guard against possible exceptions. Finally, multiple exceptions are separated by a comma.

**12. What are the differences between == and equals?**
== never throws NullPointerException, == is subject to type compatibility check at compile time

**13. What is the main difference between StringBuffer and StringBuilder?**
StringBuffer is synchronized, StringBuilder is not. When some thing is synchronized, then multiple threads can access, and modify it with out any problem or side effect. StringBuffer is synchronized, so you can use it with multiple threads with out any problem.

StringBuilder is faster than StringBuffer because it's not synchronized. Using synchronized methods in a single thread is overkill.

**14. Why does Java have transient fields?**
The transient keyword in Java is used to indicate that a field should not be part of the serialization.

**15. What is static initializer?**
The static initializer is a static {} block of code inside java class, and run only one time before the constructor or main method is called. If you had to perform a complicated calculation to determine the value of x — or if its value comes from a database — a static initializer could be very useful.

Consider:
class StaticInit {
    public static int x;
    static {
        x = 32;
    }
    // other class members such as constructors and
    // methods go here...
}

**16. What do the ... dots in the method parameters mean?**
That feature is called varargs, and it's a feature introduced in Java 5. It means that function can receive multiple String arguments:
myMethod("foo", "bar");
myMethod("foo", "bar", "baz");
myMethod(new String[]{"foo", "var", "baz"}); // you can eve

Then, you can use the String var as an array:
public void myMethod(String... strings){
    for(String whatever : strings){
        // do what ever you want
    }

    // the code above is is equivalent to
    for( int i = 0; i < strings.length; i++){
        // classical for. In this case you use strings[i]
    }
}

**17. Why is char[] preferred over String for passwords?**
Strings are immutable. That means once you've created the String, if another process can dump memory, there's no way (aside from reflection) you can get rid of the data before garbage collection kicks in.

With an array, you can explicitly wipe the data after you're done with it. You can overwrite the array with anything you like, and the password won't be present anywhere in the system, even before garbage collection.

**18. When to use LinkedList over ArrayList in Java?**
LinkedList and ArrayList are two different implementations of the List interface. LinkedList implements it with a doubly-linked list.

LinkedList<E> allows for constant-time insertions or removals using iterators, but only sequential access of elements. In other words, you can walk the list forwards or backwards, but finding a position in the list takes time proportional to the size of the list.

ArrayList<E>, on the other hand, allow fast random read access, so you can grab any element in constant time. But adding or removing from anywhere but the end requires shifting all the latter elements over, either to make an opening or fill the gap.

**19. What is Double Brace initialization in Java?**

Double brace initialisation creates an anonymous class derived from the specified class (the outer braces), and provides an initialiser block within that class (the inner braces). e.g.
new ArrayList<Integer>() {{
    add(1);
    add(2);
}};
However, I'm not too fond of that method because what you end up with is a subclass of ArrayList which has an instance initializer, and that class is created just to create one object -- that just seems like a little bit overkill to me.

**20. What exactly is marker interface in Java?**
The marker interface in Java is an interfaces with no field or methods. In other words, it an empty interface in java is called a marker interface. An example of a marker interface is a Serializable, Clonable and Remote interface.

Marker interface is used as a tag to inform a message to the java compiler so that it can add special behaviour to the class implementing it.

**21. Synchronization in Java?**
Synchronization in java is the capability to control the access of multiple threads to any shared resource.

Java Synchronization is better option where we want to allow only one thread to access the shared resource.

**22. Provide some examples when a finally block won't be executed in Java?**
Finally block will always be called unless you call System.exit() (or the thread crashes).

**23. What is an efficient way to implement a singleton pattern in Java?**
A single-element enum type is the best way to implement a singleton

**24. Compare volatile vs static variables in Java**
Declaring a static variable in Java, means that there will be only one copy, no matter how many objects of the class are created. The variable will be accessible even with no Objects created at all. However, threads may have locally cached values of it.
Declaring a variable as volatile (be it static or not) states that the variable will be accessed frequently by multiple threads. In Java, this boils down to instructing threads that they can not cache the variable's value, but will have to write back immediately after mutating so that other threads see the change. (Threads in Java are free to cache variables by default).

**25. How do we synchronize a static method to prevent data corruption in concurrent update?**
**26. How will you introduce multi-tasking in your java application?**
**27. Design a program to search files inside a directory using multi-threading?**
**28. How will you implement a LRU timed cache in Java?**
**29. There is a folder containing multiple SQL files. Each file’s name contain a sequence number which determines the execution order of that particular SQL script file. Design an API that will take such folder and execute the SQL in correct order.**
**30. What is difference between wait() and sleep() method?**
**31. What is a deadlock situation? How will you handle deadlock in development and production environment?**
**32. What is mechanism for inter-thread communication?**
**33. Would adding multi-threading to sorting algorithm improve its performance?**
**34. In what practical scenario’s multi-threading actually improves the performance of Java application?**
**35. Do we need to synchronize getter and setter both to prevent concurrency related issues on a collection?**
**36. Explain the key classes in java.util.concurrent package?**
**37. What is purpose of ConcurrentHashMap?**
**38. What is purpose of a Future? How will you use it?**
**39. How will you create your own custom threadpool in java without executor framework?**
**40. What is a BlockingQueue?**
**41. Complete Solution to Delhi Metro Design using Spring MVC with testcases**
**42. What is the use of Anonymous class?**
**43. Inner class in Java?**
**44. Super and this keyword in Java?**
**45. Java is a call by Value or reference?**


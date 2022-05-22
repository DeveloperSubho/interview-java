Java 8 was released in early 2014. This tutorial list down important Java 8 features with examples such as lambda expressions, Java streams, functional interfaces, default methods and date-time API changes.  

**1. Lambda Expressions**  
Lambda expressions are known to many of us who have worked on other popular programming languages like Scala. In Java programming language, a Lambda expression (or function) is just an anonymous function, i.e., a function with no name and without being bound to an identifier.

Lambda expressions are written precisely where it’s needed, typically as a parameter to some other function.

***1.1. Syntax***  
A few basic syntaxes of lambda expressions are:
```
(parameters) -> expression

(parameters) -> { statements; }

() -> expression
```
A typical lambda expression example will be like this:
```
//This function takes two parameters and return their sum
(x, y) -> x + y
```
Please note that based on the type of x and y, we may use the method in multiple places. Parameters can match to int, or Integer or simply String also. Based on context, it will either add two integers or concatenate two strings.

***1.2. Rules for writing lambda expressions***  
A lambda expression can have zero, one or more parameters.
The type of the parameters can be explicitly declared or it can be inferred from the context.
Multiple parameters are enclosed in mandatory parentheses and separated by commas. Empty parentheses are used to represent an empty set of parameters.
When there is a single parameter, if its type is inferred, it is not mandatory to use parentheses.
The body of the lambda expressions can contain zero, one, or more statements.
If the body of lambda expression has a single statement, curly brackets are not mandatory and the return type of the anonymous function is the same as that of the body expression. When there is more than one statement in the body then these must be enclosed in curly brackets.

**2. Functional Interfaces**
Functional interfaces are also called Single Abstract Method interfaces (SAM Interfaces). As the name suggests, a functional interface permits exactly one abstract method in it.

Java 8 introduces @FunctionalInterface annotation that we can use for giving compile-time errors it a functional interface violates the contracts.

***2.1. Functional Interface Example***    
```
//Optional annotation
@FunctionalInterface
public interface MyFirstFunctionalInterface {
public void firstWork();
}
```
Please note that a functional interface is valid even if the @FunctionalInterface annotation is omitted. It is only for informing the compiler to enforce a single abstract method inside the interface.

Also, since default methods are not abstract , we can add default methods to the functional interface as many as we need.

Another critical point to remember is that if a functional interface overrides one of the public methods of java.lang.Object, that also does not count toward the interface’s abstract method count since any implementation of the interface will have an implementation from java.lang.Object or elsewhere.

For example, given below is a perfectly valid functional interface.
```
@FunctionalInterface
public interface MyFirstFunctionalInterface 
{
   public void firstWork();

   @Override
   public String toString();                //Overridden from Object class

   @Override
   public boolean equals(Object obj);        //Overridden from Object class
}
```
**3. Default Methods**  
Java 8 allows us to add non-abstract methods in the interfaces. These methods must be declared default methods. Default methods were introduced in java 8 to enable the functionality of lambda expression.

Default methods enable us to introduce new functionality to the interfaces of our libraries and ensure binary compatibility with code written for older versions of those interfaces.

Let’s understand with an example:
```
public interface Moveable {
   default void move(){
      System.out.println("I am moving");
   }
}
```
Moveable interface defines a method move() and provided a default implementation as well. If any class implements this interface then it need not to implement its own version of move() method. It can directly call instance.move(). e.g.
```
public class Animal implements Moveable{
   public static void main(String[] args){
      Animal tiger = new Animal();
      tiger.move();
   }
}
```
Output: I am moving
If the class willingly wants to customize the behavior of move() method then it can provide its own custom implementation and override the method.

**4. Java 8 Streams**  
Another significant change introduced Java 8 Streams API, which provides a mechanism for processing a set of data in various ways that can include filtering, transformation, or any other way that may be useful to an application.

Streams API in Java 8 supports a different type of iteration where we define the set of items to be processed, the operation(s) to be performed on each item, and where the output of those operations is to be stored.

***4.1. Stream API Example***    
In this example, items is collection of String values and we want to remove the entries that begin with some prefix text.

List<String> items = ...; //Initialize the list
 
String prefix = "str";
 
List<String> filteredList = items.stream()
          .filter(e -> (!e.startsWith(prefix)))
          .collect(Collectors.toList());
Here items.stream() indicates that we wish to have the data in the items collection process using the Streams API.

**5. Java 8 Date/Time API Changes**  
The new Date and Time APIs/classes (JSR-310), also called ThreeTen, have simply changed the way we handle dates in java applications.

***5.1. Date Classes***  
Date class has even become obsolete. The new classes intended to replace Date class are LocalDate, LocalTime and LocalDateTime.

The LocalDate class represents a date. There is no information of a time or time-zone.
The LocalTime class represents a time. There is no information of a date or time-zone.
The LocalDateTime class represents a date and time. There is no information of a time-zone.
If we want to use the date functionality with timezone information, then Lambda provide us extra three classes similar to above one i.e. OffsetDate, OffsetTime and OffsetDateTime.

Timezone offset can be represented in “+05:30” or “Europe/Paris” formats. This is done via using another class i.e. ZoneId.

LocalDate localDate = LocalDate.now();
LocalTime localTime = LocalTime.of(12, 20);
LocalDateTime localDateTime = LocalDateTime.now(); 

OffsetDateTime offsetDateTime = OffsetDateTime.now();
ZonedDateTime zonedDateTime = ZonedDateTime.now(ZoneId.of("Europe/Paris"));
A good understanding of these classes will help you in date-time related Java 8 interview questions.

***5.2. Timestamp and Duration Classes***  
For representing the specific timestamp at any moment, the class needs to be used is Instant. The Instant class represents an instant in time to an accuracy of nanoseconds.

Operations on an Instant include comparison to another Instant and adding or subtracting a duration.

Instant instant = Instant.now();
Instant instant1 = instant.plus(Duration.ofMillis(5000));
Instant instant2 = instant.minus(Duration.ofMillis(5000));
Instant instant3 = instant.minusSeconds(10);
Duration class is a whole new concept brought first time in java language. It represents the time difference between two timestamps.

Duration duration = Duration.ofMillis(5000);
duration = Duration.ofSeconds(60);
duration = Duration.ofMinutes(10);
Duration deals with a small unit of time such as milliseconds, seconds, minutes, and hours. They are more suitable for interacting with application code.

To interact with humans, we need to get bigger time durations presented with Period class.

Period period = Period.ofDays(6);
period = Period.ofMonths(6);
period = Period.between(LocalDate.now(), LocalDate.now().plusDays(60));
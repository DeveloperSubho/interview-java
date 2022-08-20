**What is factory Design Pattern**
Factory pattern is one of the most used design patterns in Java. This type of design pattern comes under creational pattern as this pattern provides one of the best ways to create an object.

In Factory pattern, we create object without exposing the creation logic to the client and refer to newly created object using a common interface.

This pattern delegates the responsibility of initializing a class from the client to a particular factory class by creating a type of virtual constructor.

To achieve this, we rely on a factory which provides us with the objects, hiding the actual implementation details. The created objects are accessed using a common interface.

The Factory Method Pattern is also known as Virtual Constructor.

**Advantage of Factory Design Pattern**
• Factory Method Pattern allows the sub-classes to choose the type of objects to create.
• It promotes the loose-coupling by eliminating the need to bind application-specific classes into the code. That means the code interacts solely with the resultant interface or abstract class, so that it will work with any classes that implement that interface or that extends that abstract class.

**Usage of Factory Design Pattern**
• When a class doesn't know what sub-classes will be required to create
• When a class wants that its sub-classes specify the objects to be created.
• When the parent classes choose the creation of objects to its sub-classes.

**Concept and Class Diagram**

![\factory_pattern_uml_diagram.jpg](/Screenshots/factory_pattern_uml_diagram.jpg)

**Code Walkthrough**
_Step 1_
Create an interface.

Shape.java

```
public interface Shape {
   void draw();
}
```

_Step 2_
Create concrete classes implementing the same interface.

Rectangle.java

```
public class Rectangle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}
```

Square.java

```
public class Square implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Square::draw() method.");
   }
}
```

Circle.java

```
public class Circle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Circle::draw() method.");
   }
}
```

_Step 3_
Create a Factory to generate object of concrete class based on given information.

ShapeFactory.java

```
public class ShapeFactory {

   //use getShape method to get object of type shape
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();

      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();

      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }

      return null;
   }
}
```

_Step 4_
Use the Factory to get object of concrete class by passing an information such as type.

FactoryPatternDemo.java

```
public class FactoryPatternDemo {

   public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();

      //get an object of Circle and call its draw method.
      Shape shape1 = shapeFactory.getShape("CIRCLE");

      //call draw method of Circle
      shape1.draw();

      //get an object of Rectangle and call its draw method.
      Shape shape2 = shapeFactory.getShape("RECTANGLE");

      //call draw method of Rectangle
      shape2.draw();

      //get an object of Square and call its draw method.
      Shape shape3 = shapeFactory.getShape("SQUARE");

      //call draw method of square
      shape3.draw();
   }
}
```

_Step 5_
Verify the output.

Inside Circle::draw() method.
Inside Rectangle::draw() method.
Inside Square::draw() method.

**Real-time examples**
This design pattern has been widely used in JDK, such as

1. getInstance() method of java.util.Calendar, NumberFormat, and ResourceBundle uses factory method design pattern.
2. All the wrapper classes like Integer, Boolean etc, in Java uses this pattern to evaluate the values using valueOf() method.
3. java.nio.charset.Charset.forName(), java.sql.DriverManager#getConnection(), java.net.URL.openConnection(), java.lang.Class.newInstance(), java.lang.Class.forName() are some of ther example where factory method design pattern has been used.

**When to Use Factory Method Design Pattern**
• When the implementation of an interface or an abstract class is expected to change frequently
• When the current implementation cannot comfortably accommodate new change
• When the initialization process is relatively simple, and the constructor only requires a handful of parameters

_References_
https://www.youtube.com/watch?v=s3Wr5_tsODs
https://www.tutorialspoint.com/design_pattern/factory_pattern.htm
https://www.geeksforgeeks.org/factory-method-design-pattern-in-java/
https://www.baeldung.com/creational-design-patterns

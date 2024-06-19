In Java, the final keyword is used to indicate that a variable, method, or class cannot be modified or extended. Here are some of its characteristics:

1. Final variables: When a variable is declared as final, its value cannot be changed once it has been initialized. This is useful for declaring constants or other values that should not be modified.
2. Final methods: When a method is declared as final, it cannot be overridden by a subclass. This is useful for methods that are part of a class’s public API and should not be modified by subclasses.
3. Final classes: When a class is declared as final, it cannot be extended by a subclass. This is useful for classes that are intended to be used as is and should not be modified or extended.
4. Initialization: Final variables must be initialized either at the time of declaration or in the constructor of the class. This ensures that the value of the variable is set and cannot be changed.
5. Performance: The use of a final can sometimes improve performance, as the compiler can optimize the code more effectively when it knows that a variable or method cannot be changed.
6. Security: The final can help improve security by preventing malicious code from modifying sensitive data or behavior.
   Overall, the final keyword is a useful tool for improving code quality and ensuring that certain aspects of a program cannot be modified or extended. By declaring variables, methods, and classes as final, developers can write more secure, robust, and maintainable code.

Java Final Variable
When a variable is declared with the final keyword, its value can’t be changed, essentially, a constant. This also means that you must initialize a final variable.

If the final variable is a reference, this means that the variable cannot be re-bound to reference another object, but the internal state of the object pointed by that reference variable can be changed i.e. you can add or remove elements from the final array or final collection.

It is good practice to represent final variables in all uppercase, using underscore to separate words.

```
public class ConstantExample {
    public static void main(String[] args) {
        // Define a constant variable PI
        final double PI = 3.14159;

        // Print the value of PI
        System.out.println("Value of PI: " + PI);
    }
}
```

Output:

Value of PI: 3.14159

_Reference:_
https://www.geeksforgeeks.org/final-keyword-in-java/

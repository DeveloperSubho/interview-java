**What Is Functional Programming?**
Basically, functional programming is a style of writing computer programs that treat computations as evaluating mathematical functions.

In mathematics, a function is an expression that relates an input set to an output set.

Importantly, the output of a function depends only on its input. More interestingly, we can compose two or more functions together to get a new function.

**Categorization of Programming Paradigms**
Of course, functional programming is not the only programming style in practice. Broadly speaking, programming styles can be categorized into imperative and declarative programming paradigms.

The imperative approach defines a program as a sequence of statements that change the program's state until it reaches the final state.

Procedural programming is a type of imperative programming where we construct programs using procedures or subroutines. One of the popular programming paradigms known as Object-Oriented Programming (OOP) extends procedural programming concepts.

In contrast, the declarative approach expresses the logic of a computation without describing its control flow in terms of a sequence of statements.

Simply put, the declarative approach's focus is to define what the program has to achieve rather than how it should achieve it. Functional programming is a subset of the declarative programming languages.

**Categorization of Programming Languages**
Now we'll try to understand how programming languages are divided based on their support for functional programming for our purposes.

Pure functional languages, such as Haskell, only allow pure functional programs.

Other languages allow both functional and procedural programs and are considered impure functional languages. Many languages fall into this category, including Scala, Kotlin and Java.

It's important to understand that most of the popular programming languages today are general-purpose languages, so they tend to support multiple programming paradigms.

**Fundamental Principles and Concepts**
This section will cover some of the basic principles of functional programming and how to adopt them in Java.

Please note that many features we'll be using haven't always been part of Java, and it's advisable to be on Java 8 or later to exercise functional programming effectively.

**_First-Class and Higher-Order Functions_**
A programming language is said to have first-class functions if it treats functions as first-class citizens.

This means that functions are allowed to support all operations typically available to other entities. These include assigning functions to variables, passing them as arguments to other functions and returning them as values from other functions.

This property makes it possible to define higher-order functions in functional programming. Higher-order functions are capable of receiving functions as arguments and returning a function as a result. This further enables several techniques in functional programming such as function composition and currying.

Traditionally, it was only possible to pass functions in Java using constructs such as functional interfaces or anonymous inner classes. Functional interfaces have exactly one abstract method and are also known as Single Abstract Method (SAM) interfaces.

Let's say we have to provide a custom comparator to Collections.sort method:

```
    Collections.sort(numbers, new Comparator<Integer>() {
        @Override
        public int compare(Integer n1, Integer n2) {
            return n1.compareTo(n2);
        }
    });
```

As we can see, this is a tedious and verbose technique — certainly not something that encourages developers to adopt functional programming.

Fortunately, Java 8 brought many new features to ease the process, such as lambda expressions, method references and predefined functional interfaces.

Let's see how a lambda expression can help us with the same task:

```
    Collections.sort(numbers, (n1, n2) -> n1.compareTo(n2));
```

This is definitely more concise and understandable.

However, please note that while this may give us the impression of using functions as first-class citizens in Java, that's not the case.

Behind the syntactic sugar of lambda expressions, Java still wraps these into functional interfaces. So, Java treats a lambda expression as an Object, which is the true first-class citizen in Java.

**Pure Functions**
The definition of pure function emphasizes that a pure function should return a value based only on the arguments and should have no side effects.

This can sound quite contrary to all the best practices in Java.

As an object-oriented language, Java recommends encapsulation as a core programming practice. It encourages hiding an object's internal state and exposing only necessary methods to access and modify it. So, these methods aren't strictly pure functions.

Of course, encapsulation and other object-oriented principles are only recommendations and not binding in Java.

In fact, developers have recently started to realize the value of defining immutable states and methods without side effects.

Let's say we want to find the sum of all the numbers we've just sorted:

```
Integer sum(List<Integer> numbers) {
    return numbers.stream().collect(Collectors.summingInt(Integer::intValue));
}
```

This method depends only on the arguments it receives, so it's deterministic. Moreover, it doesn't produce any side effects.

Side effects can be anything apart from the intended behavior of the method. For instance, side effects can be as simple as updating a local or global state or saving to a database before returning a value. (Purists also treat logging as a side effect.)

So, let's look at how we deal with legitimate side effects. For instance, we may need to save the result in a database for genuine reasons. There are techniques in functional programming to handle side effects while retaining pure functions.

We'll discuss some of them in later sections.

**Immutability**
Immutability is one of the core principles of functional programming, and it refers to the property that an entity can't be modified after being instantiated.

In a functional programming language, this is supported by design at the language level. But in Java we have to make our own decision to create immutable data structures.

Please note that Java itself provides several built-in immutable types, for instance, String. This is primarily for security reasons because we heavily use String in class loading and as keys in hash-based data structures. There are also several other built-in immutable types such as primitive wrappers and math types.

But what about the data structures we create in Java? Of course, they are not immutable by default, and we have to make a few changes to achieve immutability.

The use of the final keyword is one of them, but it doesn't stop there:

```
public class ImmutableData {
    private final String someData;
    private final AnotherImmutableData anotherImmutableData;
    public ImmutableData(final String someData, final AnotherImmutableData anotherImmutableData) {
        this.someData = someData;
        this.anotherImmutableData = anotherImmutableData;
    }
    public String getSomeData() {
        return someData;
    }
    public AnotherImmutableData getAnotherImmutableData() {
        return anotherImmutableData;
    }
}

public class AnotherImmutableData {
    private final Integer someOtherData;
    public AnotherImmutableData(final Integer someData) {
        this.someOtherData = someData;
    }
    public Integer getSomeOtherData() {
        return someOtherData;
    }
}
```

Note that we have to diligently observe a few rules:

All fields of an immutable data structure must be immutable.
This must apply to all the nested types and collections (including what they contain) as well.
There should be one or more constructors for initialization as needed.
There should only be accessor methods, possibly with no side effects.
It's not easy to get it completely right every time, especially when the data structures start to get complex.

However, several external libraries can make working with immutable data in Java easier. For instance, Immutables and Project Lombok provide ready-to-use frameworks for defining immutable data structures in Java.

**Referential Transparency**
Referential transparency is perhaps one of the more difficult principles of functional programming to understand, but the concept is pretty simple.

We call an expression referentially transparent if replacing it with its corresponding value has no impact on the program's behavior.

This enables some powerful techniques in functional programming such as higher-order functions and lazy evaluation.

To understand this better, let's take an example:

```
public class SimpleData {
    private Logger logger = Logger.getGlobal();
    private String data;
    public String getData() {
        logger.log(Level.INFO, "Get data called for SimpleData");
        return data;
    }
    public SimpleData setData(String data) {
        logger.log(Level.INFO, "Set data called for SimpleData");
        this.data = data;
        return this;
    }
}
```

This is a typical POJO class in Java, but we're interested in finding if this provides referential transparency.

Let's observe the following statements:

```
String data = new SimpleData().setData("Baeldung").getData();
logger.log(Level.INFO, new SimpleData().setData("Baeldung").getData());
logger.log(Level.INFO, data);
logger.log(Level.INFO, "Baeldung");
```

The three calls to logger are semantically equivalent but not referentially transparent.

The first call is not referentially transparent since it produces a side effect. If we replace this call with its value as in the third call, we'll miss the logs.

The second call is also not referentially transparent since SimpleData is mutable. A call to data.setData anywhere in the program would make it difficult for it to be replaced with its value.

So, for referential transparency, we need our functions to be pure and immutable. These are the two preconditions we discussed earlier.

As an interesting outcome of referential transparency, we produce context-free code. In other words, we can run them in any order and context, which leads to different optimization possibilities.

**Functional Programming Techniques**
The functional programming principles that we discussed earlier enable us to use several techniques to benefit from functional programming.
In this section, we'll cover some of these popular techniques and understand how we can implement them in Java.

**_Function Composition_**
Function composition refers to composing complex functions by combining simpler functions.

This is primarily achieved in Java using functional interfaces, which are target types for lambda expressions and method references.

Typically, any interface with a single abstract method can serve as a functional interface. So, we can define a functional interface quite easily.

However, Java 8 provides us many functional interfaces by default for different use cases under the package java.util.function.

Many of these functional interfaces provide support for function composition in terms of default and static methods. Let's pick the Function interface to understand this better.

Function is a simple and generic functional interface that accepts one argument and produces a result.

It also provides two default methods, compose and andThen, which will help us in function composition:

```
Function<Double, Double> log = (value) -> Math.log(value);
Function<Double, Double> sqrt = (value) -> Math.sqrt(value);
Function<Double, Double> logThenSqrt = sqrt.compose(log);
logger.log(Level.INFO, String.valueOf(logThenSqrt.apply(3.14)));
// Output: 1.06
Function<Double, Double> sqrtThenLog = sqrt.andThen(log);
logger.log(Level.INFO, String.valueOf(sqrtThenLog.apply(3.14)));
// Output: 0.57
```

Both these methods allow us to compose multiple functions into a single function but offer different semantics. While compose applies the function passed in the argument first and then the function on which it's invoked, andThen does the same in reverse.

Several other functional interfaces have interesting methods to use in function composition, such as the default methods and, or and negate in the Predicate interface. While these functional interfaces accept a single argument, there are two-arity specializations, such as BiFunction and BiPredicate.

**_Monads_**
Many of the functional programming concepts derive from Category Theory, which is a general theory of functions in mathematics. It presents several concepts of categories such as functors and natural transformations.

For us, it's only important to know that this is the basis of using monads in functional programming.

Formally, a monad is an abstraction that allows structuring programs generically. So, a monad allows us to wrap a value, apply a set of transformations, and get the value back with all transformations applied.

Of course, there are three laws that any monad needs to follow — left identity, right identity and associativity — but we won't get into the details here.

In Java, there are a few monads that we use quite often, such as Optional and Stream:

```
Optional.of(2).flatMap(f -> Optional.of(3).flatMap(s -> Optional.of(f + s)))
```

Why do we call Optional a monad?
Here Optional allows us to wrap a value using the method of and apply a series of transformations. We're applying the transformation of adding another wrapped value using the method flatMap.

We could show that Optional follows the three laws of monads. However, an Optional does break the monad laws under some circumstances. But it should be good enough for us for most practical situations.

If we understand monads' basics, we'll soon realize that there are many other examples in Java, such as Stream and CompletableFuture. They help us achieve different objectives, but they all have a standard composition in which context manipulation or transformation is handled.

Of course, we can define our own monad types in Java to achieve different objectives such as log monad, report monad or audit monad. For example, the monad is one of the functional programming techniques to handle side effects in functional programming.

**_Currying_**
Currying is a mathematical technique of converting a function that takes multiple arguments into a sequence of functions that take a single argument.

In functional programming, it gives us a powerful composition technique where we don't need to call a function with all its arguments.

Moreover, a curried function does not realize its effect until it receives all the arguments.

In pure functional programming languages such as Haskell, currying is well supported. In fact, all functions are curried by default.

However, in Java it's not that straightforward:

Function<Double, Function<Double, Double>> weight = mass -> gravity -> mass \* gravity;

Function<Double, Double> weightOnEarth = weight.apply(9.81);
logger.log(Level.INFO, "My weight on Earth: " + weightOnEarth.apply(60.0));

Function<Double, Double> weightOnMars = weight.apply(3.75);
logger.log(Level.INFO, "My weight on Mars: " + weightOnMars.apply(60.0));
Copy
Here we've defined a function to calculate our weight on a planet. While our mass remains the same, gravity varies by the planet we're on.

We can partially apply the function by passing just the gravity to define a function for a specific planet. Moreover, we can pass this partially applied function around as an argument or return value for arbitrary composition.

Currying depends on the language to provide two fundamental features: lambda expressions and closures. Lambda expressions are anonymous functions that help us to treat code as data. We've seen earlier how to implement them using functional interfaces.

A lambda expression may close upon its lexical scope, which we define as its closure.

Let's see an example:

```
private static Function<Double, Double> weightOnEarth() {
    final double gravity = 9.81;
    return mass -> mass * gravity;
}
```

Please note how the lambda expression, which we return in the method above, depends on the enclosing variable, which we call closure. Unlike other functional programming languages, Java has a limitation that the enclosing scope has to be final or effectively final.

As an interesting outcome, currying also allows us to create a functional interface in Java of arbitrary arity.

Recursion
Recursion is another powerful technique in functional programming that allows us to break down a problem into smaller pieces. The main benefit of recursion is that it helps us eliminate the side effects, which is typical of any imperative style looping.

Let's see how we calculate the factorial of a number using recursion:

Integer factorial(Integer number) {
return (number == 1) ? 1 : number \* factorial(number - 1);
}
Copy
Here we call the same function recursively until we reach the base case and then start to calculate our result.

Notice that we're making the recursive call before calculating the result at each step or in words at the head of the calculation. So, this style of recursion is also known as head recursion.

A drawback of this type of recursion is that every step has to hold the state of all previous steps until we reach the base case. This is not really a problem for small numbers, but holding the state for large numbers can be inefficient.

A solution is a slightly different implementation of the recursion known as tail recursion. Here we ensure that the recursive call is the last call a function makes.

Let's see how we can rewrite the above function to use tail recursion:

```
Integer factorial(Integer number, Integer result) {
    return (number == 1) ? result : factorial(number - 1, result * number);
}
```

Notice the use of an accumulator in the function, eliminating the need to hold the state at every step of recursion. The real benefit of this style is to leverage compiler optimizations where the compiler can decide to let go of the current function's stack frame, a technique known as tail-call elimination.

While many languages such as Scala support tail-call elimination, Java still does not have support for this. This is part of the backlog for Java and will perhaps come in some shape as part of larger changes proposed under Project Loom.

**Why Functional Programming Matters**
By now, we might wonder why we even want to make this much effort. For someone coming from a Java background, the shift that functional programming demands is not trivial. So, there should be some really promising advantages for adopting functional programming in Java.

The biggest advantage of adopting functional programming in any language, including Java, is pure functions and immutable states. If we think back, most of the programming challenges are rooted in the side effects and mutable state one way or the other. Simply getting rid of them makes our program easier to read, reason about, test and maintain.

Declarative programming leads to very concise and readable programs. As a subset of declarative programming, functional programming offers several constructs such as higher-order functions, function composition and function chaining. Think of the benefits that Stream API has brought into Java 8 for handling data manipulations.

But don't get tempted to switch over unless completely ready. Please note that functional programming is not a simple design pattern that we can immediately use and benefit from.

Functional programming is more of a change in how we reason about problems and their solutions and how to structure the algorithm.

So, before we start using functional programming, we must train ourselves to think about our programs in terms of functions.

**Is Java a Suitable Fit?**
It's hard to deny functional programming benefits, but is Java a suitable choice for it?

Historically, Java evolved as a general-purpose programming language more suitable for object-oriented programming. Even thinking about using functional programming before Java 8 was tedious! But things have definitely changed after Java 8.

The fact that there are no true function types in Java goes against functional programming's basic principles. The functional interfaces disguised as lambda expressions largely make up for it, at least syntactically.

Then it doesn't help that types in Java are inherently mutable and we have to write so much boilerplate to create immutable types.

We expect other things from a functional programming language that are missing or difficult in Java. For instance, the default evaluation strategy for arguments in Java is eager. But lazy evaluation is a more efficient and recommended way in functional programming.

We can still achieve lazy evaluation in Java using operator short-circuiting and functional interfaces, but it's more involved.

The list is certainly not complete and can include generics support with type-erasure, missing support for tail-call optimization and other things. However, we get a broad idea.

Java is definitely not suitable for starting a program from scratch in functional programming.

But what if we already have an existing program written in Java, probably in object-oriented programming? Nothing stops us from getting some of the benefits of functional programming, especially with Java 8.

This is where most of the benefits of functional programming lie for a Java developer. A combination of object-oriented programming with the benefits of functional programming can go a long way.

_References:_
https://www.baeldung.com/java-functional-programming
https://www.baeldung.com/cs/functional-programming

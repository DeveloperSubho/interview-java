**1. What Is Reactive Programming?**
Reactive programming is about non-blocking, event-driven applications that scale with a small number of threads, with back pressure being a key ingredient that aims to ensure producers don't overwhelm consumers.

These are the primary benefits of reactive programming:

Increased utilization of computing resources on multicore and multi-CPU hardware
Increased performance by reducing serialization
Reactive programming is generally event-driven, in contrast to reactive systems, which are message-driven. So, using reactive programming does not mean we're building a reactive system, which is an architectural style.

However, reactive programming may be used as a means to implement reactive systems if we follow the Reactive Manifesto, which is quite vital to understand.

Based on this, reactive systems have four important characteristics:

Responsive – The system should respond in a timely manner.
Resilient – In case the system faces any failure, it should stay responsive.
Elastic – Reactive systems can react to changes and stay responsive under varying workload.
Message-driven – Reactive systems need to establish a boundary between components by relying on asynchronous message passing.

**2. What Is Spring WebFlux?**
Spring WebFlux is Spring's reactive-stack web framework, and it's an alternative to Spring MVC.

In order to achieve this reactive model and be highly scalable, the entire stack is non-blocking. Check out our tutorial on Spring 5 WebFlux for additional details.

**3. What Are the Mono and Flux Types?**
The WebFlux framework in Spring Framework 5 uses Reactor as its async foundation.

This project provides two core types: Mono to represent a single async value and Flux to represent a stream of async values. They both also implement the Publisher interface defined in the Reactive Streams specification.

Mono implements Publisher and returns 0 or 1 elements:

```
public abstract class Mono<T> implements Publisher<T> {...}
```

And Flux implements Publisher and returns N elements:

```
public abstract class Flux<T> implements Publisher<T> {...}
```

By definition, the two types represent streams, and so they're both lazy. This means nothing is executed until we consume the stream using the subscribe() method. Both types are also immutable, so calling any method will return a new instance of Flux or Mono.

**4. What Is the Use of WebClient and WebTestClient?**
WebClient is a component in the new Web Reactive framework that can act as a reactive client for performing non-blocking HTTP requests. Since it's reactive client, it can handle reactive streams with back pressure, and it can take full advantage of Java 8 lambdas. It can also handle both sync and async scenarios.

On the other hand, the WebTestClient is a similar class that we can use in tests. Basically, it's a thin shell around the WebClient. It can connect to any server over an HTTP connection. It can also bind directly to WebFlux applications using mock request and response objects, without the need for an HTTP server.

**5. What Are the Disadvantages of Using Reactive Streams?**
There are some major disadvantages to using reactive streams:

Troubleshooting a Reactive application is a bit difficult, so be sure to check out our tutorial on debugging reactive streams for some handy debugging tips.
There is limited support for reactive data stores since traditional relational data stores have yet to embrace the reactive paradigm.
There's an extra learning curve when implementing.
Q33. Is Spring 5 Compatible With Older Versions of Java?
In order to take advantage of Java 8 features, the Spring codebase has been revamped. This means older versions of Java cannot be used. So, the framework requires a minimum of Java 8.

**6. How Does Spring 5 Integrate With JDK 9 Modularity?**
In Spring 5, everything has been modularized. This way, we won't be forced to import jars that may not have the functionalities we're looking for.

Please have a look at our guide to Java 9 modularity for an in-depth understanding of how this technology works.

Let's see an example to understand the new module functionality in Java 9 and how to organize a Spring 5 project based on this concept.

We'll first create a new class that contains a single method to return a String “HelloWorld”. We'll place this within a new Java project — HelloWorldModule:

```
package com.hello;
public class HelloWorld {
    public String sayHello(){
        return "HelloWorld";
    }
}
```

Then we create a new module:

```
module com.hello {
    export com.hello;
}
```

Now let's create a new Java Project, HelloWorldClient, to consume the above module by defining a module:

```
module com.hello.client {
    requires com.hello;
}
```

The above module will be available for testing now:

```
public class HelloWorldClient {
public static void main(String[] args){
        HelloWorld helloWorld = new HelloWorld();
        log.info(helloWorld.sayHello());
    }
}
```

**7. Can We Use Both Web MVC and WebFlux in the Same Application?**
As of now, Spring Boot will only allow either Spring MVC or Spring WebFlux, as Spring Boot tries to auto-configure the context depending on the dependencies that exist in its classpath.

Also, Spring MVC cannot run on Netty. Moreover, MVC is a blocking paradigm and WebFlux is a non-blocking style. So, we shouldn't be mixing both together because they serve different purposes.

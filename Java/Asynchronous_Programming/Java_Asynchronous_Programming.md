**Asynchronous Programming in Java**

**_Overview_**
With the growing demand for writing non-blocking code, we need ways to execute the code asynchronously.

**_Asynchronous Programming in Java_**
**_Thread_**
We can create a new thread to perform any operation asynchronously. With the release of lambda expressions in Java 8, it's cleaner and more readable.

Let's create a new thread that computes and prints the factorial of a number:

```
int number = 20;
Thread newThread = new Thread(() -> {
    System.out.println("Factorial of " + number + " is: " + factorial(number));
});
newThread.start();
```

**_FutureTask_**
Since Java 5, the Future interface provides a way to perform asynchronous operations using the FutureTask.
We can use the submit method of the ExecutorService to perform the task asynchronously and return the instance of the FutureTask.

So let's find the factorial of a number:

```
ExecutorService threadpool = Executors.newCachedThreadPool();
Future<Long> futureTask = threadpool.submit(() -> factorial(number));

while (!futureTask.isDone()) {
    System.out.println("FutureTask is not finished yet...");
}
long result = futureTask.get();

threadpool.shutdown();
```

Here we've used the isDone method provided by the Future interface to check if the task is completed. Once finished, we can retrieve the result using the get\*\* method.

**_CompletableFuture_**
Java 8 introduced CompletableFuture with a combination of a Future and CompletionStage. It provides various methods like supplyAsync, runAsync, and thenApplyAsync for asynchronous programming.

Now let's use the CompletableFuture in place of the FutureTask to find the factorial of a number:

```
CompletableFuture<Long> completableFuture = CompletableFuture.supplyAsync(() -> factorial(number));
while (!completableFuture.isDone()) {
    System.out.println("CompletableFuture is not finished yet...");
}
long result = completableFuture.get();
```

We don't need to use the ExecutorService explicitly. The CompletableFuture internally uses ForkJoinPool to handle the task asynchronously. Thus, it makes our code a lot cleaner.

Guava
Guava provides the ListenableFuture class to perform asynchronous operations.

First, we'll add the latest guava Maven dependency:

```
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>31.0.1-jre</version>
</dependency>
```

Then let's find the factorial of a number using the ListenableFuture:

```
ExecutorService threadpool = Executors.newCachedThreadPool();
ListeningExecutorService service = MoreExecutors.listeningDecorator(threadpool);
ListenableFuture<Long> guavaFuture = (ListenableFuture<Long>) service.submit(()-> factorial(number));
long result = guavaFuture.get();
```

Here the MoreExecutors class provides the instance of the ListeningExecutorService class. Then the ListeningExecutorService.submit method performs the task asynchronously and returns the instance of the ListenableFuture.

Guava also has a Futures class that provides methods like submitAsync, scheduleAsync, and transformAsync to chain the ListenableFutures, similar to the CompletableFuture.

For instance, let's see how to use Futures.submitAsync in place of the ListeningExecutorService.submit method:

```
ListeningExecutorService service = MoreExecutors.listeningDecorator(threadpool);
AsyncCallable<Long> asyncCallable = Callables.asAsyncCallable(new Callable<Long>() {
    public Long call() {
        return factorial(number);
    }
}, service);
ListenableFuture<Long> guavaFuture = Futures.submitAsync(asyncCallable, service);
```

Here the submitAsync method requires an argument of AsyncCallable, which is created using the Callables class.

Additionally, the Futures class provides the addCallback method to register the success and failure callbacks:

```
Futures.addCallback(
  factorialFuture,
  new FutureCallback<Long>() {
      public void onSuccess(Long factorial) {
          System.out.println(factorial);
      }
      public void onFailure(Throwable thrown) {
          thrown.getCause();
      }
  },
  service);
```

**_EA Async_**
Electronic Arts brought the async-await feature from .NET to the Java ecosystem through the ea-async library.

This library allows writing asynchronous (non-blocking) code sequentially. Therefore, it makes asynchronous programming easier and scales naturally.

First, we'll add the latest ea-async Maven dependency to the pom.xml:

```
<dependency>
    <groupId>com.ea.async</groupId>
    <artifactId>ea-async</artifactId>
    <version>1.2.3</version>
</dependency>
```

Then we'll transform the previously discussed CompletableFuture code by using the await method provided by EA's Async class:

```
static {
    Async.init();
}

public long factorialUsingEAAsync(int number) {
    CompletableFuture<Long> completableFuture = CompletableFuture.supplyAsync(() -> factorial(number));
    long result = Async.await(completableFuture);
}
```

Here we make a call to the Async.init method in the static block to initialize the Async runtime instrumentation.

Async instrumentation transforms the code at runtime, and rewrites the call to the await method to behave similarly to using the chain of CompletableFuture.

Therefore, the call to the await method is similar to calling Future.join.

We can use the – javaagent JVM parameter for compile-time instrumentation. This is an alternative to the Async.init method:

java -javaagent:ea-async-1.2.3.jar -cp <claspath> <MainClass>
Copy
Now let's look at another example of writing asynchronous code sequentially.

First, we'll perform a few chain operations asynchronously using the composition methods like thenComposeAsync and thenAcceptAsync of the CompletableFuture class:

```
CompletableFuture<Void> completableFuture = hello()
  .thenComposeAsync(hello -> mergeWorld(hello))
  .thenAcceptAsync(helloWorld -> print(helloWorld))
  .exceptionally(throwable -> {
      System.out.println(throwable.getCause());
      return null;
  });
completableFuture.get();
```

Then we can transform the code using EA's Async.await():

```
try {
    String hello = await(hello());
    String helloWorld = await(mergeWorld(hello));
    await(CompletableFuture.runAsync(() -> print(helloWorld)));
} catch (Exception e) {
    e.printStackTrace();
}
```

The implementation resembles the sequential blocking code; however, the await method doesn't block the code.

As discussed, all calls to the await method will be rewritten by the Async instrumentation to work similarly to the Future.join method.

So once the asynchronous execution of the hello method is finished, the Future result is passed to the mergeWorld method. Then the result is passed to the last execution using the CompletableFuture.runAsync method.

**_Cactoos_**
Cactoos is a Java library based on object-oriented principles.

It's an alternative to Google Guava and Apache Commons that provides common objects for performing various operations.

First, let's add the latest cactoos Maven dependency:

```
<dependency>
    <groupId>org.cactoos</groupId>
    <artifactId>cactoos</artifactId>
    <version>0.43</version>
</dependency>
```

This library provides an Async class for asynchronous operations.

So we can find the factorial of a number using the instance of Cactoos's Async class:

```
Async<Integer, Long> asyncFunction = new Async<Integer, Long>(input -> factorial(input));
Future<Long> asyncFuture = asyncFunction.apply(number);
long result = asyncFuture.get();
```

Here the apply method executes the operation using the ExecutorService.submit method, and returns an instance of the Future interface.

Similarly, the Async class has the exec method that provides the same feature without a return value.

Note: the Cactoos library is in the initial stages of development and may not be appropriate for production use yet.

**_Jcabi-Aspects_**
Jcabi-Aspects provides the @Async annotation for asynchronous programming through AspectJ AOP aspects.

First, let's add the latest jcabi-aspects Maven dependency:

```
<dependency>
    <groupId>com.jcabi</groupId>
    <artifactId>jcabi-aspects</artifactId>
    <version>0.22.6</version>
</dependency>
```

The jcabi-aspects library requires AspectJ runtime support, so we'll add the aspectjrt Maven dependency:

```
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjrt</artifactId>
    <version>1.9.5</version>
</dependency>
```

Next, we'll add the jcabi-maven-plugin plugin that weaves the binaries with AspectJ aspects. The plugin provides the ajc goal that does all the work for us:

```
<plugin>
    <groupId>com.jcabi</groupId>
    <artifactId>jcabi-maven-plugin</artifactId>
    <version>0.14.1</version>
    <executions>
        <execution>
            <goals>
                <goal>ajc</goal>
            </goals>
        </execution>
    </executions>
    <dependencies>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjtools</artifactId>
            <version>1.9.1</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.1</version>
        </dependency>
    </dependencies>
</plugin>
```

Now we're all set to use the AOP aspects for asynchronous programming:

```
@Async
@Loggable
public Future<Long> factorialUsingJcabiAspect(int number) {
    Future<Long> factorialFuture = CompletableFuture.supplyAsync(() -> factorial(number));
    return factorialFuture;
}
```

When we compile the code, the library will inject AOP advice in place of the @Async annotation through AspectJ weaving, for the asynchronous execution of the factorialUsingJcabiAspect method.

Let's compile the class using the Maven command:

```
mvn install
```

The output from the jcabi-maven-plugin may look like:

```
 --- jcabi-maven-plugin:0.14.1:ajc (default) @ java-async ---
[INFO] jcabi-aspects 0.18/55a5c13 started new daemon thread jcabi-loggable for watching of @Loggable annotated methods
[INFO] Unwoven classes will be copied to /tutorials/java-async/target/unwoven
[INFO] jcabi-aspects 0.18/55a5c13 started new daemon thread jcabi-cacheable for automated cleaning of expired @Cacheable values
[INFO] ajc result: 10 file(s) processed, 0 pointcut(s) woven, 0 error(s), 0 warning(s)
```

We can verify if our class is woven correctly by checking the logs in the jcabi-ajc.log file generated by the Maven plugin:

```
Join point 'method-execution(java.util.concurrent.Future
com.baeldung.async.JavaAsync.factorialUsingJcabiAspect(int))'
in Type 'com.baeldung.async.JavaAsync' (JavaAsync.java:158)
advised by around advice from 'com.jcabi.aspects.aj.MethodAsyncRunner'
(jcabi-aspects-0.22.6.jar!MethodAsyncRunner.class(from MethodAsyncRunner.java))
```

Then we'll run the class as a simple Java application, and the output will look like:

```
17:46:58.245 [main] INFO com.jcabi.aspects.aj.NamedThreads -
jcabi-aspects 0.22.6/3f0a1f7 started new daemon thread jcabi-loggable for watching of @Loggable annotated methods
17:46:58.355 [main] INFO com.jcabi.aspects.aj.NamedThreads -
jcabi-aspects 0.22.6/3f0a1f7 started new daemon thread jcabi-async for Asynchronous method execution
17:46:58.358 [jcabi-async] INFO com.baeldung.async.JavaAsync -
#factorialUsingJcabiAspect(20): 'java.util.concurrent.CompletableFuture@14e2d7c1[Completed normally]' in 44.64µs
```

As we can see, a new daemon thread, jcabi-async, is created by the library that performed the task asynchronously.

Similarly, the logging is enabled by the @Loggable annotation provided by the library.

_References:_
https://www.baeldung.com/java-asynchronous-programming
https://www.javatpoint.com/asynchronous-call-in-java
https://medium.com/xebia-engineering/asynchronous-programming-with-java-java-8-d71a5323070e
https://doc.zeroc.com/ice/3.7/language-mappings/java-mapping/client-side-slice-to-java-mapping/asynchronous-method-invocation-ami-in-java

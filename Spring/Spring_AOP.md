**1. What Is Aspect-Oriented Programming (AOP)?**
Aspects enable the modularization of cross-cutting concerns such as transaction management that span multiple types and objects by adding extra behavior to already existing code without modifying affected classes.

**2. What Are Aspect, Advice, Pointcut and JoinPoint in AOP?**

- Aspect – a class that implements cross-cutting concerns, such as transaction management
- Advice – the methods that get executed when a specific JoinPoint with matching Pointcut is reached in the application
- Pointcut – a set of regular expressions that are matched with JoinPoint to determine whether Advice needs to be executed or not
- JoinPoint – a point during the execution of a program, such as the execution of a method or the handling of an exception

**3. What Is Weaving?**
According to the official docs, weaving is a process that links aspects with other application types or objects to create an advised object. This can be done at compile time, load time, or runtime. Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.

**4. What is Aspect, Advice, Pointcut, JointPoint and Advice Arguments in AOP?**
Aspect: Aspect is a class that implements cross-cutting concerns, such as transaction management. Aspects can be a normal class configured and then configured in the Spring Bean configuration file or we can use Spring AspectJ support to declare a class as an Aspect using @Aspect annotation.

Advice: Advice is the action taken for a particular join point. In terms of programming, they are methods that get executed when a specific join point with a matching pointcut is reached in the application. You can think of Advice as Spring interceptors or Servlet Filters.

Pointcut: Pointcuts are regular expressions that are matched with join points to determine whether advice needs to be executed or not. Pointcut uses different kinds of expressions that are matched with the join points. Spring framework uses the AspectJ pointcut expression language for determining the join points where advice methods will be applied.

JoinPoint: A join point is a specific point in the application such as method execution, exception handling, changing object variable values, etc. In Spring AOP a join point is always the execution of a method.

Advice Arguments: We can pass arguments in the advice methods. We can use the args() expression in the pointcut to be applied to any method that matches the argument pattern. If we use this, then we need to use the same name in the advice method from where the argument type is determined.

**5. What is the difference between Spring AOP and AspectJ AOP?**
AspectJ is the industry-standard implementation for Aspect-Oriented Programming whereas Spring implements AOP for some cases. The main differences between Spring AOP and AspectJ are:

Spring AOP is simpler to use than AspectJ because we don’t need to worry about the weaving process.
Spring AOP supports AspectJ annotations, so if you are familiar with AspectJ then working with Spring AOP is easier.
Spring AOP supports only proxy-based AOP, so it can be applied only to method execution join points. AspectJ support all kinds of pointcuts.
One of the shortcomings of Spring AOP is that it can be applied only to the beans created through Spring Context.

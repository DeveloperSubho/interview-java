**1. What Is Spring Framework?**
Spring is the most broadly used framework for the development of Java Enterprise Edition applications. Further, the core features of Spring can be used in developing any Java application.

We use its extensions for building various web applications on top of the Jakarta EE platform. We can also just use its dependency injection provisions in simple standalone applications.

**2. What Are the Benefits of Using Spring?**
Spring targets to make Jakarta EE development easier, so let's look at the advantages:

- Lightweight – There is a slight overhead of using the framework in development.
- Inversion of Control (IoC) – Spring container takes care of wiring dependencies of various objects instead of creating or looking for dependent objects.
- Aspect-Oriented Programming (AOP) – Spring supports AOP to separate business logic from system services.
- IoC container – manages Spring Bean life cycle and project-specific configurations
- MVC framework – used to create web applications or RESTful web services, capable of returning XML/JSON responses
- Transaction management – reduces the amount of boilerplate code in JDBC operations, file uploading, etc., either by using Java annotations or by Spring Bean XML configuration file
- Exception Handling – Spring provides a convenient API for translating technology-specific exceptions into unchecked exceptions.

**3. What Spring Sub-Projects Do You Know? Describe Them Briefly.**

- Core – a key module that provides fundamental parts of the framework, such as IoC or DI
- JDBC – enables a JDBC-abstraction layer that removes the need to do JDBC coding for specific vendor databases
- ORM integration – provides integration layers for popular object-relational mapping APIs, such as JPA, JDO and Hibernate
- Web – a web-oriented integration module that provides multipart file upload, Servlet listeners and web-oriented application context functionalities
- MVC framework – a web module implementing the Model View Controller design pattern
- AOP module – aspect-oriented programming implementation allowing the definition of clean method-interceptors and pointcuts

**4. What Is Dependency Injection?**
Dependency injection, an aspect of Inversion of Control (IoC), is a general concept stating that we do not create our objects manually but instead describe how they should be created. Then an IoC container will instantiate required classes if needed.

Dependency Injection design pattern allows us to remove the hard-coded dependencies and make our application loosely coupled, extendable and maintainable. We can implement the dependency injection pattern to move the dependency resolution from compile-time to runtime.

Some of the benefits of using Dependency Injection are Separation of Concerns, Boilerplate Code reduction, Configurable components, and easy unit testing.

**5. How Can We Inject Beans in Spring?**
A few different options exist in order to inject Spring beans:

- Setter injection
- Constructor injection
- Field injection
  The configuration can be done using XML files or annotations.

For more details, check this article.

**6. Which Is the Best Way of Injecting Beans and Why?**
The recommended approach is to use constructor arguments for mandatory dependencies and setters for optional ones. This is because constructor injection allows injecting values to immutable fields and makes testing easier.

**7. What Is the Difference Between BeanFactory and ApplicationContext?**
BeanFactory is an interface representing a container that provides and manages bean instances. The default implementation instantiates beans lazily when getBean() is called.

In contrast, ApplicationContext is an interface representing a container holding all information, metadata and beans in the application. It also extends the BeanFactory interface, but the default implementation instantiates beans eagerly when the application starts. However, this behavior can be overridden for individual beans.

For all differences, please refer to the documentation.

**8. What Is a Spring Bean?**
The Spring Beans are Java Objects that are initialized by the Spring IoC container.

**9. What Is the Default Bean Scope in Spring Framework?**
By default, a Spring Bean is initialized as a singleton.

**10. How to Define the Scope of a Bean?**
In order to set Spring Bean's scope, we can use @Scope annotation or “scope” attribute in XML configuration files. Note that there are five supported scopes:

- Singleton
- Prototype
- Request
- Session
- Global-session

| Scope          | Description                                                                                                                                                                                                                                                |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| singleton      | Scopes a single bean definition to a single object instance per Spring IoC container.                                                                                                                                                                      |
| prototype      | Scopes a single bean definition to any number of object instances.                                                                                                                                                                                         |
| request        | Scopes a single bean definition to the lifecycle of a single HTTP request; that is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring ApplicationContext. |
| session        | Scopes a single bean definition to the lifecycle of an HTTP Session. Only valid in the context of a web-aware Spring ApplicationContext.                                                                                                                   |
| global session | Scopes a single bean definition to the lifecycle of a global HTTP Session. Typically only valid when used in a portlet context. Only valid in the context of a web-aware Spring ApplicationContext.                                                        |

**11. Are Singleton Beans Thread-Safe?**
No, singleton beans are not thread-safe, as thread safety is about execution, whereas the singleton is a design pattern focusing on creation. Thread safety depends only on the bean implementation itself.

**12. What Does the Spring Bean Life Cycle Look Like?**
First, a Spring bean needs to be instantiated based on Java or XML bean definition. It may also be required to perform some initialization to get it into a usable state. After that, when the bean is no longer required, it will be removed from the IoC container.

The whole cycle with all initialization methods is shown in the image
![Spring Bean Life Cycle](/Screenshots/Spring-Bean-Life-Cycle.png)

**13. What Is the Spring Java-Based Configuration?**
It's one of the ways of configuring Spring-based applications in a type-safe manner. It's an alternative to the XML-based configuration.

Also, to migrate a project from XML to Java config, please refer to this article.

**14. Can We Have Multiple Spring Configuration Files in One Project?**
Yes, in large projects, having multiple Spring configurations is recommended to increase maintainability and modularity.

We can load multiple Java-based configuration files:

@Configuration
@Import({MainConfig.class, SchedulerConfig.class})
public class AppConfig {
Or we can load one XML file that will contain all other configs:

ApplicationContext context = new ClassPathXmlApplicationContext("spring-all.xml");
And inside this XML file we'll have the following:

<import resource="main.xml"/>
<import resource="scheduler.xml"/>

**15. What Is Spring Security?**
Spring Security is a separate module of the Spring framework that focuses on providing authentication and authorization methods in Java applications. It also takes care of most of the common security vulnerabilities such as CSRF attacks.

To use Spring Security in web applications, we can get started with the simple annotation @EnableWebSecurity.

For more information, we have a whole series of articles related to security.

**16. What Is Spring Boot?**
Spring Boot is a project that provides a pre-configured set of frameworks to reduce boilerplate configuration. This way, we can have a Spring application up and running with the smallest amount of code.

**17. Name Some of the Design Patterns Used in the Spring Framework?**
Singleton Pattern – singleton-scoped beans
Factory Pattern – Bean Factory classes
Prototype Pattern – prototype-scoped beans
Adapter Pattern – Spring Web and Spring MVC
Proxy Pattern – Spring Aspect-Oriented Programming support
Template Method Pattern – JdbcTemplate, HibernateTemplate, etc.
Front Controller – Spring MVC DispatcherServlet
Data Access Object – Spring DAO support
Model View Controller – Spring MVC

**18. How Does the Scope Prototype Work?**
Scope prototype means that every time we call for an instance of the Bean, Spring will create a new instance and return it. This differs from the default singleton scope, where a single object instance is instantiated once per Spring IoC container.

**19. Name some of the important Spring Modules?**
Some of the important Spring Framework modules are:

Spring Context – for dependency injection.
Spring AOP – for aspect-oriented programming.
Spring DAO – for database operations using DAO pattern
Spring JDBC – for JDBC and DataSource support.
Spring ORM – for ORM tools support such as Hibernate
Spring Web Module – for creating web applications.
Spring MVC – Model-View-Controller implementation for creating web applications, web services, etc.

**20. What is Bean wiring and @Autowired annotation?**
The process of injection spring bean dependencies while initializing is called Spring Bean Wiring.

Usually, it’s best practice to do the explicit wiring of all the bean dependencies, but the spring framework also supports auto-wiring. We can use @Autowired annotation with fields or methods for autowiring byType. For this annotation to work, we also need to enable annotation-based configuration in spring bean configuration file. This can be done by context:annotation-config element.

**21. What are different types of Spring Bean autowiring?**
There are four types of autowiring in Spring framework.

autowire byName
autowire byType
autowire by constructor
autowiring by @Autowired and @Qualifier annotations

22. Does Spring Bean provide thread safety?
    The default scope of Spring bean is singleton, so there will be only one instance per context. That means that all the having a class level variable that any thread can update will lead to inconsistent data. Hence in default mode spring beans are not thread-safe.

However, we can change spring bean scope to request, prototype or session to achieve thread-safety at the cost of performance. It’s a design decision and based on the project requirements.

23. What is a Controller in Spring MVC?
    Just like the MVC design pattern, the Controller is the class that takes care of all the client requests and sends them to the configured resources to handle them. In Spring MVC, DispatcherServlet is the front controller class that initializes the context based on the spring beans configurations.

A Controller class is responsible to handle a different kind of client requests based on the request mappings. We can create a controller class by using @Controller annotation. Usually, it’s used with @RequestMapping annotation to define handler methods for specific URI mapping.

24. What is DispatcherServlet and ContextLoaderListener?
    DispatcherServlet is the front controller in the Spring MVC application and it loads the spring bean configuration file and initializes all the beans that are configured. If annotations are enabled, it also scans the packages and configures any bean annotated with @Component, @Controller, @Repository, or @Service annotations.

ContextLoaderListener is the listener to start up and shut down Spring’s root WebApplicationContext. Its important functions are to tie up the lifecycle of ApplicationContext to the lifecycle of the ServletContext and to automate the creation of ApplicationContext. We can use it to define shared beans that can be used across different spring contexts.

**25. What is ViewResolver in Spring?**
ViewResolver implementations are used to resolve the view pages by name. We configure it in the spring bean configuration file. For example:

<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->

<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<beans:property name="prefix" value="/WEB-INF/views/" />
<beans:property name="suffix" value=".jsp" />
</beans:bean>
InternalResourceViewResolver is one of the implementation of ViewResolver interface and we are providing the view pages directory and suffix location through the bean properties. So if a controller handler method returns “home”, view resolver will use view page located at /WEB-INF/views/home.jsp.

**26. How to handle exceptions in Spring MVC Framework?**
Spring MVC Framework provides the following ways to help us achieving robust exception handling.

Controller-Based – We can define exception handler methods in our controller classes. All we need is to annotate these methods with @ExceptionHandler annotation.

Global Exception Handler – Exception Handling is a cross-cutting concern and Spring provides @ControllerAdvice annotation that we can use with any class to define our global exception handler.

HandlerExceptionResolver implementation – For generic exceptions, most of the time we serve static pages. Spring Framework provides a HandlerExceptionResolver interface that we can implement to create a global exception handler. The reason behind this additional way to define the global exception handler is that the Spring framework also provides default implementation classes that we can define in our spring bean configuration file to get spring framework exception handling benefits.

**27. How to validate form data in Spring Web MVC Framework?**
Spring supports JSR-303 annotation-based validations as well as provides a Validator interface that we can implement to create our own custom validator. For using JSR-303 based validation, we need to annotate bean variables with the required validations.

For custom validator implementation, we need to configure it in the controller class.

**28. What is Spring MVC Interceptor and how to use it?**
Spring MVC Interceptors are like Servlet Filters and allow us to intercept client requests and process them. We can intercept client requests at three places – preHandle, postHandle, and afterCompletion.

We can create a spring interceptor by implementing the HandlerInterceptor interface or by extending the abstract class HandlerInterceptorAdapter.

We need to configure interceptors in the spring bean configuration file. We can define an interceptor to intercept all the client requests or we can configure it for specific URI mapping too.

**29. What are some of the best practices for Spring Framework?**
Some of the best practices for Spring Framework are:

- Avoid version numbers in schema reference, to make sure we have the latest configs.
- Divide spring bean configurations based on their concerns such as spring-jdbc.xml, spring-security.xml.
- For spring beans that are used in multiple contexts in Spring MVC, create them in the root context and initialize with the listener.
- Configure bean dependencies as much as possible, try to avoid auto wiring as much as possible.
- For application-level properties, the best approach is to create a property file and read it in the spring bean configuration file.
- For smaller applications, annotations are useful but for larger applications, annotations can become a pain. If we have all the configuration in XML files, maintaining it will be easier.
- Use correct annotations for components for understanding the purpose easily. For services use @Service and for DAO beans use @Repository.
- Spring framework has a lot of modules, use what you need. Remove all the extra dependencies that get usually added when you create projects through Spring Tool Suite templates.
- If you are using Aspects, make sure to keep the join pint as narrow as possible to avoid advice on unwanted methods. Consider custom annotations that are easier to use and avoid any issues.
- Use dependency injection when there is an actual benefit, just for the sake of loose-coupling don’t use it because it’s harder to maintain.

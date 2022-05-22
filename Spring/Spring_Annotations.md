**1. Difference between @Component, @Service, @Controller, and @Repository in Spring**  
@Component and @Controller are the same with respect to bean creation and dependency injection but later is a specialized form of former. Even if you replace @Controller annotation with @Compoenent, Spring can automatically detect and register the controller class but it may not work as you expect with respect to request mapping.

The same is true for @Service and @Repository annotation, they are a specialization of @Component in service and persistence layer. A Spring bean in the service layer should be annotated using @Service instead of @Component annotation and a spring bean in the persistence layer should be annotated with @Repository annotation.

By using a specialized annotation we hit two birds with one stone. First, they are treated as Spring bean, and second, you can put special behavior required by that layer.

- @Component is a generic stereotype for any Spring-managed component or bean.
- @Repository is a stereotype for the persistence layer.
- @Service is a stereotype for the service layer.
- @Controller is a stereotype for the presentation layer (spring-MVC).
  And here is the nice diagram to explain the hierarchy of all these annotations in Spring Framework:

  ![Component Annotation](/Screenshots/Component_Annotation.png)

**2. What’s the difference between @Component, @Controller, @Repository & @Service annotations in Spring?**  
@Component is used to indicate that a class is a component. These classes are used for auto-detection and configured as a bean when annotation-based configurations are used.

@Controller is a specific type of component, used in MVC applications and mostly used with RequestMapping annotation.

@Repository annotation is used to indicate that a component is used as a repository and a mechanism to store/retrieve/search data. We can apply this annotation with DAO pattern implementation classes.

@Service is used to indicate that a class is a Service. Usually, the business facade classes that provide some services are annotated with this.

We can use any of the above annotations for a class for auto-detection but different types are provided so that you can easily distinguish the purpose of the annotated classes.

**3. What are some of the important Spring annotations you have used?**  
Some of the Spring annotations that I have used in my project are:

- @Controller – for controller classes in Spring MVC project.
- @RequestMapping – for configuring URI mapping in controller handler methods. This is a very important annotation, so you should go through Spring MVC RequestMapping Annotation Examples
- @ResponseBody – for sending Object as a response, usually for sending XML or JSON data as a response.
- @PathVariable – for mapping dynamic values from the URI to handler method arguments.
- @Autowired – for auto wiring dependencies in spring beans.
- @Qualifier – with @Autowired annotation to avoid confusion when multiple instances of bean type are present.
- @Service – for service classes.
- @Scope – for configuring scope of the spring bean.
- @Configuration, @ComponentScan, and @Bean – for java based configurations.
AspectJ annotations for configuring aspects and advice, @Aspect, @Before, @After, @Around, @Pointcut, etc.

**_Important Spring Annotations_**

- @Configuration - used to mark a class as a source of the bean definitions. Beans are the components of the system that you want to wire together. A method marked with the @Bean annotation is a bean producer. Spring will handle the life cycle of the beans for you, and it will use these methods to create the beans.
- @ComponentScan -use to make sure that Spring knows about your configuration classes and can initialize the beans correctly. It makes Spring scan the packages configured with it for the @Configuration classes.
- @Import - If you need even more precise control of the configuration classes, you can always use @import to load additional configuration. This one works even when you specify the beans in an XML file like it's 1999.
- @Component - Another way to declare a bean is to mark a class with a @Component annotation. Doing this turns the class into a Spring bean at the auto-scan time.
- @Service - Mark a specialization of a @Component. It tells Spring that it's safe to manage them with more freedom than regular components. Remember, services have no encapsulated state.
- @Autowired - To wire the application parts together, use the @Autowired on the fields, constructors, or methods in a component. Spring's dependency injection mechanism wires appropriate beans into the class members marked with @Autowired.
- @Bean - A method-level annotation to specify a returned bean to be managed by Spring context. The returned bean has the same name as the factory method.
- @Lookup - tells Spring to return an instance of the method's return type when we invoke it.
- @Primary - gives higher preference to a bean when there are multiple beans of the same type.
- @Required - shows that the setter method must be configured to be dependency-injected with a value at configuration time. Use @Required on setter methods to mark dependencies populated through XML. Otherwise, a BeanInitializationException is thrown.
- @Value - used to assign values into fields in Spring-managed beans. It's compatible with the constructor, setter, and field injection.
- @DependsOn - makes Spring initialize other beans before the annotated one. Usually, this behavior is automatic, based on the explicit dependencies between beans. The @DependsOn annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.
- @Lazy - makes beans to initialize lazily. @Lazy annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.
- @Scope - used to define the scope of a @Component class or a @Bean definition and can be either singleton, prototype, request, session, globalSession, or custom scope.
- @Profile - adds beans to the application only when that profile is active.
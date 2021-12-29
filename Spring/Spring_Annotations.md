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

@Controller – for controller classes in Spring MVC project.
@RequestMapping – for configuring URI mapping in controller handler methods. This is a very important annotation, so you should go through Spring MVC RequestMapping Annotation Examples
@ResponseBody – for sending Object as a response, usually for sending XML or JSON data as a response.
@PathVariable – for mapping dynamic values from the URI to handler method arguments.
@Autowired – for auto wiring dependencies in spring beans.
@Qualifier – with @Autowired annotation to avoid confusion when multiple instances of bean type are present.
@Service – for service classes.
@Scope – for configuring scope of the spring bean.
@Configuration, @ComponentScan, and @Bean – for java based configurations.
AspectJ annotations for configuring aspects and advice, @Aspect, @Before, @After, @Around, @Pointcut, etc.

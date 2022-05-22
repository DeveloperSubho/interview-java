**_Spring Core Annotations_**

- @Configuration - @Configuration is used on classes that define beans. @Configuration is an analog for an XML configuration file – it is configured using Java classes. A Java class annotated with @Configuration is a configuration by itself and will have methods to instantiate and configure the dependencies.

- @ComponentScan - @ComponentScan is used with the @Configuration annotation to allow Spring to know the packages to scan for annotated components. @ComponentScan is also used to specify base packages using basePackageClasses or basePackage attributes to scan. If specific packages are not defined, scanning will occur from the package of the class that declares this annotation.

- @Autowired - @Autowired is used to mark a dependency which Spring is going to resolve and inject automatically. We can use this annotation with a constructor, setter, or field injection.

- @Component - @Component is used on classes to indicate a Spring component. The @Component annotation marks the Java class as a bean or component so that the component-scanning mechanism of Spring can add it into the application context.

- @Bean - @Bean is a method-level annotation and a direct analog of the XML element. @Bean marks a factory method which instantiates a Spring bean. Spring calls these methods when a new instance of the return type is required.

- @Qualifier - @Qualifier helps fine-tune annotation-based autowiring. There may be scenarios when we create more than one bean of the same type and want to wire only one of them with a property. This can be controlled using @Qualifier annotation along with the @Autowired annotation.
  We use @Qualifier along with @Autowired to provide the bean ID or bean name we want to use in ambiguous situations.

- @Primary - We use @Primary to give higher preference to a bean when there are multiple beans of the same type. When a bean is not marked with @Qualifier, a bean marked with @Primary will be served in case on ambiquity.

- @Required - The @Required annotation is method-level annotation, and shows that the setter method must be configured to be dependency-injected with a value at configuration time.
  @Required on setter methods to mark dependencies we want to populate through XML; Otherwise, BeanInitializationException will be thrown.

- @Value - Spring @Value annotation is used to assign default values to variables and method arguments. We can read spring environment variables as well as system variables using @Value annotation.
  We can use @Value for injecting property values into beans. It’s compatible with the constructor, setter, and field injection.

- @DependsOn - @DependsOn makes Spring initialize other beans before the annotated one. Usually, this behavior is automatic, based on the explicit dependencies between beans.
  The @DependsOn annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.

- @Lazy - @Lazy makes beans to initialize lazily. By default, the Spring IoC container creates and initializes all singleton beans at the time of application startup.
  @Lazy annotation may be used on any class directly or indirectly annotated with @Component or on methods annotated with @Bean.

- @Lookup - A method annotated with @Lookup tells Spring to return an instance of the method’s return type when we invoke it.

- @Scope - @Scope is used to define the scope of a @Component class or a @Bean definition. It can be either singleton, prototype, request, session, globalSession or some custom scope.

- @Profile - Beans marked with @Profile will be initialized in the container by Spring only when that profile is active. By Default, all beans has “default” value as Profile. We can configure the name of the profile with the value argument of the annotation.

- @Import - @Import allows to use specific @Configuration classes without component scanning. We can provide those classes with @Import‘s value argument.

- @ImportResource - @ImportResource allows to import XML configurations with this annotation. We can specify the XML file locations with the locations argument, or with its alias, the value argument.

- @PropertySource - @PropertySource annotation provides a convenient and declarative mechanism for adding a PropertySource to Spring’s Environment. To be used in conjunction with @Configuration classes.
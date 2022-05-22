**_Spring Boot Annotations_**

- @SpringBootApplication:  
  @SpringBootApplication marks the main class of a Spring Boot application. This is used usually on a configuration class that declares one or more @Bean methods and also triggers auto-configuration and component scanning.
  The @SpringBootApplication annotation is equivalent to using @Configuration, @EnableAutoConfiguration, and @ComponentScan with their default attributes.
  One of the most basic and helpful annotations, is @SpringBootApplication. It's syntactic sugar for combining other annotations that we'll look at in just a moment.

- @Configuration and @ComponentScan:  
  The @Configuration and @ComponentScan annotations that we described above make Spring create and configure the beans and components of your application. It's a great way to decouple the actual business logic code from wiring the app together.

- @EnableAutoConfiguration:  
  @EnableAutoConfiguration tells Spring Boot to look for auto-configuration beans on its classpath and automatically applies them. It tells Spring Boot to “guess” how you want to configure Spring based on the jar dependencies that you have added.
  Since spring-boot-starter-web dependency added to classpath leads to configure Tomcat and Spring MVC, the auto-configuration assumes that you are developing a web application and sets up Spring accordingly. This annotation is used with @Configuration.
  Now the @EnableAutoConfiguration annotation is even better. It makes Spring guess the configuration based on the JAR files available on the classpath. It can figure out what libraries you use and preconfigure their components without you lifting a finger. It is how all the spring-boot-starter libraries work. Meaning it's a major lifesaver both when you're just starting to work with a library as well as when you know and trust the default config to be reasonable.

- @ConditionalOnClass and @ConditionalOnMissingClass:  
  The @ConditionalOnClass and @ConditionalOnMissingClass annotations let configuration be included based on the presence or absence of specific classes. With these annotations, Spring will only use the marked auto-configuration bean if the class in the annotation’s argument is present/absent.

- @ConditionalOnBean and @ConditionalOnMissingBean:  
  @ConditionalOnBean and @ConditionalOnMissingBean annotations let a bean be included based on the presence or absence of specific beans.

- @ConditionalOnProperty:  
  @ConditionalOnProperty annotation lets configuration be included based on a Spring Environment property i.e. make conditions on the values of properties.

- @ConditionalOnResource:  
  @ConditionalOnResource annotation lets configuration be included only when a specific resource is present.

- @ConditionalOnWebApplication and @ConditionalOnNotWebApplication:  
  @ConditionalOnWebApplication and @ConditionalOnNotWebApplication annotations let configuration be included depending on whether the application is a “web application”. A web application is an application that uses a spring WebApplicationContext, defines a session scope, or has a StandardServletEnvironment.

- @ConditionalExpression:  
  @ConditionalExpression is used in more complex situations. Spring will use the marked definition when the SpEL expression is evaluated to true.

- @Conditional:  
  @Conditional is used in complex conditions, we can create a class evaluating the custom condition.

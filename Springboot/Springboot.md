**1. @Configuration vs @Component:**
_@Configuration_ Indicates that a class declares one or more @Bean methods and may be processed by the Spring container to generate bean definitions and service requests for those beans at runtime

_@Component_ Indicates that an annotated class is a "component". Such classes are considered as candidates for auto-detection when using annotation-based configuration and classpath scanning.

_@Configuration_ is meta-annotated with _@Component_, therefore @Configuration classes are candidates for component scanning

**2. How to create Spring Boot application**
a. Spring Boot CLI
b. Spring Starter Project Wizard
c. Spring Initializr
d. Spring Maven Project

**3. Mention the possible sources of external configuration**
There is no doubt in the fact that Spring Boot allows the developers to run the same application in different environments. Well, this is done with the support it provides for external configuration. It uses environment variables, properties files, command-line arguments, YAML files, and system properties to mention the required configuration properties. Also, the @value annotation is used to gain access to the properties. So, the most possible sources of external configuration are as follows:

- Application Properties – By default, Spring Boot searches for the application properties file or its YAML file in the current directory, classpath root or config directory to load the properties.

- Command-line properties – Spring Boot provides command-line arguments and converts these arguments to properties. Then it adds them to the set of environment properties.

- Profile-specific properties – These properties are loaded from the application-{profile}.properties file or its YAML file. This file resides in the same location as that of the non-specific property files and the{profile} placeholder refers to an active profile.

**4. Can you explain what happens in the background when a Spring Boot Application is “Run as Java Application”?**
When a Spring Boot application is executed as “Run as Java application”, then it automatically launches up the tomcat server as soon as it sees, that you are developing a web application.

**5. What are the Spring Boot starters and what are available the starters?**
Spring Boot starters are a set of convenient dependency management providers which can be used in the application to enable dependencies. These starters, make development easy and rapid. All the available starters come under the org.springframework.boot group. Few of the popular starters are as follows:

- _spring-boot-starter_ – This is the core starter and includes logging, auto-configuration support, and YAML.
- _spring-boot-starter-jdbc_ – This starter is used for HikariCP connection pool with JDBC
- _spring-boot-starter-web_ – Is the starter for building web applications, including RESTful, applications using Spring MVC
- _spring-boot-starter-data-jpa_ – Is the starter to use Spring Data JPA with Hibernate
- _spring-boot-starter-security_ – Is the starter used for Spring Security
- _spring-boot-starter-aop_- This starter is used for aspect-oriented programming with AspectJ and Spring AOP
- _spring-boot-starter-test_- Is the starter for testing Spring Boot applications

**6. Explain Spring Actuator and its advantages**
Spring Actuator is a cool feature of Spring Boot with the help of which you can see what is happening inside a running application. So, whenever you want to debug your application, and need to analyze the logs you need to understand what is happening in the application right? In such a scenario, the Spring Actuator provides easy access to features such as identifying beans, CPU usage, etc. The Spring Actuator provides a very easy way to access the production-ready REST points and fetch all kinds of information from the web. These points are secured using Spring Security’s content negotiation strategy.

**7. Spring boot Dependency Management**
https://www.javatpoint.com/spring-boot-dm

**8. Explain what is thymeleaf and how to use thymeleaf?**
Thymeleaf is a server-side Java template engine used for web applications. It aims to bring natural template for your web application and can integrate well with Spring Framework and HTML5 Java web applications. To use Thymeleaf, you need to add the following code in the pom.xml file:
<dependency>  
 <groupId>org.springframework.boot</groupId>  
 <artifactId>spring-boot-starter-thymeleaf</artifactId>  
 </dependency>

**9. What is the need for Spring Boot DevTools?**
https://www.baeldung.com/spring-boot-devtools
Spring Boot Dev Tools are an elaborated set of tools and aims to make the process of developing an application easier. If the application runs in the production, then this module is automatically disabled, repackaging of archives are also excluded by default. So, the Spring Boot Developer Tools applies properties to the respective development environments. To include the DevTools, you just have to add the following dependency into the pom.xml file:
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-devtools</artifactId>
</dependency>

**10. How to enable HTTP/2 support in Spring Boot**
You can enable the HTTP/2 support in Spring Boot by: server.http2.enabled=true

**11. Spring boot JDBC**
https://www.javatpoint.com/spring-boot-jdbc

**12. How can we create a custom endpoint in Spring Boot Actuator?**
To create a custom endpoint in Spring Boot 2.x, you can use the @Endpoint annotation. Spring Boot also exposes endpoints using @WebEndpointor, @WebEndpointExtension over HTTP with the help of Spring MVC, Jersey, etc.

**13. What do you understand by auto-configuration in Spring Boot and how to disable the auto-configuration?**
Auto-configuration is used to automatically configure the required configuration for the application. For example, if you have a data source bean present in the classpath of the application, then it automatically configures the JDBC template. With the help of auto-configuration, you can create a Java application in an easy way, as it automatically configures the required beans, controllers, etc.

To disable the auto-configuration property, you have to exclude attribute of @EnableAutoConfiguration, in the scenario where you do not want it to be applied.

@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
If the class is not on the classpath, then to exclude the auto-configuration, you have to mention the following code:

@EnableAutoConfiguration(excludeName={Sample.class})
Apart from this, Spring Boot also provides the facility to exclude list of auto-configuration classes by using the spring.autoconfigure.exclude property. You can go forward, and add it either in the application.properties or add multiple classes with comma-separated.

**14. What are the steps to deploy Spring Boot web applications as JAR and WAR files?**
To deploy a Spring Boot web application, you just have to add the following plugin in the pom.xml file:
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
By using the above plugin, you will get a JAR executing the package phase. This JAR will contain all the necessary libraries and dependencies required. It will also contain an embedded server. So, you can basically run the application like an ordinary JAR file.
Note: The packaging element in the pom.xml file must be set to jar to build a JAR file as below:

<packaging>jar</packaging>
Similarly, if you want to build a WAR file, then you will mention

<packaging>war</packaging>

**15. Mention the advantages of the YAML file than Properties file and the different ways to load YAML file in Spring boot**
The advantages of the YAML file than a properties file is that the data is stored in a hierarchical format. So, it becomes very easy for the developers to debug if there is an issue. The SpringApplication class supports the YAML file as an alternative to properties whenever you use the SnakeYAML library on your classpath. The different ways to load a YAML file in Spring Boot is as follows:

Use YamlMapFactoryBean to load YAML as a Map
Use YamlPropertiesFactoryBean to load YAML as Properties

**16. How is Hibernate chosen as the default implementation for JPA without any configuration?**
When we use the Spring Boot Auto Configuration, automatically the spring-boot-starter-data-jpa dependency gets added to the pom.xml file. Now, since this dependency has a transitive dependency on JPA and Hibernate, Spring Boot automatically auto-configures Hibernate as the default implementation for JPA, whenever it sees Hibernate in the classpath.

**17. What do you understand by Spring Data REST?**
Spring Data REST is used to expose the RESTful resources around Spring Data repositories. Consider the following example:

@RepositoryRestResource(collectionResourceRel = "sample", path = "sample")
public interface SampleRepository
extends CustomerRepository<sample, Long> {
Now, to expose the REST services, you can use the POST method in the following way:
{
"customername": "Rohit"
}
Response Content
{
"customername": "Rohit"
"\_links": {
"self": {
"href": "<a href="http://localhost:8080/sample/1">http://localhost:8080/sample/1</a>"
},
"sample": {
"href": "<a href="http://localhost:8080/sample/1">http://localhost:8080/sample/1</a>"
}
}

**18. In which layer, should the boundary of a transaction start?**
The boundary of the transaction should start from the Service Layer since the logic for the business transaction is present in this layer itself.

**19. Explain how to register a custom auto-configuration.**
In order to register an auto-configuration class, you have to mention the fully-qualified name under the @EnableAutoConfiguration key META-INF/spring. factories file. Also, if we build the with maven, then this file should be placed in the resources/META-INT directory.

**20. How do you Configure Log4j for logging?**
Since Spring Boot supports Log4j2 for logging a configuration, you have to exclude Logback and include Log4j2 for logging. This can be only done if you are using the starters project.

**21. What are the steps to add a custom JS code with Spring Boot?**

The steps to add a custom JS code with Spring Boot are as follows:

Now, create a folder and name it static under the resources folder
In this folder, you can put the static content in that folder
Note: Just in case, the browser throws an unauthorized error, you either disable the security or search for the password in the log file, and eventually pass it in the request header. 22. How to instruct an auto-configuration to back off when a bean exists?
To instruct an auto-configuration class to back off when a bean exists, you have to use the @ConditionalOnMissingBean annotation. The attributes of this annotation are as follows:

value: This attribute stores the type of beans to be checked
name: This attribute stores the name of beans to be checked

**23. Why is Spring Data REST not recommended in real-world applications?**
Spring Data REST is not recommended in real-world applications as you are exposing your database entities directly as REST Services. While designing RESTful services, the two most important things that we consider is the domain model and the consumers. But, while using Spring Data REST, none of these parameters are considered. The entities are directly exposed. So, I would just say, you can use Spring Data REST, for the initial evolution of the project.

**24. What do you understand by Spring Boot supports relaxed binding?**
Relaxed binding, is a way in which, the property name does not need to match the key of the environment property. In Spring Boot, relaxed binding is applicable to the type-safe binding of the configuration properties. For example, if a property in a bean class with the @ConfigurationPropertie annotation is used sampleProp, then it can be bounded to any of the following environment properties:

sampleProp
sample-Prop
sample_Prop
SAMPLE_PROP

**25. How to exclude any package without using the basePackages filter?**

There are different ways you can filter any package. But Spring Boot provides a trickier option for achieving this without touching the component scan. You can use the exclude attribute while using the annotation @SpringBootApplication. See the following code snippet:

@SpringBootApplication(exclude= {Employee.class})
public class FooAppConfiguration {}

**26. How to disable a specific auto-configuration class?**

You can use the exclude attribute of@EnableAutoConfiguration, if you find any specific auto-configuration classes that you do not want are being applied.

**27. What is a shutdown in the actuator?**

Shutdown is an endpoint that allows the application to be gracefully shutdown. This feature is not enabled by default. You can enable this by using management.endpoint.shutdown.enabled=true in your application.properties file. But be careful about this if you are using this.

**28. Which Is a Better Way to Configure a Spring Boot Project – Using Properties or YAML?**

YAML offers many advantages over properties files, such as:

More clarity and better readability
Perfect for hierarchical configuration data, which is also represented in a better, more readable format
Support for maps, lists, and scalar types
Can include several profiles in the same file

**29. What is Spring Boot?**
In simple words, Spring Boot Framework is Auto-Dependency Resolution, Auto-Configuration, Management EndPoints, Embedded HTTP Servers(Jetty/Tomcat etc.) and Spring Boot CLI

![Springboot](/Screenshots/Springboot.png)

In other words, Spring Boot Framework is Spring Boot Starter, Spring Boot Auto-Configurator, Spring Boot Actuator, Embedded HTTP Servers, and Groovy.

Why we need Spring Boot?
Spring Framework aims to simplify Java Applications Development.
Spring Boot Framework aims to simplify Spring Development.

What is Spring Boot Starter?
Spring Boot Starters are just JAR Files. They are used by Spring Boot Framework to provide “Auto-Dependency Resolution”.

What is Spring Boot AutoConfigurator?
Spring Boot AutoConfigurator is used by Spring Boot Framework to provide “Auto-Configuration”.

What is Spring Boot Actuator?
Spring Boot Actuator is used by Spring Boot Framework to provide “Management EndPoints” to see Application Internals, Metrics etc.

What is Spring Boot CLI?
In simple words, Spring Boot CLI is Auto Dependency Resolution, Auto-Configuration, Management EndPoints, Embedded HTTP Servers(Jetty, Tomcat etc.) and (Groovy, Auto-Imports)

With Spring Boot CLI:

No Semicolons
No Public and private access modifiers
No Imports(Most)
No “return” statement
No setters and getters
No Application class with main() method(It takes care by SpringApplication class).
No Gradle/Maven builds.
No separate HTTP Servers.

What is Spring Boot Initilizr?
Spring Boot Initilizr is a Spring Boot tool to bootstrap Spring Boot or Spring Applications very easily.

Spring Boot Initilizr comes in the following forms:

Spring Boot Initilizr With Web Interface
Spring Boot Initilizr With IDEs/IDE Plugins
Spring Boot Initilizr With Spring Boot CLI
Spring Boot Initilizr With ThirdParty Tools
Why we need Spring Boot Initilizr?
Spring Boot Initilizr simplifies Spring Applications Development by providing initial project structure and build scripts.

It reduces Development time
It increases Productivity

Spring Boot With Maven/Gradle?
Spring Boot Framework uses one of the greatest features of Maven/Gradle build tools: “Transitively Dependency Resolution Management”.

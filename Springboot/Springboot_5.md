1.  What is Spring Boot?
Spring Boot is called a microservice framework that is built on top of the spring framework. This can help developers to focus more on convention rather than configuration.

The main aim of Spring boot is to give you a production-ready application. So, the moment you create a spring-boot project, it is runnable and can be executed/deployed on the server. 
It comes with features like autoconfiguration, auto dependency resolution, embedded servers, security, health checks which enhances the productivity of a developer.
2. How to create Spring Boot project in eclipse?
One of the ways to create a spring boot project in eclipse is by using Spring Initializer.

You can go to the official website of spring and add details such as version, select maven or Gradle project, add your groupId, artifactId, select your required dependencies and then click on CREATE PROJECT. 

Once the project is created, you can download it and extract and import it in your eclipse or STS.

And see your project is ready!

3. How to deploy spring boot application in tomcat?
Whenever you will create your spring boot application and run it, Spring boot will automatically detect the embedded tomcat server and deploy your application on tomcat.
After successful execution of your application, you will be able to launch your rest endpoints and get a response.

4. What is the difference between Spring and Spring Boot?
Difference between Spring and Spring boot are as follows:

Spring –

Is a dependency injection framework.
It is basically used to manage the life cycle of java classes (beans). It consists of a lot of boilerplate configuration.
Uses XML based configuration.
It takes time to have a spring application up and running and it’s mainly because of boilerplate code.
Spring boot- 

It is a suite of pre- configured frameworks and technologies which helps to remove boilerplate configuration.
Uses annotations.
It is used to create a production-ready code.
5. What is actuator in spring boot?
An actuator is one of the best parts of spring boot which consists of production-ready features to help you monitor and manage your application. 

With the help of an actuator, you can monitor what is happening inside the running application.
Actuator dependency figures out the metrics and makes them available as a new endpoint in your application and retrieves all required information from the web. You can identify beans, the health status of your application, CPU usage, and many more with the actuator.

6. How to change port in spring boot?
The default port number to start your SpringBoot application is 8080.

Just to change the port number, you need to add server.port=8084c(your port number) property in your application.properties file and start your application.

7. How to install spring boot in eclipse?
The classic and preferred way to install STS in eclipse is:

Go to Eclipse IDE, click on “Help”->then go to Eclipse marketplace->and type Spring IDE and click on the finish button.

8. How to create war file in spring boot?
To create a war file in spring boot you need to define your packaging file as war in your pom.xml(if it is maven project).

Then just do maven clean and install so that your application will start building. Once the build is successful, just go into your Target folder and you can see .war file generated for your application.                   

9. What is JPA in spring boot?
JPA is basically Java Persistence API. It’s a specification that lets you do ORM when you are connecting to a relational database which is Object-Relational Mapping. 

So, when you need to connect from your java application to relational database, you need to be able to use something like JDBC and run SQL queries and then you get the results and convert them into Object instances. 

ORM lets you map your entity classes in your SQL tables so that when you connect to the database , you don’t need to do query yourself, it’s the framework that handles it for you.

And JPA is a way to use ORM, it’s an API which lets you configure your entity classes and give it to a framework so that the framework does the rest.

10. How to save image in database using spring boot?
First configure mysql in your spring boot application.
Then you can map your entities with your db tables using JPA.
And with the help of save() method in JPA you can directly insert your data into your database
1
2
3
4
5
6
7
@RestController
@RequestMapping("/greatleasrning")
public class Controller {
@Autowired
private final GreatLearningRepository greatLearningRepository;
public Controller(GreatLearningRepository greatLearningRepository) {
}
In above case, your data which may be in JSON format can be inserted successfully into database.

1
2
3
4
5
6
@RequestMapping(method = RequestMethod.POST)
ResponseEntity<?> insert(@RequestBody Course course) {
greatLearningRepository.save(course);
 return ResponseEntity.accepted().build();
}
}
11. What is auto configuration in spring boot?
AutoConfiguration is a process by which Spring Boot automatically configures all the infrastructural beans. It declares the built-in beans/objects of the spring specific module such as JPA, spring security and so on based on the dependencies present in your applications class path.

For example: If we make use of Spring JDBC, the spring boot autoconfiguration feature automatically registers the DataSource and JDBCTemplete bean.
This entire process of automatically declaring the framework specific bean without the need of writing the xml code or java config code explicity  is called Autoconfiguration which is done by springBoot with the help of an annotation called @EnableAutoconfiguration alternatively @SpringBootApplication.

12. How to change port number in spring boot?
The default port number to start your Spring Boot application is 8080.
Just to change the port number, you need to add server.port=8084(your port number) property in your application.properties file and start your application.

13. How to resolve whitelabel error page in spring boot application?
This is quite common error in spring boot application which says 404(page not found).

We can mostly resolve this in 3 ways:

Custom Error Controller– where you will be implementing ErrorController  interface which is provided by SpringFramework and then overriding its getErrorPath() so that you can return a custom path whenever such type of error is occurred.
By Displaying Custom error page– All you have to do is create an error.html page and place it into the src/main/resources/templates path. The BasicErrorController of of springboot will automatically pick this file by default.
By disabling the whitelabel error page– this is the easiest way where all you need to do is server.error.whitelabel.enabled property to false in the application.properties file to disable the whitelabel error page.
14. How to fetch data from database in spring boot?
You can use the following steps to connect your application with MySQL database.
1. First create a database in MySQL with create DATABASE student;
2. Now, create a table inside this DB:
CREATE TABLE student(studentid INT PRIMARY KEY NOT NULL AUTO_INCREMENT, studentname VARCHAR(255)); 
3. Create a SpringBoot application and add JDBC, MySQL and web dependencies.
4. In application.properties, you need to configure the database.

1
2
3
4
spring.datasource.url=jdbc:mysql://localhost:3306/studentDetails
spring.datasource.username=system123 
spring.datasource.password=system123 
spring.jpa.hibernate.ddl-auto=create-drop 
5. In your controller class, you need to handle the requests.

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
package com.student;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.RestController;
@RestController
public class JdbcController {
@Autowired
JdbcTemplate jdbc;
@RequestMapping("/save")
public String index(){
jdbc.execute("insert into student (name)values(GreatLearnings)");
return "Data Entry Successful";
}
}
6. Run the application and check the entry in your Database.

15. What is response entity in spring boot?
Response Entity is basically an HTTP response which includes headers,status code and body of your response.

16. How to use logger in spring boot?
There are many logging options available in SpringBoot. Some of them are mentioned below:

Using log4j2:
1
2
3
4
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;
// [...]
Logger logger = LogManager.getLogger(LoggingController.class);
Using Lombok:
All you need to do is add a dependency called org.projectlombok in your pom.xml as shown below:

1
2
3
4
5
6
<dependency>
 <groupId>org.projectlombok</groupId>
<artifactId>lombok</artifactId>
<version>1.18.4</version>
<scope>provided</scope>
</dependency>
Now you can create a loggingController and add the @Slf4j annotation to it. Here we would not create any logger instances.

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
@RestController
@Slf4j
public class LoggingController {
 
@RequestMapping("/logging")
public String index() {
log.trace("A TRACE Message");
log.debug("A DEBUG Message");
log.info("An INFO Message");
log.warn("A WARN Message");
log.error("An ERROR Message");
  
return "Here are your logs!”;
}
}
So, there are many such ways in spring boot to use logger.

17. What is bootstrapping in spring boot?
One of the way to bootstrap your spring boot application is using Spring Initializer.
you can go to the official website of spring  and select your version, and add you groupID, artifactId and all the required dependencies. 

And then you can create your restEndpoints and build and run your project.
There you go, you have bootstrapped your spring boot application.

18. Spring boot introduced in which year?
Spring boot was introduced in the year 2002.

19. How to create jar file in spring boot?
To create a jar file in spring boot you need to define your packaging file as jar in your pom.xml(if it is maven project).

Then just do maven build with specifying goals as package so that your application will start building. 

Once the build is successful, just go into your Target folder and you can see .jar file generated for you application.

20. How to handle exceptions in spring boot?
To handle exceptions in spring boot, you can use @ControllerAdvice annotation to handle your exceptions globally.

In order to handle specific exception and send customized response, you need to use @ExceptionHandler annotation.

21. What is dependency injection in spring boot?
Dependency injection is a way through which the Spring container injects one object into another. This helps for loose coupling of components.

For example: if class student uses functionality of department class, then we say student class has dependency of Department class. Now we need to create object of class Department in your student class so that it can directly use functionalities of department class is called dependency injection.

22. How to store image in MongoDB using spring boot?
One of the way for storing image in MongoDB is by using Spring Content. And also you should have the below dependency in your pom.xml.

1
2
3
4
5
<dependency>
<groupId>com.github.paulcwarren</groupId>
<artifactId>spring-content-mongo-boot-starter</artifactId>
<version>0.0.10</version>
</dependency>
You should have a GridFsTemplate bean in your applicationContext.

1
2
3
4
5
6
7
8
@Configuration
public class Config
 
@Bean
public GridFsTemplate gridFsTemplate() throws Exception {
return new GridFsTemplate(mongoDbFactory(), mappingMongoConverter());
}
...
Now add attributes so that your content will be associated to your entity.

1
2
3
4
5
6
7
8
9
10
11
12
@ContentId
private String contentId;
 
@ContentLength 
private long contentLength = 0L;
 
@MimeType
private String mimeType = "text/plain";
And last but not the least, add a store interface.
@StoreRestResource(path="greatlearningImages")
public interface GreatLearningImageStore extends ContentStore<Candidate, String> {
}
That’s all you have to do to store your images in mongoDb using Springboot.

23. Which is the ui web framework that is built to use spring boot?
The best UI web framework that can be used with springboot is JHipster.

With this you can generate your web-applications and microservices within less time.

24. How to configure hibernate in spring boot?
The important and required dependency to configure hibernate is:

spring-boot-starter-data-jpa
h2 (you can also use any other database)
Now, provide all the database connection properties in application.properties file of your application in order to connect your JPA code with the database.

Here we will configure H2 database in application.properties file:

1
2
3
4
5
6
7
spring.datasource.url=jdbc:h2:file:~/test
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=test
spring.datasource.password=test
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
Adding the above properties in your application.properties file will help you to interact with your database using JPA repository interface.

25. Why spring boot is used for microservices?
In microservices, you can write code for your single functionality. You can use different technology stacks for different microservices as per the skill set.

You can develop this type of microservices with the help of Spring boot very quickly as spring boot gives priority to convention over configuration which increases the productivity of your developers.

26. Mention the advantages of Spring Boot.
Advantages of Spring Boot-

It allows convention over configuration hence you can fully avoid XML configuration.
SpringBoot reduces lots of development time and helps to increase productivity.
Helps to reduce a lot of boilerplate code in your application.
It comes with embedded HTTP servers like tomcat, Jetty, etc to develop and test your applications.
It also provides CLI (Command Line Interface) tool which helps you  to develop and test your application from CMD.
27. Explain Spring Actuator and its advantages.
An actuator is one of the best parts of spring boot which consists of production-ready features to help you monitor and manage your application.

With the help of Actuator, you can monitor what is happening inside the running application.
Actuator dependency figures out the metrics and makes them available as a new endpoint in your application and retrieves all required information from the web. You can identify beans, the health status of your application, CPU usage, and many more with the actuator.

28. Explain what is thyme leaf and how to use thymeleaf?
Thymeleaf is a server-side java template engine which helps processing and creating HTML, XML, JavaScript , CSS, and text. Whenever the dependency in pom.xml (in case of  maven project) is find, springboot automatically configures Thymeleaf to serve dynamic web content.

Dependency: spring-boot-starter-thymeleaf

We can place the thyme leaf templates which are just the HTML files in src/main/resources/templates/ folder so that spring boot can pick those files and renders whenever required.

Thymeleaf will parse the index.html and will replace the dynamic values with its actual value that is been passed from the controller class.
That’s it, once you run your Spring Boot application and your message will be rendered in web browsers.

29. What is the need for Spring Boot DevTools?
This is one of the amazing features provided by Spring Boot, where it restarts the spring boot application whenever any changes are being made in the code. 

 Here, you don’t need to right-click on the project and run your application again and again. Spring Boot dev tools does this for you with every code change.
Dependency to be added is: spring-boot-devtools

The main focus of this module is to improve the development time while working on Spring Boot applications.

30. Can we change the port of the embedded Tomcat server in Spring boot?
Yes, you can change the port of embedded Tomcat server in Spring boot by adding the following property in your application.properties file.

server.port=8084

31. Mention the steps to connect Spring Boot application to a database using JDBC
Below are the steps to connect your Spring Boot application to a database using JDBC:

Before that, you need to add required dependencies that are provided by spring-boot to connect your application with JDBC.

Step 1: First create a database in MySQL with create DATABASE student;

Step 2:  Now, create a table inside this DB:
CREATE TABLE student(studentid INT PRIMARY KEY NOT NULL AUTO_INCREMENT,     

studentname VARCHAR(255)); 

Step 3: Create a springBoot and add JDBC,mysql and web dependencies.
Step 4: In application.properties, you need to configure the database.

1
2
3
4
spring.datasource.url=jdbc:mysql://localhost:3306/studentDetails
spring.datasource.username=system123 
spring.datasource.password=system123 
spring.jpa.hibernate.ddl-auto=create-drop 
Step 5: In your controller class, you need to handle the requests.

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
package com.student;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.RestController;
@RestController
  public class JdbcController {
@Autowired
JdbcTemplate jdbc;
   @RequestMapping("/save")
public String index(){
jdbc.execute("insert into student 
(name)values(GreatLearnings)");
    return "Data Entry Successful";
}
}
Step 6: Run the application and check the entry in your Database.

Step 7: You can also go ahead and open the URL and you will see “Data Entry Successful” as your output.

32. What are the @RequestMapping and @RestController annotation in Spring Boot used for?
The @RequestMapping annotation can be used at class-level or method level in your controller class.

The global request path that needs to be mapped on a controller class can be done by using @RequestMapping at class-level. If you need to map a particular request specifically to some method level.

Below is a simple example to refer to:

1
2
3
4
5
6
7
8
9
10
11
12
@RestController
@RequestMapping("/greatLearning")
public class GreatLearningController {
@RequestMapping("/")
String greatLearning(){
return "Hello from greatLearning ";
}
@RequestMapping("/welcome")
String welcome(){
return "Welcome from GreatLearning";
}
}
The @RestController annotation is used at the class level.

You can use @RestController when you need to use that class as a request handler class.All the requests can be mapped and handled in this class.

@RestController itself consists @Controller and @ResponseBody which helps us to remove the need of annotating every method with @ResponseBody annotation.

Below is a simple example to refer to for use of @RestController annotation:

1
2
3
4
5
6
7
8
@RestController
@RequestMapping(“bank-details”)
public class DemoRestController{
@GetMapping(“/{id}”,produces =”application/json”)
public Bank getBankDetails(@PathVariable int id){
return findBankDetailsById();
}
}
Here, @ResponseBody is not required as the class is annotated with @RestController.

33. How can we create a custom endpoint in Spring Boot Actuator?
By using @Endpoint annotation, you can create a custom endpoint.

34. What do you understand  by auto-configuration in Spring Boot and how to disable the auto-configuration?
AutoConfiguration is a process by which Spring Boot automatically configures all the infrastructural beans. It declares the built-in beans/objects of the spring-specific module such as JPA, spring-security, and so on based on the dependencies present in your application’s classpath.
For example: If we make use of Spring JDBC, the spring boot autoconfiguration feature automatically registers the DataSource and JDBCTemplete bean.
This entire process of automatically declaring the framework-specific bean without the need of writing the XML code or java-config code explicitly  is called Autoconfiguration which is done by spring-boot with the help of an annotation called @EnableAutoconfiguration alternatively @SpringBootApplication.

1. You can exclude the attribute of @EnableAutoConfiguration where you don’t want it to be configured implicity in order to disable the spring boot’s auto-configuration feature.

2. Another way of disabling auto-configuration is by using the property file:

For example: 

1
2
3
spring.autoconfigure.exclude= 
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,
org.springframework.boot.autoconfigure.data.MongoDataConfiguration,
In the above example, we have disabled the autoconfiguration of MongoDB.

35. Can you give an example for ReadOnly as true in Transaction management?
Yes, example for ReadOnly as true in Transaction Management is:

Suppose you have a scenario where you have to read data from your database like if you have a STUDENT database and you have to read the student details such as studentID, and studentName.

 So in such scenarios, you will have to set read-only on the transaction.

36. Mention the advantages of the YAML file than Properties file and the different ways to load  
YAML file in Spring boot.

YAML gives you more clarity and is very friendly to humans. It also supports maps, lists, and other scalar types.

YAML comes with hierarchical nature which helps in avoiding repetition as well as indentations.

If we have different deployment profiles such as  development, testing, or production and we may have different configurations for each environment, so instead of creating new files for each environment we can place them in a single YAML file.
But in the case of the properties file, you cannot do that.

For example: 

1
2
3
4
5
6
7
8
9
10
11
12
13
14
spring:
profiles:
active:
-test
---
spring:
profiles:
active:
-prod
---
spring:
profiles:
active:
-development
37. What do you understand by Spring Data REST?
By using Spring Data Rest, you have access to all the RESTful resources that revolves around Spring Data repositories.

Refer the below example:

1
2
3
@RepositoryRestResource(collectionResourceRel = "greatlearning", path = "sample")
public interface GreatLearningRepo extends CustomerRepository< greatlearning, Long> {
}
Now you can use the POST method in the below manner:

1
2
3
{
“Name”:”GreatLearning”
}
And you will get response as follow:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
{
“Name”:”GreatLearning”
}
__________
{
"name": "Hello greatlearning "
"_links": {
"self": {
"href": "<a href="http://localhost:8080/sample/1">http://localhost:8080/ greatlearning /1</a>"
},
" greatlearning ": {
“href": "<a href="http://localhost:8080/sample/1">http://localhost:8080/ greatlearning /1</a>"
}
}
In the above, you can see the response of the newly created resource.

38. How do you Configure Log4j for logging?
Spring Boot supports log4j2 for logging configuration. what all you have to do is you have to exclude log backfile and include log4j2 for logging. 

You can do it by using the spring starter projects.

39. What do you think is the need for Profiles?
The application has different stages-such as the development stage, testing stage, production stage and may have different configurations based on the environments.

With the help of spring boot, you can place profile-specific properties in different files such as

application-{profile}.properties

In the above, you can replace the profile with whatever environment you need, for example, if it is a development profile, then application-development.properties file will have development specific configurations in it.

So, in order to have profile-specific configurations/properties, you need to specify an active profile.

40. What are the steps to add a custom JS code with Spring Boot?
All you have to do is just create a static folder under src/main/resources and place all your static content in this folder.

And your custom JS code will be added with spring boot.

41. What is the name of the default H2 database configured by Spring Boot?
H2 database is an in-memory database configured by SpringBoot.
The default name of the H2 database that is configured by spring boot is testdb.

42. How to call servlet in spring boot?
Servlet can be configured by using implementing or extending WebAppInitializer interface, AbstractDispatcherServletInitializer abstract class, and AbstractAnnotationConfigDispatcherServletInitializer abstract class

43. what is the HTTP methods that can be implemented in spring boot rest service?
Spring boot rest service helps in CRUD (create,retrieve,update,delete)operations. So based on that most commonly used HTTP methods are GET, POST, PUT, DELETE, and PATCH are the methods that can be implemented in spring boot rest services.

44. How to debug spring boot application in eclipse?
Debugging your spring boot application is as simple as same as debugging your Java application.
All you have to do is place debug point within your application, and right-click-> click on debug as java application or spring boot application.

45. How to insert data in mysql using spring boot?
First configure mysql in your spring boot application.

Then you can map your entities with your db tables using JPA.

And with the help of save() method in JPA, you can directly insert your data into your database.

1
2
3
4
5
6
7
8
@RestController
@RequestMapping("/greatleasrning")
public class Controller {
@Autowired
private final GreatLearningRepository greatLearningRepository;
public Controller(GreatLearningRepository greatLearningRepository) {
this. greatLearningRepository = greatLearningRepository;
}
In the above case, your data which may be in JSON format can be inserted successfully into the database.

1
2
3
4
5
6
@RequestMapping(method = RequestMethod.POST)
ResponseEntity<?> insert(@RequestBody Course course) {
greatLearningRepository.save(course);
return ResponseEntity.accepted().build();
}
}
46. How to create a login page in spring boot?
You can create a simple and default login page in spring boot, you can make use of Spring security. Spring security secures all HTTP endpoints where the user has to login into the default HTTP form provided by spring.

We need to add spring-boot-starter-security dependency in your pom.xml or build.gradle and a default username and password can be generated with which you can log in.

47. What is the main class in spring boot?
Usually in java applications, a class that has a main method in it is considered as a main class. Similarly, in spring boot applications main class is the class which has a public static void main() method and which starts up the SpringApplicationContext.

48. How to use crud repository in spring boot?
In order to use crud repository in spring boot, all you have to do is extend the crud repository which in turn extends the Repository interface as a result you will not need to implement your own methods.

Create a simple spring boot application which includes below dependency:
spring-boot-starter-data-jpa, spring-boot-starter-data-rest

And extend your repository interface as shown below:

1
2
3
4
5
6
7
8
9
10
11
12
package com.greatlearning;
import java.util.List;
import org.springframework.data.repository.CrudRepository;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;
@RepositoryRestResource
public interface GreatLearning extends CrudRepository<Candidate, Long> 
{
public List<Candidate> findById(long id);
 
//@Query("select s from Candidate s where s.age <= ?")
public List<Candidate> findByAgeLessThanEqual (long age);
}
49. How to run spring-boot jar from the command line?
In order to run spring boot jar from the command line, you need to update you pom.xml(or build.gradle) of your project with the maven plugin.

1
2
3
4
5
6
7
8
<build>
<plugins>
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
</plugins>
</build>
Now, Build your application and package it into the single executable jar. Once the jar is built you can run it through the command prompt  using the below query:

java -jar target/myDemoService-0.0.1-SNAPSHOT.jar

And you have your application running.

50. What is Spring Boot CLI and how to execute the Spring Boot project using boot CLI?
Spring Boot CLI is nothing but a command-line tool which is provided by Spring so that you can develop your applications quicker and faster.

To execute your spring boot project using CLI, you need first to download CLI from Spring’s official website and extract those files. You may see a bin folder present in the Spring setup which is used to execute your spring boot application.

As Spring boot CLI allows you to execute groovy files, you can create one and open it in the terminal.
And then execute  ./spring run filename.groovy;

51. How to ignore null values in JSON response spring boot?
In order to ignore null values in JSON response, you can use @JsonIgnore annotation.
The field can be ignored while reading JSON into Java objects as well as while writing Java objects into JSON.

52. what is the rest controller in spring boot?
The @RestController annotation is used at the class level.

You can use @RestController when you need to use that class as a request handler class.All the requests can be mapped and handled in this class.

@RestController itself consists @Controller and @ResponseBody which helps us to remove the need of annotating every method with @ResponseBody annotation.

Below is a simple example to refer to for use of @RestController annotation:

1
2
3
4
5
6
7
8
@RestController
@RequestMapping(“bank-details”)
public class DemoRestController{
@GetMapping(“/{id}”,produces =”application/json”)
public Bank getBankDetails(@PathVariable int id){
return findBankDetailsById();
}
}
Here, @ResponseBody is not required as the class is annotated with @RestController.

53. How to handle 404 error in spring boot?
Consider a scenario, where there are no stockDetails in the DB and still, whenever you hit the GET method you get 200(OK) even though the resource is not found which is not expected. Instead of 200, you should get 404 error.
So to handle this, you need to create an exception, in the above scenario “StockNotFoundException”.

1
2
3
4
5
6
7
8
9
GetMapping("/stocks/{number}")  
public Stock retriveStock(@PathVariable int number)  
{  
Stock  stock  = service.findOne(number);  
if(Stock  ==null)  
//runtime exception  
throw new StockNotFoundException("number: "+ number);  
return stock;  
}  
Now, create a Constructor from Superclass.

Right-click on the file -> Go to Source ->And generate constuctors from superclass-> and check the RuntimeException(String)-> and generate.

And add an annotation called @ResponseStatus which will give you 404(not found) error.

1
2
3
4
5
6
7
8
9
10
11
12
package com.greatlearning;  
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ResponseStatus;  
  
@ResponseStatus(HttpStatus.NOT_FOUND)
public class StockNotFoundException extends RuntimeException   
{  
public StockNotFoundException(String message)   
{  
super(message);  
}  
}  
Now, you can hit the same URL again and there you go, you get a 404 error when a resource is not found.

54. How to do pagination in spring boot?
The process of dividing your data into small and suitable chunks is Pagination.

One can achieve pagination by using PagingAndSortingRepository which is an extension of crudRepository.

55. What are spring boot annotations?
Spring Boot annotations are nothing but the supplemental data/information about the program. It is not directly part of the program and hence does not affect the compilation or execution. In other words, it is kind of the metadata that supplies us with the data of the program. Some important Spring Boot Annotations are:

Spring Framework Stereotype Annotations
Core Spring Framework Annotations
56. Which is the spring boot latest version?
The latest version of spring boot is 2.6.0. It came out with a lot of dependency upgrades, java 15 support and much more.

Yes, now as you are brushed up with spring boot interview questions and answers. We have also tried to cover all the springboot interview questions for experienced professionals.

Hope you can easily crack the spring boot interview now!

Please feel free to comment below if you have any queries related to the above questions or answers.
Also, do comment if you find any other questions that you think must be included in the above list of questions.

For a more detailed course experience, visit Great Learning Academy, where you will find a bunch of free courses on AIML and Data Science.

Spring Boot Interview Questions for Experienced Professionals
57. How to check the environment properties in your Spring boot application?
If we need to set the different target environments, Spring Boot has a built-in mechanism.

One can simply define an application environment.properties file in the src/main/resources directory and then set a Spring profile with the same environment name.

For example, if we define a “production” environment, that means we’ll have to define a production profile and then application-production.properties.

This environment file will be loaded and will take precedence over the default property file. You should note that the default file will still be loaded. It’s just that when there is a property collision, the environment-specific property file takes precedence.

58. How to enable debugging log in the spring boot application?
The user can enable a “debug” mode in the spring boot application by starting your application with a –debug flag. The user can also specify debug=true in the application.properties 

When the debug mode is enabled, a user can configure a selection of core loggers (embedded container, Hibernate, and Spring Boot) to output more information.

59. Where do we define properties in the Spring Boot application?
Command Line Properties

Command-line properties are converted into Spring Boot Environment properties by the spring boot application. 

Command-line properties have more precedence over the other property sources. 

Spring Boot uses the 8080 port number, by default, to start the Tomcat. Let us see how one can change the port number by using command-line properties.


Properties File

Properties files are used to keep one or more properties in a single file to run the application in a different environment. Properties are kept in the application.properties file under the classpath in a typical spring boot application. The location of the application.properties file is at src/main/resources directory. The code of application.properties file is as below:


YAML File

Spring Boot also supports YAML-based properties configurations to run the application. The user can use,  application.yml file instead of the application.properties file. The YAML file is kept inside the classpath. The sample application.yml file is given below −


Externalized Properties

The user can keep properties in different locations or paths instead of keeping the properties file under classpath. While running the JAR file, the user can specify the properties file path. The application developer can use the following command to specify the location of the properties file while running the JAR −


60. What is dependency Injection?
Classes often require references to other classes. For example, a Train class might need a reference to an Engine class. These required classes are called dependencies, and in this example, the Train class is dependent on having an instance of the Engine class to run.

Dependency injection is a programming technique that makes a class independent of its dependencies. This is achieved by decoupling the usage of an object from its creation. Dependency injection allows the creation of dependent objects outside of a class, and it provides those objects to a class in different ways. The user can move the creation and binding of the dependent objects outside of the class that depends on them, using dependency injection.

61. What is an IOC container?
IoC Container is a framework that is used for implementing automatic dependency injection. It manages object creation and its lifetime. It, it also injects dependencies into the class.

The IoC container is used to create an object of the specified class. It also injects all the dependency objects through a constructor, a property, or a method at run time and disposes it at the appropriate time. With this, one doesn’t have to create and manage objects manually.

All the containers provide easy support for the Dependency Injection lifecycle as below.

Register: The container should know which dependency to instantiate when it encounters a particular type. This process is called registration. 

Resolve: When using the IoC container, the objects need to be created manually. This is done by the container and is called resolution. The container should include some methods to resolve the specified type; the container creates an object of the specified type. It then injects the required dependencies if any and returns the object.

Dispose: The container should manage the lifetime of the dependent objects. IoC containers include different lifetime managers to manage an object’s lifecycle and dispose it.

62. What are the basic Annotations that spring boot offers?
First of all, we have to know about the annotations. Annotations are used to instruct the intention of the programmers.

As the name suggests, spring boot annotations is a form of Metadata that provides the whole data about the program. In other ways, we can define it as annotations are used to provide supplemental information about the program. It is not part of the program.

It does not change the programs which are already compiled.

Core Spring Framework Annotation:-
@Required:-
@Required applies to the bean setter method.

This indicates that the annotated bean must be populated at the configuration time with the required property; if the following case is not satisfied, it throws an exception BeanInitializationException.

@Autowired:-
 In the spring framework, spring provides annotation-based auto–wiring by providing @Autowired annotation.

It is used to auto-wire spring bean on setter methods, instance variables and constructors., When we use the annotation @Autowired, the spring container auto-wires the bean factory by matching the data type.

 Other Annotations which are provided by Spring Boot, Spring Framework, and In Spring MVC are:-

@configuartion.
@Componentscan
@Bean
@component.
@Controller.
@service.
@Repository
@EnableAutoConfiguaration
@SpringBootApplication.
@RequestMapping
@GetMapping
@PostMapping.
63. What is spring Boot dependency Management?
Spring Boot manages dependencies and configuration automatically. Each release of spring boot provides a list of dependencies that it supports. The list of dependencies available as a part of Spring-boot dependencies can be used in maven, so we need to specify the version of the dependencies or add the dependencies version in our config file in our configuration.

Spring boot automatically manages and spring boot upgrades all dependencies automatically respectively or consistently at the time when we update the spring boot version.

Advantage of Dependency Management:-
Spring dependency management provides us the centralized dependency information by adding or specifying the dependencies version in a required place in the spring boot version. It also helps us to switch from one version to another version with ease.
This management helps us to avoid the mismatch of different versions of the Spring Boot library.
Here we simply have to write a library name specifying the version.
64. Can we create a non-web application in spring boot?
Yes, but the application could also be called as spring boot standalone application.

To create a non-web application, your application needs to implement CommandLineRunner interface and its Run method for the running of our application. So this run method always acts like the main of our non-web application.

65. Is it possible to change the port of embedded Tomcat server in spring boot?
Yes it is possible to change the port number of embedded tomcat server in Spring boot.

The default port number of the tomcat server to run the spring boot application is 8080, which is further possible to change it.

So we can change the port of tomcat following ways given below:-

Using application.properties
Using application.yml
Using EmbeddedServletContainerCustomizer interface.
Using WebServerFactoryCustomizer interface.
Using Command-Line Parameter.
66. What is the default port of the tomcat server in Spring Boot?
As we had already discussed about the default port, the tomcat server in spring boot is port 8080. Which is changeable based on the user or the programmer’s requirement.

67. Can we override or replace the embedded tomcat server in spring boot?
If we consider the fact, spring boot by default comes up with the embedded server once we add the “Spring –boot-starter” dependency. But the spring boot gives us the flexibility to use the tomcat.

If we don’t want to use the tomcat, then tomcat comes with three types of embed servers: Tomcat, jetty, and undertow.

68. Can we disable the default web server in the spring boot application?
Yes, as discussed above, there are 3 web servers available we can choose between them. Spring boot gives more priority for using the tomcat server. 

69. How can we disable the auto-configuration class?
In case if we find some auto-configure classes are being applied in an application that you don’t want, then we have to use the annotation @EnableAutoConfioguration to disable them.

70. Explain @Restcontroller annotation in spring boot?
Spring restcontroller annotation is an annotation that is itself annotated within two annotations.

@Restcontroller is annotated within @controller and @Responsebody. This annotation is applied to mark the respective class as a request handler in your application.

Spring Rest controller annotation is used to create restful web services using Spring MVC. 

71. What is the difference between @RestController and @Controller in Spring Boot?
@controller	@RestController
Controller is a common annotation that is used to mark a class as a spring MVC controller.	Rest controller is a Springspecial controller used in Restful web services and the wrapped within the @controller and @Responsebody
72. Describe the flow of HTTPS request through the spring boot app?
![Springboot_Flow](/Screenshots/Springboot_Flow.png)

We all can see the above image of the spring boot flow architecture to understand the basic concept of the HTTPS request flow in the spring boot app.

We have the validator classes, view classes, and utility classes.

As we all know, spring boot uses the modules of spring-like MVC, spring data, etc.

So the concept also the same for several things, and also the architecture of spring boot is the same as the architecture of spring MVC; instead of one concept, there is no need for the DAO and DAOimpl classes in spring boot. 

It creates a data access layer and started performing CRUD operations.

CRUD operation is nothing but Create Read Update and Delete operation, which is done by all of the programmers in their website.

The client makes the HTTP request in PUT or GET.

After this, the request goes to the controller, and the controller maps that respective request and handles it; if there is the requirement for calling some logic, it calls the service logic after handling the request.

All the business logic performs in the service layer.

Service layer performs the logic on the data that is mapped to JPA with model classes.

A JSP page is returned to the user if no error has occurred.

73. What is the difference between RequestMapping and GetMapping?
The @GetMapping is a composed annotation which is the short notation of @RequestMapping(method=RequestMethod.GET).

These both methods support the “Consumes.”

The consumes options are,

Consumes=”text/plain”

Consumes={“text/plain”,”application”};  .

74. What is the use of profiles in spring boot?
A profile is nothing but a set of configuration settings. Spring boot allows us to define the profile regarding property files in the form of application-{profile}.properties.

Profile automatically loads the properties in an application.

75. What is Spring Actuator? What are its advantages?
Spring boot actuator brings product-ready features to our application. Some of the features are like monitoring our app, gathering metrics, understanding traffic, or the states of our database become trivial with this dependency.

The main advantage of having this library is that we can production-level tools even if we don’t have the actual implementation of these features by ourselves.

Spring boot Actuator mainly used to expose the operational information about the running application.

Ex:- Running applications like health, metrics, info, dump, env, etc.

76. How to enable Actuator in spring boot application?
For enabling the spring boot actuator endpoints to your spring boot application, we have to add the spring boot starter actuator dependency in our application configuration file.

In the maven users’ case, they can also add the spring boot starter actuator dependency in their applications Pom.xml file. Like the Gradle users, they can add the dependency in their respective build.

77. What are the actuator-provided endpoints used for monitoring the spring boot application?
We can use the HTTP and JMX endpoints to manage and monitor the spring boot application.

78. How to get the list of all the beans in your spring boot application?
 In the case of spring boot, you can use appContext.getBeanDefinitionNames() to get all the beans loaded by the spring container.

By calling this method, we can show all of our beans present in our spring boot applications.

Spring Boot Microservices Interview Questions
79. What is Spring Cloud?
Spring Cloud is an open-source library that provides tools for quickly deploying the JVM based application on the clouds. It provides a better user experience and an extensible mechanism due to various features like Distributed configuration, Circuit breakers, Global locks, Service registrations, Load balancing, Cluster state, Routing, Load Balancing, etc. It is capable of working with spring and different applications in various languages

Features of Spring Cloud

Major features are as below:

Distributed configuration
Distributed messaging
service-to-service calls
Circuit breakers
Global locks
Service registration
Service Discovery
Load balancing
Cluster state
Routing
80. How Do You Override A Spring Boot Project’s Default Properties?
Spring Application loads properties from the application.properties files in the following locations and add them to the Spring Environment:

A /config subdirectory of the current directory.
The current directory
A classpath /config package
The classpath root
The list is ordered by precedence means that the properties that are defined in locations higher in the list override those defined in lower locations.

If the user does not want application.properties as the configuration file name, they can switch to another by specifying a spring.config.name environment property. The user can also refer to an explicit location using the spring.config.location environment property (comma-separated list of directory locations, or file paths).
![Default Properties](/Screenshots/image9.png)

81. What is the Role Of Actuator In Spring Boot?
The Spring boot Actuator is used to expose operational information about the running application, like health, metrics, info, dump, env, etc. It uses HTTP endpoints or JMX beans to enable the user to interact with it. Once this dependency is on the classpath, several endpoints are available for the user out of the box

82. How Is Spring Security Implemented In A Spring Boot Application?
Spring Security is a framework that majorly focuses on providing both authentication and authorization to Java EE-based enterprise software applications.

Adding Spring security:

Maven:

To include spring security, include the below dependency:
![Maven_Dependency](/Screenshots/Maven_Dependency.png)

Gradle:

To include spring security in Gradle based project use:
![Gradle_Dependency](/Screenshots/Gradle_Dependency.png)

83. Which Embedded Containers Are Supported By Spring Boot?
The embedded containers supported by spring boot are Tomcat (default), Jetty, and undertow servers

84. Where Do We Use WebMVC Test Annotation?
![WebMVC Test](/Screenshots/image14.png)

Annotation can be used for a Spring MVC test that focuses only on Spring MVC components.

Using this annotation disables full auto-configuration and instead apply only configuration relevant to MVC tests (i.e., @Controller, @ControllerAdvice, @JsonComponent, Converter/GenericConverter, Filter, WebMvcConfigurer, and HandlerMethodArgumentResolver beans but not @Component, @Service, or @Repository beans).

By default, annotated tests with @WebMvcTest will also auto-configure Spring Security and MockMvc (including support for HtmlUnit WebClient and Selenium WebDriver). For more fine-grained control of MockMVC, the @AutoConfigureMockMvc annotation is used.

Usually @WebMvcTest is used in combination with @MockBean or @Import to create any collaborators required by your @Controller beans.

85. How to Configure Spring Boot Application Logging?
Spring Boot provides a LoggingSystem abstraction that configures logging based on the content of the classpath. If Logback is available, it is definitely the first choice.

Suppose the only change the user needs to make to logging is to set the levels of various loggers. In that case, they can do so in application.properties by using the “logging.level” prefix, as shown in the following example:
```
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate=ERROR
```

Java Spring boot interview questions
86. What is the Minimum Java version needed for Spring Boot?
Java 8 is the minimum version required.

87. What is Thymeleaf?
Thymeleaf is basically a server-side Java template engine for both web and standalone environments.

The main goal of Thymeleaf is to bring the natural templates to your development workflow — HTML that can be correctly displayed in browsers and also work as static prototypes, allowing for stronger collaboration in development teams.

88. How to use thymeleaf?
Steps are as follows:

First, create a Spring Boot Project using STS or Spring Initializer. Add dependency for Thymeleaf and Spring Web.
![Thymeleaf](/Screenshots/image15.png)
Create a Controller Class in package by either adding a new package or use the default package containing the main application class.
![Thymeleaf](/Screenshots/image17.png)
Add template in the resources folder. src/main/resources/templates/thymeleafTemplate.html
![Thymeleaf](/Screenshots/image11.png)
Build code.
Run the application using Integrated Development Environment: Run as -> Spring Boot App.
![Thymeleaf](/Screenshots/image12.png)


_Resource:_
https://www.mygreatlearning.com/blog/spring-boot-interview-questions/

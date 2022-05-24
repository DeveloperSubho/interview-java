**1. Overview**
Exceptions are undesired behavior of a software application caused by faulty logic. In this article, we're going to be looking at how to handle exceptions in a Spring Boot application.

What we want to achieve is that whenever there's an error in our application, we want to gracefully handle it and return the following response format:

Listing 1.1 error response format 
```
JSON
{
  "code": 400,
  "message": "Missing required fields",
  "errors": [
    "additional specific error"
    "username is required",
    "email is required"
  ],
  "status": false
}
```

The code is a standard HTTP status code and the status attribute is a simple way to know if the request is successful or not. The response message is a summary of the failures while the errors array contains more specific and detailed error messages.

**2. Basic Exception Handling**
We will create a class GlobalExceptionHandler that will implement the ErrorController interface and define a controller action for the /error endpoint. We will annotate the class with @RestController for Spring Boot to recognize our error endpoint.

Listing 2.1 GlobalExceptionHandler.java
```
Java
@RestController
public class GlobalExceptionHandler implements ErrorController {

    public GlobalExceptionHandler() {
    }

    @RequestMapping("/error")

    public ResponseEntity<Map<String, Object>> handleError(HttpServletRequest request) {
        HttpStatus httpStatus = getHttpStatus(request);
        String message = getErrorMessage(request, httpStatus);
        Map<String, Object> response = new HashMap<>();
        response.put("status", false);
        response.put("code", httpStatus.value());
        response.put("message", message);
        response.put("errors", Collections.singletonList(message));

        return ResponseEntity.status(httpStatus).body(response);
    }

    private HttpStatus getHttpStatus(HttpServletRequest request) {

        //get the standard error code set by Spring Context
        Integer status = (Integer) request.getAttribute(RequestDispatcher.ERROR_STATUS_CODE);
        if (status != null) {
            return HttpStatus.valueOf(status);
        }

        // maybe we're the one that trigger the redirect
        // with the code param
        String code = request.getParameter("code");
        if (code != null && !code.isBlank()) {
            return HttpStatus.valueOf(code);
        }

        //default fallback
        return HttpStatus.INTERNAL_SERVER_ERROR;
    }
    private String getErrorMessage(HttpServletRequest request, HttpStatus httpStatus) {
​
        //get the error message set by Spring context
        // and return it if it's not null
        String message = (String) request.getAttribute(RequestDispatcher.ERROR_MESSAGE);
        if (message != null && !message.isEmpty()) {
            return message;
        }

        //if the default message is null,
        //let's construct a message based on the HTTP status
        switch (httpStatus) {
            case NOT_FOUND:
                message = "The resource does not exist";
                break;
            case INTERNAL_SERVER_ERROR:
                message = "Something went wrong internally";
                break;
            case FORBIDDEN:
                message = "Permission denied";
                break;
            case TOO_MANY_REQUESTS:
                message = "Too many requests";
                break;
            default:
                message = httpStatus.getReasonPhrase();
        }

        return message;
    }

}
```

We created two helper functions — getHttpStatus and getErrorMessage. The first one will extract the HTTP status from the Servlet request while the second function will extrapolate the error message from either the Servlet request or the HTTP status.

The handleError function will be called whenever there's a runtime error in the application. The function will use the two helper methods to get the code and message to return as part of the final response.

Let's run the application and use curl to test our setup. We will simply visit an endpoint that does not exist:
```
Java
curl --location --request GET 'http://localhost:8080/not/found'
​
{
  "code": 404,
  "message": "The resource does not exist",
  "errors": [
    "The resource does not exist"
  ],
  "status": false
}
```

Our application is now returning our custom response. Let's add a new controller action that will raise a RuntimeException with a custom message and see what the response will be when we call it.

Listing 2.2 IndexController.java
```
Java
@RestController
public class IndexController {
​
    @GetMapping("/ex/runtime")
    public ResponseEntity<Map<String, Object>> runtimeException() {
        throw new RuntimeException("RuntimeException raised");
    }
​
}
```
```
Java
curl --location --request GET 'http://localhost:8080/ex/runtime' -v
​
*   Trying ::1:8080...
* Connected to localhost (::1) port 8080 (#0)
> GET /ex/runtime HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.77.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 500 
< Content-Type: application/json
< Transfer-Encoding: chunked
< Date: Tue, 16 Nov 2021 07:10:10 GMT
< Connection: close
< 
* Closing connection 0

{
  "code": 500,
  "message": "Something went wrong internally",
  "errors": [
    "Something went wrong internally"
  ],
  "status": false
}
```

This time around, we appended the -v flag to the curl command and we can see from the verbose response that the HTTP code returned is indeed 500 — the same as the value of code in the returned response body.

**3. Handling Specific Exception Class**
Even though what we have is capable of handling all exceptions, we can still have specific handlers for specific exception classes.

At times we want to handle certain exception classes because we want to respond differently and/or execute custom logic. 

To achieve this, we will annotate the GlobalExceptionHandler class with @RestControllerAdvice and define exception handler methods for each exception class we want to handle.

For the purpose of this article, we will handle the HttpRequestMethodNotSupportedException class and return a custom message.

Listing 3.1 GlobalExceptionHandler.java
```
Java
@ExceptionHandler(HttpRequestMethodNotSupportedException.class)
public ResponseEntity<Map<String, Object>> handleError(HttpRequestMethodNotSupportedException e) {
    String message = e.getMessage();
    Map<String, Object> response = new HashMap<>();
    response.put("status", false);
    response.put("code", HttpStatus.METHOD_NOT_ALLOWED);
    response.put("message", "It seems you're using the wrong HTTP method");
    response.put("errors", Collections.singletonList(message));
    return ResponseEntity.status(HttpStatus.METHOD_NOT_ALLOWED).body(response);
}
```

Now, if we call the /ex/runtime endpoint with a POST method, we should get the unique message that we set and the errors array will contain the raw exception message:
```
Java
curl --location --request POST 'http://localhost:8080/ex/runtime'

{
  "code": 405,
  "message": "It seems you're using the wrong HTTP method",
  "errors": [
    "Request method 'POST' not supported"
  ],
  "status": false
}
```

We can repeat this for as many as possible exception classes that we want to handle specifically. Note that declaring a specific handler means the /error endpoint will not be invoked for that particular exception

**4. Handling a Custom Exception Class**
Simply put, we will create a subclass of the RuntimeException class and create a specific handler for it in the GlobalExceptionHandler. Whenever we want to return an error response to our API client, we will just raise a new instance of our custom exception class.

The sweet part is that we can throw the exception from a controller, a service, or just about any other component and it will be handled correctly.

First, let's create the custom exception class.

Listing 4.1 CustomApplicationException.java
```
Java
public class CustomApplicationException extends RuntimeException {
​
    private HttpStatus httpStatus;
    private List<String> errors;
    private Object data;

    public CustomApplicationException(String message) {
        this(HttpStatus.BAD_REQUEST, message);
    }

    public CustomApplicationException(String message, Throwable throwable) {
        super(message, throwable);
    }

    public CustomApplicationException(HttpStatus httpStatus, String message) {
        this(httpStatus, message, Collections.singletonList(message), null);
    }

    public CustomApplicationException(HttpStatus httpStatus, String message, Object data) {
        this(httpStatus, message, Collections.singletonList(message), data);
    }

    public CustomApplicationException(HttpStatus httpStatus, String message, List<String> errors) {
        this(httpStatus, message, errors, null);
    }

    public CustomApplicationException(HttpStatus httpStatus, String message, List<String> errors, Object data) {
        super(message);
        this.httpStatus = httpStatus;
        this.errors = errors;
        this.data = data;
    }

    public HttpStatus getHttpStatus() {
        return httpStatus;
    }

    public List<String> getErrors() {
        return errors;
    }

    public Object getData() {
        return data;
    }
}
```

We defined a number of useful fields for the CustomApplicationException class alongside convenient constructors. This means we can specify the HTTP status, message, and list of errors when we're raising the exception.

Now we will define a handler for it and create a controller endpoint to test it out.

Listing 4.2 GlobalExceptionHandler.java 
```
Java
@ExceptionHandler(CustomApplicationException.class)
public ResponseEntity<Map<String, Object>> handleError(CustomApplicationException e) {
    Map<String, Object> response = new HashMap<>();
    response.put("status", false);
    response.put("code", e.getHttpStatus().value());
    response.put("message", e.getMessage());
    response.put("errors", e.getErrors());
    return ResponseEntity.status(e.getHttpStatus()).body(response);
}
```

Listing 4.3 IndexController.java
```
Java
@PostMapping("/ex/custom")
public ResponseEntity<Map<String, Object>> customException(@RequestBody Map<String, Object> request) {
    List<String> errors = new ArrayList<>();
    if(!request.containsKey("username"))
        errors.add("Username is required");
    if(!request.containsKey("password"))
        errors.add("Password is required");

    if(!errors.isEmpty()) {
        String errorMessage = "Missing required parameters";
        throw new CustomApplicationException(HttpStatus.BAD_REQUEST, errorMessage , errors);
    }

    return ResponseEntity.ok(Collections.singletonMap("status", true));

}
```
​```
Java
curl --location --request POST 'http://localhost:8080/ex/custom' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "john"
}'
```
```
{
    "code": 400,
    "message": "Missing required parameters",
    "errors": [
        "Password is required"
    ],
    "status": false
}
```

The returned message is a general description of what went wrong while the errors contain the exact field that's missing — just as we wanted.

-----------------------------------------------------------------------------------------------------------------

Handling exceptions and errors in APIs and sending the proper response to the client is good for enterprise applications. In this chapter, we will learn how to handle exceptions in Spring Boot.

Before proceeding with exception handling, let us gain an understanding on the following annotations.

Controller Advice
The @ControllerAdvice is an annotation, to handle the exceptions globally.

Exception Handler
The @ExceptionHandler is an annotation used to handle the specific exceptions and sending the custom responses to the client.

You can use the following code to create @ControllerAdvice class to handle the exceptions globally −

package com.tutorialspoint.demo.exception;

import org.springframework.web.bind.annotation.ControllerAdvice;

@ControllerAdvice
   public class ProductExceptionController {
}
Define a class that extends the RuntimeException class.

package com.tutorialspoint.demo.exception;

public class ProductNotfoundException extends RuntimeException {
   private static final long serialVersionUID = 1L;
}
You can define the @ExceptionHandler method to handle the exceptions as shown. This method should be used for writing the Controller Advice class file.

@ExceptionHandler(value = ProductNotfoundException.class)

public ResponseEntity<Object> exception(ProductNotfoundException exception) {
}
Now, use the code given below to throw the exception from the API.

@RequestMapping(value = "/products/{id}", method = RequestMethod.PUT)
public ResponseEntity<Object> updateProduct() { 
   throw new ProductNotfoundException();
}
The complete code to handle the exception is given below. In this example, we used the PUT API to update the product. Here, while updating the product, if the product is not found, then return the response error message as “Product not found”. Note that the ProductNotFoundException exception class should extend the RuntimeException.

package com.tutorialspoint.demo.exception;
public class ProductNotfoundException extends RuntimeException {
   private static final long serialVersionUID = 1L;
}
The Controller Advice class to handle the exception globally is given below. We can define any Exception Handler methods in this class file.

package com.tutorialspoint.demo.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class ProductExceptionController {
   @ExceptionHandler(value = ProductNotfoundException.class)
   public ResponseEntity<Object> exception(ProductNotfoundException exception) {
      return new ResponseEntity<>("Product not found", HttpStatus.NOT_FOUND);
   }
}
The Product Service API controller file is given below to update the Product. If the Product is not found, then it throws the ProductNotFoundException class.

package com.tutorialspoint.demo.controller;

import java.util.HashMap;
import java.util.Map;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.tutorialspoint.demo.exception.ProductNotfoundException;
import com.tutorialspoint.demo.model.Product;

@RestController
public class ProductServiceController {
   private static Map<String, Product> productRepo = new HashMap<>();
   static {
      Product honey = new Product();
      honey.setId("1");
      honey.setName("Honey");
      productRepo.put(honey.getId(), honey);
      
      Product almond = new Product();
      almond.setId("2");
      almond.setName("Almond");
      productRepo.put(almond.getId(), almond);
   }
   
   @RequestMapping(value = "/products/{id}", method = RequestMethod.PUT)
   public ResponseEntity<Object> updateProduct(@PathVariable("id") String id, @RequestBody Product product) { 
      if(!productRepo.containsKey(id))throw new ProductNotfoundException();
      productRepo.remove(id);
      product.setId(id);
      productRepo.put(id, product);
      return new ResponseEntity<>("Product is updated successfully", HttpStatus.OK);
   }
}
The code for main Spring Boot application class file is given below −

package com.tutorialspoint.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
   public static void main(String[] args) {
      SpringApplication.run(DemoApplication.class, args);
   }
}
The code for POJO class for Product is given below −

package com.tutorialspoint.demo.model;
public class Product {
   private String id;
   private String name;

   public String getId() {
      return id;
   }
   public void setId(String id) {
      this.id = id;
   }
   public String getName() {
      return name;
   }
   public void setName(String name) {
      this.name = name;
   }
}
The code for Maven build – pom.xml is shown below −

<?xml version = "1.0" encoding = "UTF-8"?>
<project xmlns = "http://maven.apache.org/POM/4.0.0" 
   xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0 
   http://maven.apache.org/xsd/maven-4.0.0.xsd">
   
   <modelVersion>4.0.0</modelVersion>
   <groupId>com.tutorialspoint</groupId>
   <artifactId>demo</artifactId>
   <version>0.0.1-SNAPSHOT</version>
   <packaging>jar</packaging>
   <name>demo</name>
   <description>Demo project for Spring Boot</description>

   <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>1.5.8.RELEASE</version>
      <relativePath/> 
   </parent>

   <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
      <java.version>1.8</java.version>
   </properties>

   <dependencies>
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
      <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-test</artifactId>
         <scope>test</scope>
      </dependency>
   </dependencies>

   <build>
      <plugins>
         <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
         </plugin>
      </plugins>
   </build>
   
</project>
The code for Gradle Build – build.gradle is given below −

buildscript {
   ext {
      springBootVersion = '1.5.8.RELEASE'
   }
   repositories {
      mavenCentral()
   }
   dependencies {
      classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
   }
}
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'

group = 'com.tutorialspoint'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = 1.8

repositories {
   mavenCentral()
}
dependencies {
   compile('org.springframework.boot:spring-boot-starter-web')
   testCompile('org.springframework.boot:spring-boot-starter-test')
}
You can create an executable JAR file, and run the Spring Boot application by using the Maven or Gradle commands −

For Maven, you can use the following command −

mvn clean install
After “BUILD SUCCESS”, you can find the JAR file under the target directory.

For Gradle, you can use the following command −

gradle clean build
After “BUILD SUCCESSFUL”, you can find the JAR file under the build/libs directory.

You can run the JAR file by using the following command −

java –jar <JARFILE>
This will start the application on the Tomcat port 8080 as shown below −

![exception_handling](/Screenshots/exception_handling.jpg)
Now hit the below URL in POSTMAN application and you can see the output as shown below −

Update URL: http://localhost:8080/products/3

![postman_application_update_url.jpg](/Screenshots/postman_application_update_url.jpg)

------------------------------------------------------------------------------------------------------------------

Introduction
Spring Boot provides us tools to handle exceptions beyond simple ‘try-catch’ blocks. To use these tools, we apply a couple of annotations that allow us to treat exception handling as a cross-cutting concern:

@ResponseStatus
@ExceptionHandler
@ControllerAdvice
Before jumping into these annotations we will first look at how Spring handles exceptions thrown by our web controllers - our last line of defense for catching an exception.

We will also look at some configurations provided by Spring Boot to modify the default behavior.

We’ll identify the challenges we face while doing that, and then we will try to overcome those using these annotations.

Spring Boot’s Default Exception Handling Mechanism
Let’s say we have a controller named ProductController whose getProduct(...) method is throwing a NoSuchElementFoundException runtime exception when a Product with a given id is not found:

@RestController
@RequestMapping("/product")
public class ProductController {
  private final ProductService productService;
  //constructor omitted for brevity...
  
  @GetMapping("/{id}")
  public Response getProduct(@PathVariable String id){
    // this method throws a "NoSuchElementFoundException" exception
    return productService.getProduct(id);
  }
  
}
If we call the /product API with an invalid id the service will throw a NoSuchElementFoundException runtime exception and we’ll get the following response:

{
  "timestamp": "2020-11-28T13:24:02.239+00:00",
  "status": 500,
  "error": "Internal Server Error",
  "message": "",
  "path": "/product/1"
}
We can see that besides a well-formed error response, the payload is not giving us any useful information. Even the message field is empty, which we might want to contain something like “Item with id 1 not found”.

Let’s start by fixing the error message issue.

Spring Boot provides some properties with which we can add the exception message, exception class, or even a stack trace as part of the response payload:

server:
  error:
  include-message: always
  include-binding-errors: always
  include-stacktrace: on_trace_param
  include-exception: false
Using these Spring Boot server properties in our application.yml we can alter the error response to some extent.

Now if we call the /product API again with an invalid id we’ll get the following response:

{
  "timestamp": "2020-11-29T09:42:12.287+00:00",
  "status": 500,
  "error": "Internal Server Error",
  "message": "Item with id 1 not found",
  "path": "/product/1"
} 
Note that we’ve set the property include-stacktrace to on_trace_param which means that only if we include the trace param in the URL (?trace=true), we’ll get a stack trace in the response payload:

{
  "timestamp": "2020-11-29T09:42:12.287+00:00",
  "status": 500,
  "error": "Internal Server Error",
  "message": "Item with id 1 not found",
  "trace": "io.reflectoring.exception.exception.NoSuchElementFoundException: Item with id 1 not found...", 
  "path": "/product/1"
} 
We might want to keep the value of include-stacktrace flag to never, at least in production, as it might reveal the internal workings of our application.

Moving on! The status and error message - 500 - indicates that something is wrong with our server code but actually it’s a client error because the client provided an invalid id.

Our current status code doesn’t correctly reflect that. Unfortunately, this is as far as we can go with the server.error configuration properties, so we’ll have to look at the annotations that Spring Boot offers.

@ResponseStatus
As the name suggests, @ResponseStatus allows us to modify the HTTP status of our response. It can be applied in the following places:

On the exception class itself
Along with the @ExceptionHandler annotation on methods
Along with the @ControllerAdvice annotation on classes
In this section, we’ll be looking at the first case only.

Let’s come back to the problem at hand which is that our error responses are always giving us the HTTP status 500 instead of a more descriptive status code.

To address this we can we annotate our Exception class with @ResponseStatus and pass in the desired HTTP response status in its value property:

@ResponseStatus(value = HttpStatus.NOT_FOUND)
public class NoSuchElementFoundException extends RuntimeException {
  ...
}
This change will result in a much better response if we call our controller with an invalid ID:

{
  "timestamp": "2020-11-29T09:42:12.287+00:00",
  "status": 404,
  "error": "Not Found",
  "message": "Item with id 1 not found",
  "path": "/product/1"
} 
Another way to achieve the same is by extending the ResponseStatusException class:

public class NoSuchElementFoundException extends ResponseStatusException {

  public NoSuchElementFoundException(String message){
    super(HttpStatus.NOT_FOUND, message);
  }

  @Override
  public HttpHeaders getResponseHeaders() {
      // return response headers
  }
}
This approach comes in handy when we want to manipulate the response headers, too, because we can override the getResponseHeaders() method.

@ResponseStatus, in combination with the server.error configuration properties, allows us to manipulate almost all the fields in our Spring-defined error response payload.

But what if want to manipulate the structure of the response payload as well?

Let’s see how we can achieve that in the next section.

@ExceptionHandler
The @ExceptionHandler annotation gives us a lot of flexibility in terms of handling exceptions. For starters, to use it, we simply need to create a method either in the controller itself or in a @ControllerAdvice class and annotate it with @ExceptionHandler:

@RestController
@RequestMapping("/product")
public class ProductController { 
    
  private final ProductService productService;
  
  //constructor omitted for brevity...

  @GetMapping("/{id}")
  public Response getProduct(@PathVariable String id) {
    return productService.getProduct(id);
  }

  @ExceptionHandler(NoSuchElementFoundException.class)
  @ResponseStatus(HttpStatus.NOT_FOUND)
  public ResponseEntity<String> handleNoSuchElementFoundException(
      NoSuchElementFoundException exception
  ) {
    return ResponseEntity
        .status(HttpStatus.NOT_FOUND)
        .body(exception.getMessage());
  }

}
The exception handler method takes in an exception or a list of exceptions as an argument that we want to handle in the defined method. We annotate the method with @ExceptionHandler and @ResponseStatus to define the exception we want to handle and the status code we want to return.

If we don’t wish to use these annotations, then simply defining the exception as a parameter of the method will also do:

@ExceptionHandler
public ResponseEntity<String> handleNoSuchElementFoundException(
    NoSuchElementFoundException exception)
Although it’s a good idea to mention the exception class in the annotation even though we have mentioned it in the method signature already. It gives better readability.

Also, the annotation @ResponseStatus(HttpStatus.NOT_FOUND) on the handler method is not required as the HTTP status passed into the ResponseEnity will take precedence, but we have kept it anyway for the same readability reasons.

Apart from the exception parameter, we can also have HttpServletRequest, WebRequest, or HttpSession types as parameters.

Similarly, the handler methods support a variety of return types such as ResponseEntity, String, or even void.

Find more input and return types in @ExceptionHandler java documentation.

With many different options available to us in form of both input parameters and return types in our exception handling function, we are in complete control of the error response.

Now, let’s finalize an error response payload for our APIs. In case of any error, clients usually expect two things:

An error code that tells the client what kind of error it is. Error codes can be used by clients in their code to drive some business logic based on it. Usually, error codes are standard HTTP status codes, but I have also seen APIs returning custom errors code likes E001.
An additional human-readable message which gives more information on the error and even some hints on how to fix them or a link to API docs.
We will also add an optional stackTrace field which will help us with debugging in the development environment.

Lastly, we also want to handle validation errors in the response. You can find out more about bean validations in this article on Handling Validations with Spring Boot.

Keeping these points in mind we will go with the following payload for the error response:

@Getter
@Setter
@RequiredArgsConstructor
@JsonInclude(JsonInclude.Include.NON_NULL)
public class ErrorResponse {
  private final int status;
  private final String message;
  private String stackTrace;
  private List<ValidationError> errors;

  @Getter
  @Setter
  @RequiredArgsConstructor
  private static class ValidationError {
    private final String field;
    private final String message;
  }

  public void addValidationError(String field, String message){
    if(Objects.isNull(errors)){
      errors = new ArrayList<>();
    }
    errors.add(new ValidationError(field, message));
  }
}
Now, let’s apply all these to our NoSuchElementFoundException handler method.

@RestController
@RequestMapping("/product")
@AllArgsConstructor
public class ProductController {
  public static final String TRACE = "trace";

  @Value("${reflectoring.trace:false}")
  private boolean printStackTrace;
  
  private final ProductService productService;

  @GetMapping("/{id}")
  public Product getProduct(@PathVariable String id){
    return productService.getProduct(id);
  }

  @PostMapping
  public Product addProduct(@RequestBody @Valid ProductInput input){
    return productService.addProduct(input);
  }

  @ExceptionHandler(NoSuchElementFoundException.class)
  @ResponseStatus(HttpStatus.NOT_FOUND)
  public ResponseEntity<ErrorResponse> handleItemNotFoundException(
      NoSuchElementFoundException exception, 
      WebRequest request
  ){
    log.error("Failed to find the requested element", exception);
    return buildErrorResponse(exception, HttpStatus.NOT_FOUND, request);
  }

  @ExceptionHandler(MethodArgumentNotValidException.class)
  @ResponseStatus(HttpStatus.UNPROCESSABLE_ENTITY)
  public ResponseEntity<ErrorResponse> handleMethodArgumentNotValid(
      MethodArgumentNotValidException ex,
      WebRequest request
  ) {
    ErrorResponse errorResponse = new ErrorResponse(
        HttpStatus.UNPROCESSABLE_ENTITY.value(), 
        "Validation error. Check 'errors' field for details."
    );
    
    for (FieldError fieldError : ex.getBindingResult().getFieldErrors()) {
      errorResponse.addValidationError(fieldError.getField(), 
          fieldError.getDefaultMessage());
    }
    return ResponseEntity.unprocessableEntity().body(errorResponse);
  }

  @ExceptionHandler(Exception.class)
  @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
  public ResponseEntity<ErrorResponse> handleAllUncaughtException(
      Exception exception, 
      WebRequest request){
    log.error("Unknown error occurred", exception);
    return buildErrorResponse(
        exception,
        "Unknown error occurred", 
        HttpStatus.INTERNAL_SERVER_ERROR, 
        request
    );
  }

  private ResponseEntity<ErrorResponse> buildErrorResponse(
      Exception exception,
      HttpStatus httpStatus,
      WebRequest request
  ) {
    return buildErrorResponse(
        exception, 
        exception.getMessage(), 
        httpStatus, 
        request);
  }

  private ResponseEntity<ErrorResponse> buildErrorResponse(
      Exception exception,
      String message,
      HttpStatus httpStatus,
      WebRequest request
  ) {
    ErrorResponse errorResponse = new ErrorResponse(
        httpStatus.value(), 
        exception.getMessage()
    );
    
    if(printStackTrace && isTraceOn(request)){
      errorResponse.setStackTrace(ExceptionUtils.getStackTrace(exception));
    }
    return ResponseEntity.status(httpStatus).body(errorResponse);
  }

  private boolean isTraceOn(WebRequest request) {
    String [] value = request.getParameterValues(TRACE);
    return Objects.nonNull(value)
        && value.length > 0
        && value[0].contentEquals("true");
  }
}
Couple of things to note here:

Providing a Stack Trace
Providing stack trace in the error response can save our developers and QA engineers the trouble of crawling through the log files.

As we saw in Spring Boot’s Default Exception Handling Mechanism, Spring already provides us with this functionality. But now, as we are handling error responses ourselves, this also needs to be handled by us.

To achieve this, we have first introduced a server-side configuration property named reflectoring.trace which, if set to true, To achieve this, we have first introduced a server-side configuration property named reflectoring.trace which, if set to true, will enable the stackTrace field in the response. To actually get a stackTrace in an API response, our clients must additionally pass the trace parameter with the value true:

curl --location --request GET 'http://localhost:8080/product/1?trace=true'
Now, as the behavior of stackTrace is controlled by our feature flag in our properties file, we can remove it or set it to false when we deploy in production environments.

Catch-All Exception Handler
Gotta catch em all:

try{
  performSomeOperation();
} catch(OperationSpecificException ex){
  //...
} catch(Exception catchAllExcetion){
  //...  
}
As a cautionary measure, we often surround our top-level method’s body with a catch-all try-catch exception handler block, to avoid any unwanted side effects or behavior. The handleAllUncaughtException() method in our controller behaves similarly. It will catch all the exceptions for which we don’t have a specific handler.

One thing I would like to note here is that even if we don’t have this catch-all exception handler, Spring will handle it anyway. But we want the response to be in our format rather than Spring’s, so we have to handle the exception ourselves.

A catch-all handler method is also be a good place to log exceptions as they might give insight into a possible bug. We can skip logging on field validation exceptions such as MethodArgumentNotValidException as they are raised because of syntactically invalid input, but we should always log unknown exceptions in the catch-all handler.

Order of Exception Handlers
The order in which you mention the handler methods doesn’t matter. Spring will first look for the most specific exception handler method.

If it fails to find it then it will look for a handler of the parent exception, which in our case is RuntimeException, and if none is found, the handleAllUncaughtException() method will finally handle the exception.

This should help us handle the exceptions in this particular controller, but what if these same exceptions are being thrown by other controllers too? How do we handle those? Do we create the same handlers in all controllers or create a base class with common handlers and extend it in all controllers?

Luckily, we don’t have to do any of that. Spring provides a very elegant solution to this problem in form of “controller advice”.

Let’s study them.

@ControllerAdvice
Why is it called "Controller Advice"?
The term 'Advice' comes from Aspect-Oriented Programming (AOP) which allows us to inject cross-cutting code (called "advice") around existing methods. A controller advice allows us to intercept and modify the return values of controller methods, in our case to handle exceptions.

Controller advice classes allow us to apply exception handlers to more than one or all controllers in our application:

@ControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

  public static final String TRACE = "trace";

  @Value("${reflectoring.trace:false}")
  private boolean printStackTrace;

  @Override
  @ResponseStatus(HttpStatus.UNPROCESSABLE_ENTITY)
  protected ResponseEntity<Object> handleMethodArgumentNotValid(
      MethodArgumentNotValidException ex,
      HttpHeaders headers,
      HttpStatus status,
      WebRequest request
  ) {
      //Body omitted as it's similar to the method of same name
      // in ProductController example...  
      //.....
  }

  @ExceptionHandler(ItemNotFoundException.class)
  @ResponseStatus(HttpStatus.NOT_FOUND)
  public ResponseEntity<Object> handleItemNotFoundException(
      ItemNotFoundException itemNotFoundException, 
      WebRequest request
  ){
      //Body omitted as it's similar to the method of same name
      // in ProductController example...  
      //.....  
  }

  @ExceptionHandler(RuntimeException.class)
  @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
  public ResponseEntity<Object> handleAllUncaughtException(
      RuntimeException exception, 
      WebRequest request
  ){
      //Body omitted as it's similar to the method of same name
      // in ProductController example...  
      //.....
  }
  
  //....

  @Override
  public ResponseEntity<Object> handleExceptionInternal(
      Exception ex,
      Object body,
      HttpHeaders headers,
      HttpStatus status,
      WebRequest request) {

    return buildErrorResponse(ex,status,request);
  }

}
The bodies of the handler functions and the other support code are omitted as they’re almost identical to the code we saw in the @ExceptionHandler section. Please find the full code in the Github Repo’s GlobalExceptionHandler class.

A couple of things are new which we will talk about in a while. One major difference here is that these handlers will handle exceptions thrown by all the controllers in the application and not just ProductController.

If we want to selectively apply or limit the scope of the controller advice to a particular controller, or a package, we can use the properties provided by the annotation:

@ControllerAdvice("com.reflectoring.controller"): we can pass a package name or list of package names in the annotation’s value or basePackages parameter. With this, the controller advice will only handle exceptions of this package’s controllers.
@ControllerAdvice(annotations = Advised.class): only controllers marked with the @Advised annotation will be handled by the controller advice.
Find other parameters in the @ControllerAdvice annotation docs.

ResponseEntityExceptionHandler
ResponseEntityExceptionHandler is a convenient base class for controller advice classes. It provides exception handlers for internal Spring exceptions. If we don’t extend it, then all the exceptions will be redirected to DefaultHandlerExceptionResolver which returns a ModelAndView object. Since we are on the mission to shape our own error response, we don’t want that.

As you can see we have overridden two of the ResponseEntityExceptionHandler methods:

handleMethodArgumentNotValid(): in the @ExceptionHandler section we have implemented a handler for it ourselves. In here we have only overridden its behavior.
handleExceptionInternal(): all the handlers in the ResponseEntityExceptionHandler use this function to build the ResponseEntity similar to our buildErrorResponse(). If we don’t override this then the clients will receive only the HTTP status in the response header but since we want to include the HTTP status in our response bodies as well, we have overridden the method.
Handling NoHandlerFoundException Requires a Few Extra Steps
This exception occurs when you try to call an API that doesn't exist in the system. Despite us implementing its handler via ResponseEntityExceptionHandler class the exception is redirected to DefaultHandlerExceptionResolver.

To redirect the exception to our advice we need to set a couple of properties in the the properties file: spring.mvc.throw-exception-if-no-handler-found=true and spring.web.resources.add-mappings=false

Some Points to Keep in Mind when Using @ControllerAdvice
To keep things simple always have only one controller advice class in the project. It’s good to have a single repository of all the exceptions in the application. In case you create multiple controller advice, try to utilize the basePackages or annotations properties to make it clear what controllers it’s going to advise.
Spring can process controller advice classes in any order unless we have annotated it with the @Order annotation. So, be mindful when you write a catch-all handler if you have more than one controller advice. Especially when you have not specified basePackages or annotations in the annotation.
How Does Spring Process The Exceptions?
Now that we have introduced the mechanisms available to us for handling exceptions in Spring, let’s understand in brief how Spring handles it and when one mechanism gets prioritized over the other.

Have a look through the following flow chart that traces the process of the exception handling by Spring if we have not built our own exception handler:

![Spring_Exception_Process](/Screenshots/Spring_Exception_Process.jpg)

-------------------------------------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------------------------------------

_Resources:_
https://dzone.com/articles/spring-boot-exception-handling (Added)
https://www.tutorialspoint.com/spring_boot/spring_boot_exception_handling.htm (Added)
https://reflectoring.io/spring-boot-exception-handling/ (Added)
https://www.toptal.com/java/spring-boot-rest-api-error-handling

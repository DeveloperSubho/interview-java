**_Spring MVC Annotations_**

- @Controller - The @Controller annotation is used to indicate the class is a Spring controller. This annotation is simply a specialization of the @Component class and allows implementation classes to be auto-detected through the class path scanning. It marks the class as a web controller, capable of handling the HTTP requests. Spring will look at the methods of the class marked with the @Controller annotation and establish the routing table to know which methods serve which endpoints.

- @Service - @Service marks a Java class that performs some service, such as executing business logic, performing calculations, and calling external APIs. This annotation is a specialized form of the @Component annotation intended to be used in the service layer.

- @Repository - This annotation is used on Java classes that directly access the database. The @Repository annotation works as a marker for any class that fulfills the role of repository or Data Access Object. This annotation has an automatic translation feature. For example, when an exception occurs in the @Repository, there is a handler for that exception and there is no need to add a try-catch block.

- @RequestMapping - @RequestMapping marks request handler methods inside @Controller classes. It accepts below options:  
  path/name/value: which URL the method is mapped to.  
  method: compatible HTTP methods.  
  params: filters requests based on presence, absence, or value of HTTP parameters.  
  headers: filters requests based on presence, absence, or value of HTTP headers.  
  consumes: which media types the method can consume in the HTTP request body.  
  produces: which media types the method can produce in the HTTP response body.

- @RequestBody - @RequestBody indicates a method parameter should be bound to the body of the web request. It maps the body of the HTTP request to an object. The body of the request is passed through an HttpMessageConverter to resolve the method argument depending on the content type of the request. The deserialization is automatic and depends on the content type of the request.

- @GetMapping - @GetMapping is used for mapping HTTP GET requests onto specific handler methods.
  Specifically, @GetMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.GET).

- @PostMapping - @PostMapping is used for mapping HTTP POST requests onto specific handler methods. Specifically, @PostMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.POST).

- @PutMapping - @PutMapping is used for mapping HTTP PUT requests onto specific handler methods. Specifically, @PutMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.PUT).

- @DeleteMapping - @DeleteMapping is used for mapping HTTP DELETE requests onto specific handler methods.Specifically, @DeleteMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.DELETE).

- @PatchMapping - @PatchMapping is used for mapping HTTP PATCH requests onto specific handler methods. Specifically, @PatchMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.PATCH).

- @ControllerAdvice - @ControllerAdvice is applied at the class level. For each controller, you can use @ExceptionHandler on a method that will be called when a given exception occurs. But this handles only those exceptions that occur within the controller in which it is defined. To overcome this problem, you can now use the @ControllerAdvice annotation. This annotation is used to define @ExceptionHandler, @InitBinder, and @ModelAttribute methods that apply to all @RequestMapping methods. Thus, if you define the @ExceptionHandler annotation on a method in a @ControllerAdvice class, it will be applied to all the controllers.

- @ResponseBody - @ResponseBody on a request handler method tells Spring to convert the return value and writes it to the HTTP response automatically. It tells Spring to treat the result of the method as the response itself.
  The @ResponseBody annotation tells a controller that the object returned is automatically serialized into JSON and passed back into the HttpResponse object. If we annotate a @Controller class with this annotation, all request handler methods will use it.
  The @ResponseBody is a utility annotation that makes Spring bind a method's return value to the HTTP response body. When building a JSON endpoint, this is an amazing way to magically convert your objects into JSON for easier consumption.

- @ExceptionHandler - @ExceptionHandler is used to declare a custom error handler method. Spring calls this method when a request handler method throws any of the specified exceptions.
  The caught exception can be passed to the method as an argument.

- @ResponseStatus - @ResponseStatus is used to specify the desired HTTP status of the response if we annotate a request handler method with this annotation. We can declare the status code with the code argument, or its alias, the value argument.

- @PathVariable - @PathVariable is used to indicates that a method argument is bound to a URI template variable. We can specify the URI template with the @RequestMapping annotation and bind a method argument to one of the template parts with @PathVariable.

- @RequestParam - @RequestParam indicates that a method parameter should be bound to a web request parameter. We use @RequestParam for accessing HTTP request parameters. With @RequestParam we can specify an injected value when Spring finds no or empty value in the request. To achieve this, we have to set the defaultValue argument.

- @RestController - @RestController combines @Controller and @ResponseBody. By annotating the controller class with @RestController annotation, we no longer need to add @ResponseBody to all the request mapping methods. Then there's the @RestController annotation, a convenience syntax for @Controller and @ResponseBody together. This means that all the action methods in the marked class will return the JSON response.

- @ModelAttribute - @ModelAttribute is used to access elements that are already in the model of an MVC @Controller, by providing the model key.

- @CrossOrigin - @CrossOrigin enables cross-domain communication for the annotated request handler methods. If we mark a class with it, it applies to all request handler methods in it. We can fine-tune CORS behavior with this annotation’s arguments.

- @InitBinder - @InitBinder is a method-level annotation that plays the role of identifying the methods that initialize the WebDataBinder — a DataBinder that binds the request parameter to Java Bean objects. To customize request parameter data binding, you can use @InitBinder annotated methods within our controller. The methods annotated with @InitBinder include all argument types that handler methods support. The @InitBinder annotated methods will get called for each HTTP request if we don’t specify the value element of this annotation. The value element can be a single or multiple form names or request parameters that the init binder method is applied to.

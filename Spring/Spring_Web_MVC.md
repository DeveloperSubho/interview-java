**1. How to Get ServletContext and ServletConfig Objects in a Spring Bean?**
We can do either by implementing Spring-aware interfaces. The complete list is available here.

We could also use @Autowired annotation on those beans:

```
@Autowired
ServletContext servletContext;

@Autowired
ServletConfig servletConfig;
```

**2. What Is a Controller in Spring MVC?**
Simply put, all the requests processed by the DispatcherServlet are directed to classes annotated with @Controller. Each controller class maps one or more requests to methods that process and execute the requests with provided inputs.

To take a step back, we recommend having a look at the concept of the Front Controller in the typical Spring MVC architecture.

**3. How Does the @RequestMapping Annotation Work?**
The @RequestMapping annotation is used to map web requests to Spring Controller methods. In addition to simple use cases, we can use it for mapping of HTTP headers, binding parts of the URI with @PathVariable, and working with URI parameters and the @RequestParam annotation.

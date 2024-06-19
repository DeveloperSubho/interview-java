## ResponseEntity

ResponseEntity represents the whole HTTP response: status code, headers, and body. As a result, we can use it to fully configure the HTTP response.

If we want to use it, we have to return it from the endpoint; Spring takes care of the rest.

ResponseEntity is a generic type. Consequently, we can use any type as the response body:

```
public class ResponseEntity<T> extends HttpEntity<T>
```

Extension of HttpEntity that adds an HttpStatus status code. Used in RestTemplate as well as in @Controller methods.
In RestTemplate, this class is returned by getForEntity() and exchange():

```
ResponseEntity<String> entity = template.getForEntity("https://example.com", String.class);
String body = entity.getBody();
MediaType contentType = entity.getHeaders().getContentType();
HttpStatus statusCode = entity.getStatusCode();
```

This can also be used in Spring MVC as the return value from an @Controller method:

```
@RequestMapping("/handle")
public ResponseEntity<String> handle() {
  URI location = ...;
  HttpHeaders responseHeaders = new HttpHeaders();
  responseHeaders.setLocation(location);
  responseHeaders.set("MyResponseHeader", "MyValue");
  return new ResponseEntity<String>("Hello World", responseHeaders, HttpStatus.CREATED);
}
```

While ResponseEntity is very powerful, we shouldn't overuse it. In simple cases, there are other options that satisfy our needs and they result in much cleaner code.

Alternatives

### @ResponseBody

In classic Spring MVC applications, endpoints usually return rendered HTML pages. Sometimes we only need to return the actual data; for example, when we use the endpoint with AJAX.

In such cases, we can mark the request handler method with @ResponseBody, and Spring treats the result value of the method as the HTTP response body itself.

### @ResponseStatus

When an endpoint returns successfully, Spring provides an HTTP 200 (OK) response. If the endpoint throws an exception, Spring looks for an exception handler that tells which HTTP status to use.

We can mark these methods with @ResponseStatus, and therefore, Spring returns with a custom HTTP status.

_References_
https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html
https://www.baeldung.com/spring-response-entity
https://www.javaguides.net/2019/08/spring-responseentity-using-responseentity-in-spring-application.html

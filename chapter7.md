1. What does REST stand for?
    REST stands for REpresentational State Transfer and is an architectural style which allows clients
    to access and manipulate textual representations of web resources given a set of constraints.
    If the application does observe the constraints, certain benefits are gained. Examples of such
    benefits are scalability, simplicity, portability and reliability.

2. What is a resource?
    Anything that can be named is a resource. Usually that nouns that define our model.

3. What does CRUD mean?
    The CRUD acronym stands for Create, Read, Update and Delete. These are the basic operations on
    REST resources, as well as persistent storage.

4. Is REST secure? What can you do to secure it?
    The REST architectural style in itself does not stipulate any particular security solution but suggests
    using a layered system style using one or more intermediaries. Security may be a responsibility of
    one type of intermediary.
    This maps very well to what Spring has to offer when it comes to developing REST web services:
    A REST web service can be developed using Spring without having to consider security at all.
    Security can later be added to the REST web service using Spring Security, as described in the
    previous chapter.
    While security, such as basic authentication, will make a REST service unavailable to everyone
    except those who know the login information, the messages passed between clients and the service
    will still be readable by anyone able to intercept them. To protect the messages in transit, encryption
    can be used such as with the HTTPS protocol.

5. Is REST scalable and/or interoperable?
    The short answer is yes, REST web services are both scalable and interoperable.

6. Which HTTP methods does REST use?
    Create POST
    Read GET
    Update PUT
    Delete DELETE

7. What is an HttpMessageConverter?
    HttpMessageConverters convert HTTP requests to objects and back from objects to HTTP responses. Spring has a list of converters that is registered by default but it is customizable - additional implementations may be plugged in.

8. Is REST normally stateless?
    Statelessness of a REST service is one of the fundamental constraints of the REST architectural
    style. Thus REST is always stateless or else it is no longer REST.
    With this said, a REST client may hold state of some kind and enclose data being part of this state
    in requests to a REST service. The server however is not aware of the data being part of some state
    and the server will not retain any such data between requests.

9. What does @RequestMapping do?
    The @RequestMapping annotation marks a method in a controller as a method that will be invoked
    to handle requests that match the configuration in the @RequestMapping annotation

10. Is @Controller a stereotype? Is @RestController a stereotype?
    @Controller is a stereotype. @RestController is not. Its a convenience annotation that is itself annotated with @Controller and @ResponseBody

    • What is a stereotype annotation? What does that mean?
    Annotations denoting the roles of types or methods in the overall architecture (at a conceptual, rather than implementation, level). When you suggest a role of a particular class being annotated. (Controller classes should be annotated controller, service classes should be annotated service)

11. What is the difference between @Controller and @RestController?
    A REST controller is processed by a HttpMessageConverter. The @RestController annotation tells Spring to render the resulting string directly back to the caller for web requests.

    @RestController is used on the server side.

12. When do you need @ResponseBody?
    The @ResponseBody annotation can be applied to either a controller class or a controller handler
    method. It causes the response to be created from the serialized result of the controller method
    result processed by a HttpMessageConverter. This is useful when you want the web response body
    to contain the result produced by the controller method, as is the case when implementing a
    backend service, for example a REST service.
    The @ResponseBody annotation is not needed if the controller class is annotated with
    @RestController

12. What are the HTTP status return codes for a successful GET, POST, PUT or DELETE operation?
    200, 201 Created, 202 Accepted, 204 No Content

13. When do you need @ResponseStatus?
    @ResponseStatus will prevent DispatcherServlet from trying to find a view for the result and will set a status for the response. So for delete methods that return void you can use a response status (no content)

    @ResponseStatus( value = HttpStatus.NOT_FOUND, reason = "Not Found")

14. Where do you need @ResponseBody? What about @RequestBody? Try not to get these muddled up!
    @ResponseBody
    As earlier, the @ResponseBody annotation is an annotation that can be applied to controller classes
    or controller handler methods. It is used when the web response body is to contain the result
    produced by the controller method(s).
    @RequestBody
    The @RequestBody annotation can only be applied to parameters of methods. More specific,
    parameters of controller handler methods. It is used when the web request body is to be bound to a
    parameter of the controller handler method. That is, the annotated method parameter will contain
    the web request body when there is a request to be handled by the controller handler method with
    the annotated parameter.

15. Do you need Spring MVC in your classpath?
    Yes. Spring MVC is the core component for REST support.

16. What Spring Boot starter would you use for a Spring REST application?
    The Spring Boot Web Starter Maven pom.xml file contains the following in the description:
    “Starter for building web, including RESTful, applications using Spring MVC.”
    I would thus use the Spring Boot Web Starter if I were to implement a RESTful web service in a
    Spring Boot application

17. What are the advantages of the RestTemplate?
    RestTemplate is a synchronous client to perform HTTP requests. It is the original Spring REST client and exposes a simple, template-method API over underlying HTTP client libraries. RestTemplate has methods specific to HTTP methods.


1. MVC is an abbreviation for a design pattern. What does it stand for and what is the idea
behind it?
    MVC stands for Model-View-Controller. The idea is to separate these three concepts rather than mixing them – as it is very hard to work when you have a view mixed with a controller and you need to change some details of the view.
    Model
    The model holds the current data and business logic of the application.
    View
    The view is responsible for presenting the data of the application to the user. The user
    interacts with the view.
    Controller
    Controllers acts as a mediator between the model and the view accepting requests from the
    view, issuing commands to the model to manipulate the data of the application and finally
    interacts with the view to render the result

2. What is the DispatcherServlet and what is it used for?
    • Receives requests and delegates them to registered handlers.
    If only one DispatcherServlet is used in the application, then this servlet will receive all
    requests that are sent to the application.
    • Resolves views by mapping view-names to View instances.
    • Resolves exceptions that occur during handler mapping or execution.
    The most common is to map an exception to an error view

3. What is a web application context? What extra scopes does it offer?
    Web application context, specified by the WebApplicationContext interface, is a Spring application
    context for a web applications. It has all the properties of a regular Spring application context, given
    that the WebApplicationContext interface extends the ApplicationContext interface, and add a
    method for retrieving the standard Servlet API ServletContext for the web application.

    DispatcherServlet expects a WebApplicationContext (an extension of a plain ApplicationContext) for its own configuration. WebApplicationContext has a link to the ServletContext and the Servlet with which it is associated

    A Spring MVC application usually has 2 application context – one for the web controllers etc. and another one for the “base” application components (services, data sources etc.).

4. What is the @Controller annotation used for?
    The @Controller annotation is a specialization of the @Component annotation that have been
    discussed earlier. It is used to annotate classes that implement web controllers, the C in MVC.
    Such classes need not extend any particular parent class or implement any particular interfaces.

5. How is an incoming request mapped to a controller and mapped to a method?
    Via the @Controller and @RequestMapping annotations via the servlet dispatcher

    Enable component scanning.
    This enables auto-detection of classes annotated with the @Controller annotation.
    In

    Spring Boot applications, the @SpringBootApplication annotation is an annotation that
    itself is annotated with a number of meta-annotation one of which is the @ComponentScan
    annotation.
    • Annotate one of the configuration-classes in the application with the @EnableWebMvc
    annotation.
    In Spring Boot applications, it is sufficient to have one configuration-class in the application
    implement the WebMvcConfigurer interface.
    • Implement a controller class that is annotated with the @Controller annotation.
    Controller classes can also be annotated with the @RequestMapping annotation, in which
    case it will add a part to the URL which will map to controller method(s).
    Example: Controller class is annotated with @RequestMapping("/c2") and a method in the
    controller class is annotate with @RequestMapping("/greeting"). A request to
    http://localhost:8080/c2/greeting will be mapped to the to the method in the controller class
    in the example.
    • Implement at least one method in the controller class and annotate the method with
    @RequestMapping.
    Instead of the @RequestMapping annotation, one of the specialized annotations can be used.
    An example is @GetMapping.
    This type of methods are called controller method or handler method.

    When a request is issued to the application:
    • The/a DispatcherServlet of the application receives the request.
    • The/a DispatcherServlet maps the request to a method in a controller.
    The DispatcherServlet holds a list of classes implementing the HandlerMapping interface.
    • The/a DispatcherServlet dispatches the request to the controller.
    • The method in the controller is executed.

6. What is the difference between @RequestMapping and @GetMapping?
    @GetMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.GET).

7. What is @RequestParam used for?
    The @RequestParam annotation is used to annotate parameters to handler methods in order to bind
    request parameters to method parameters.
    Assume there is a controller method with the following signature and annotations:
    @RequestMapping("/greeting")
     public String greeting(@RequestParam(name="name", required=false) String inName) {
     …
     }
     If then a request is sent to the URL http://localhost:8080/greeting?name=Ivan then the inName
    method parameter will contain the string “Ivan”.

8. What are the differences between @RequestParam and @PathVariable?
    @PathVariable gets mapping method parameters from the URI itself, while @RequestParam gets them from URI parameters. for @PathVariable: http://www.foo.com/user/33 for @RequestParam: http://www.foo.com/user?id=33

9. What are some of the parameter types for a controller method?
    ServletRequest, HttpServletRequest, HttpSession, WebRequest, NativeWebRequest, Locale, Reader, InputStream, OutputStream, Principal, HttpEntity, Map, ModelMap, RedirectAttributes, Errors, BindingResult, SessionStatus, UriComponentsBuilder

    • What other annotations might you use on a controller method parameter? (You can
    ignore form-handling annotations for this exam)
    @PathVariable, @RequestParam, @RequestHeader, @RequestBody, @RequestPart

10. What are some of the valid return types of a controller method?
    ModelAndView , Model, Map, View, String, void, HttpBody, HttpEntity or ResponseEntity, HttpHeaders, Callable< ?>, DeferredResult< ?>, ListenableFuture< ?>, ResponseBodyEmitter, SseEmitter, StreamingResponseBody




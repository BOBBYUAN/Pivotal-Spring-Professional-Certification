--Spring Boot Actuator
24. What value does Spring Boot Actuator provide?
    The spring-boot-actuator module provides all of Spring Boot’s production-ready features.

25. What are the two protocols you can use to access actuator endpoints?
    JMX and HTTP

26. What are the actuator endpoints that are provided out of the box?
    The /actuator endpoint will provide a hypermedia-based discovery page for all the other endpoints, but it will require the Spring HATEOAS in the classpath.

    /autoconfig This endpoint will display the auto-configuration report. It will give you two groups: positiveMatches and negativeMatches.

    /beans This endpoint will display all the Spring beans that are used in your application. R

    /configprops This endpoint will list all the configuration properties that are defined by the @ConfigurationProperties beans,

    /docs This endpoint will show HTML pages with all the documentation for all the Actuator module endpoints. This endpoint can be activated by including the spring-boot-actuator-docs dependency in pom.xml

    /dump This endpoint will perform a thread dump of your application.

    /env This endpoint will expose all the properties from the Spring’s ConfigurableEnvironment interface. This will show any active profiles and system environment variables and all application properties, including the Spring Boot properties.

    /flyway This endpoint will provide all the information about your database migration scripts; it’s based on the Flyway project (https://flywaydb.org/). This is very useful when you want to have full control of your database by versioning your schemas. I

    /health This endpoint will show the health of the application. If you are doing a database app like in the previous section (/flyway) you will see the DB status and by default you will see also the diskSpace from your system.

    /info This endpoint will display the public application info. This means that you need to add this information to application.properties. It’s recommended that you add it if you have multiple Spring Boot applications.

    /logfile This endpoint will show the contents of the log file specified by the logging.file property, where you specify the name of the log file (this will be written in the current directory). You can also set the logging.path, where you set the path where the spring.log will be written. By default Spring Boot writes to the console/standard out, and if you specify any of these properties, it will also write everything from the console to the log file. You can stop your application. Go to src/main/resources/application.properties and add this to the very end:

    logging.file=mylog.log
    /metrics This endpoint shows the metrics information of the current application, where you can determine the how much memory it’s using, how much memory is free, the uptime of your application, the size of the heap is being used, the number of threads used, and so on.

    /mappings This endpoint shows all the lists of all @RequestMapping paths declared in your application. This is very useful if you want to know more about what mappings are declared.

    /shutdown This endpoint is not enabled by default.

27. What is info endpoint for? How do you supply data?
    f you added any information about the application in the application.properties file using the info.app.* properties, then you can view it at the http://localhost:8080/application/info endpoint.

    info.app.name=Spring Boot Web Actuator Application
    info.app.description=This is an example of the Actuator module
    info.app.version=1.0.0

28. How do you change logging level of a package using loggers endpoint?
    To configure a given logger, POST a partial entity to the resource’s URI, as shown in the following example:
    {
    "configuredLevel": "DEBUG"
    }

    To “reset” the specific level of the logger (and use the default configuration instead), you can pass a value of null as the configuredLevel.


29. How do you access an endpoint using a tag?
    For example, you know that there have been 2,103 requests, but what’s unknown is how many of them resulted in an HTTP 200 versus an HTTP 404 or HTTP 500 response status. Using the status tag, you can get metrics for all requests resulting in an HTTP 404 status like this:

    $ curl localhost:8081/actuator/metrics/http.server.requests?tag=status:404

30. What is metrics for?
    This endpoint shows the metrics information of the current application, where you can determine the how much memory it’s using, how much memory is free, the uptime of your application, the size of the heap is being used, the number of threads used, and so on.

    One of the important features about this endpoint is that it has some counters and gauges that you can use, even for statistics about how many times your app is being visited or if you have the log file enabled. If you are accessing the /logfile endpoint, you will find some counters like counter.status.304.logfile, which indicates that the /logfile endpoint was accessed but hasn’t change. And of course you can have custom counters.

31. How do you create a custom metric with or without tags?
    Ultimately, Actuator metrics are implemented by Micrometer. The most basic means of publishing metrics with Micrometer is through Micrometer’s MeterRegistry.

    In a Spring Boot application, all you need to do to publish metrics is to inject a MeterRegistry wherever you may need to publish counters, timers, or gauges that capture the metrics for your application.

    To register custom metrics, inject MeterRegistry into your component,
    @Component
    public class TacoMetrics extends AbstractRepositoryEventListener<Taco> {
      private MeterRegistry meterRegistry;
      public TacoMetrics(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
      }

      @Override
      protected void onAfterCreate(Taco taco) {
        List<Ingredient> ingredients = taco.getIngredients();
        for (Ingredient ingredient : ingredients) {
          meterRegistry.counter("tacocloud", "ingredient", ingredient.getId()).increment();
        }
      }
    }

    $ curl localhost:8087/actuator/metrics/tacocloud

    {
    "name": "tacocloud",
    "measurements": [
      { "statistic": "COUNT", "value": 84 } ],
    "availableTags": [
      {"tag": "ingredient",
      "values": [ "FLTO", "CHED", "LETC", "GRBF", "COTO", "JACK", "TMTO", "SLSA"]} ]
    }

    Without Tag:
    class Dictionary {
      private final List<String> words = new CopyOnWriteArrayList<>();
      Dictionary(MeterRegistry registry) {
        registry.gaugeCollectionSize("dictionary.size", Tags.empty(), this.words);
      }
    }
32. What is Health Indicator?
    This endpoint will show the health of the application. If you are doing a database app like in the previous section (/flyway) you will see the DB status and by default you will see also the diskSpace from your system. If you are running your app, you can go to http://localhost:8080/health.

33. What are the Health Indicators that are provided out of the box?
    See in the image.

34. What is the Health Indicator status?
    DOWN SERVICE_UNAVAILABLE (503)

    OUT_OF_SERVICE SERVICE_UNAVAILABLE (503)

    UP No mapping by default, so http status is 200

    UNKNOWN No mapping by default, so http status is 200

35. What are the Health Indicator statuses that are provided out of the box
    DOWN SERVICE_UNAVAILABLE (503)

    OUT_OF_SERVICE SERVICE_UNAVAILABLE (503)

    UP No mapping by default, so http status is 200

    UNKNOWN No mapping by default, so http status is 200

36. How do you change the Health Indicator status severity order?
    Assume a new Status with code FATAL is being used in one of your HealthIndicator implementations.
    You might also want to register custom status mappings if you access the health endpoint over HTTP.

    management.health.status.order=FATAL, DOWN, OUT_OF_SERVICE, UNKNOWN, UP
    management.health.status.http-mapping.FATAL=503

37. Why do you want to leverage 3rd-party external monitoring system?
    Spring Boot auto-configures a composite MeterRegistry and adds a registry to the composite for each of the supported implementations that it finds on the classpath.

    Having a dependency on micrometer-registry-{system} in your runtime classpath is enough for Spring Boot to configure the registry.

--Spring Boot Testing
1. When do you want to use @SpringBootTest annotation?
    The test context framework will be searching for the class annotated with @SpringBootApplication (if no specific configuration is passed) and will use that to actually start the application.

    @RunWith(SpringRunner.class)
    @SpringBootTest(classes = CalculatorApplication.class)
    public class CalculatorApplicationTests {

      @Autowired private Calculator calculator;

      @Test(expected = IllegalArgumentException.class)
      public void doingDivisionShouldFail() {
        calculator.calculate(12,13, '/');
      }
    }

2. What does @SpringBootTest auto-configure?
    Spring boot provides the @SpringBootTest annotation to configure the ApplicationContext for tests that use SpringApplication behind the scenes so that all the Spring Boot features will be available.

    For @SpringBootTest, you can pass

    Spring configuration classes,
    Spring bean definition XML files,
    and more,
    In Spring Boot applications, you’ll typically use the entry point class.

3. What dependencies does spring-boot-starter-test brings to the classpath?
    The Spring Boot Test starter spring-boot-starter-test pulls in
    Spring Test
    Spring Boot Test modules
    JSONPath,
    JUnit,
    AssertJ,
    Mockito,
    Hamcrest,
    JSONassert

4. How do you perform integration testing with @SpringBootTest for a web application?
    To properly test a web application, you need a way to throw actual HTTP requests at it and assert that it processes those requests correctly. Two options:

    Spring Mock MVC — Enables controllers to be tested in a mocked approximation of a servlet container without actually starting an application server. To set up a Mock MVC in your test, you can use MockMvcBuilders.
    standaloneSetup() — Builds a Mock MVC to serve one or more manually created and configured controllers. It expects you to manually instantiate and inject the controllers you want to test, whereas webAppContextSetup() works from an instance of WebApplicationContext, which itself was probably loaded by Spring. The former is slightly more akin to a unit test in that you’ll likely only use it for very focused tests around a single controller.
    webAppContextSetup() — Builds a Mock MVC using a Spring application context, which presumably includes one or more configured controllers. lets Spring load your controllers as well as their dependencies for a full-blown integration test.
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringApplicationConfiguration( classes = ReadingListApplication.class)
    @WebAppConfiguration public class MockMvcWebTests {
      @Autowired
      private WebApplicationContext webContext;

      private MockMvc mockMvc;

      @Before
      public void setupMockMvc() {

        mockMvc = MockMvcBuilders
          .webAppContextSetup(webContext)
          .build();
      }

      @Test
      public void homePage() throws Exception {
      mockMvc.perform(MockMvcRequestBuilders.get("/readingList"))
        .andExpect(MockMvcResultMatchers.status().isOk())
        .andExpect(MockMvcResultMatchers.view().name("readingList"))
        .andExpect(MockMvcResultMatchers.model().attributeExists("books"))
        .andExpect(MockMvcResultMatchers.model().attribute("books", Matchers.is(Matchers.empty())));
      }
    }
    Web integration tests — Actually starts the application in an embedded servlet container (such as Tomcat or Jetty), enabling tests that exercise the application in a real application server.
    uses @WebIntegrationTest to start the application along with a server and uses Spring’s RestTemplate to perform HTTP requests against the application.
    @RunWith(SpringJUnit4ClassRunner.class)
    @SpringApplicationConfiguration( classes=ReadingListApplication.class)
    @WebIntegrationTest
    public class SimpleWebTest {

      @Test(expected=HttpClientErrorException.class)
      public void pageNotFound() {

        try {
          RestTemplate rest = new RestTemplate();
          rest.getForObject( "http://localhost:8080/bogusPage", String.class);
          fail("Should result in HTTP 404");
        } catch (HttpClientErrorException e) {
          assertEquals(HttpStatus.NOT_FOUND, e.getStatusCode());
          throw e;
        }
      }
    }
5. When do you want to use @WebMvcTest? What does it auto-configure?
    Spring Boot provides the @WebMvcTest annotation, which will autoconfigure SpringMVC infrastructure components and load only

    @Controller,
    @ControllerAdvice,
    @JsonComponent,
    Filter,
    WebMvcConfigurer, and
    HandlerMethodArgumentResolver components.
    Other Spring beans (annotated with @Component, @Service, @Repository, etc.) will not be scanned when using this annotation.

    In contrast to @SpringBootTest, which loads the entire configuration.

    @RunWith(SpringRunner.class)
    @WebMvcTest(controllers= TodoController.class)
    public class TodoControllerTests {

      @Autowired
      private MockMvc mvc;

      @MockBean
      private TodoRepository todoRepository;

      @Test
      public void testShowAllTodos() throws Exception {

        Todo todo1 = new Todo(1, "Todo1",false);
        Todo todo2 = new Todo(2, "Todo2",true);

        given(this.todoRepository.findAll())
          .willReturn(Arrays.asList(todo1, todo2));

        this.mvc.perform(get("/todolist")
          .accept(MediaType.TEXT_HTML))
          .andExpect(status().isOk())
          .andExpect(view().name("todos"))
          .andExpect(model().attribute("todos", hasSize(2))) ;

          verify(todoRepository, times(1)).findAll();
      }
    }
6. What are the differences between @MockBean and @Mock?
    @Mock @Mock = Mockito.mock(). It's a from Mockito library.

    @MockBean

    This is indeed a Spring Boot class.
    It allows to add Mockito mocks in a Spring ApplicationContext.
    If a bean, compatible with the declared class exists in the context, it replaces it by the mock.
    If it is not the case, it adds the mock in the context as a bean.

7. When do you want @DataJpaTest for? What does it auto-configure?
    @DataJpaTest and @JdbcTest annotations to test the Spring beans, which talk to relational databases.

    The @DataJpaTest allows you to test the persistence layer components, doesn’t load other Spring beans (@Components, @Controller, @Service, and annotated beans) into ApplicationContext.

    Enable transactions by applying Spring's @Transactional annotation to the test class Enable caching on the test class, defaulting to a NoOp cache instance

    Autoconfigure an embedded test database in place of a real one Create a TestEntityManager bean and add it to the application context

    Regular @Component beans are not loaded into the ApplicationContext.
    @RunWith(SpringRunner.class)
    @DataJpaTest
    public class UserRepositoryTests {

      @Autowired
      private UserRepository userRepository;

      @Test
      public void testFindByEmail() {
        User user = userRepository.findByEmail("admin@gmail.com");
        assertNotNull(user);
      }
    }

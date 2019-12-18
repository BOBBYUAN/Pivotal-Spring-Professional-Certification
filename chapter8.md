Testing
To Do

1. Do you use Spring in a unit test?
    You do not need to, no.

2. What type of tests typically use Spring?
    Integration tests, testing profiles, testing databases

3. How can you create a shared application context in a JUnit integration test?
    By default, once loaded, the configured ApplicationContext is reused for each test. Thus, the setup cost is incurred only once per test suite, and subsequent test execution is much faster. @SpringJUnitConfig(classes=SystemTestConfig.class)

    @ContextConfiguration(classes = ContextConfigurationTwo.class)
    public class TestChildClass extends TestBaseClass {
     @Autowired
     @Qualifier("firstBean")
     protected MyBean mMyBeanOne;
     @Autowired
     @Qualifier("secondBean")
     protected MyBean mMyBeanTwo;
     @Test
     public void gotBeanFromParentTestContextConfigurationTest() {
     System.out.println("Bean instance: " + mMyBeanOne);
     assertNotNull(mMyBeanOne);
     System.out.println("Bean message: " + mMyBeanOne.getBeanMessage());
     assertTrue(mMyBeanOne.getBeanMessage().contains("ContextConfigurationOne"));
     }
     @Test
     public void gotBeanFromChildTestContextConfigurationTest() {
     System.out.println("Bean instance: " + mMyBeanTwo);
     assertNotNull(mMyBeanTwo);
     System.out.println("Bean message: " + mMyBeanTwo.getBeanMessage());
     assertTrue(mMyBeanTwo.getBeanMessage().contains("ContextConfigurationTwo"));
     }
    }

4. When and where do you use @Transactional in testing?
    On the method level where you want to test a service out.
    You must declare Spring’s @Transactional annotation either at the class or the method level for your tests. Annotating a test method with @Transactional causes the test to be run within a transaction that is, by default, automatically rolled back after completion of the test. If a test class is annotated with @Transactional, each test method within that class hierarchy runs within a transaction. Test methods that are not annotated with @Transactional (at the class or method level) are not run within a transaction. Furthermore, tests that are annotated with @Transactional but have the propagation type set to NOT_SUPPORTED are not run within a transaction.

5. How are mock frameworks such as Mockito or EasyMock used?
    Spring Boot includes a @MockBean annotation that can be used to define a Mockito mock for a bean inside your ApplicationContext. You can use the annotation to add new beans or replace a single existing bean definition. The annotation can be used directly on test classes, on fields within your test, or on @Configuration classes and fields. When used on a field, the instance of the created mock is also injected. Mock beans are automatically reset after each test method.

6. How is @ContextConfiguration used?
    Most commonly in integration tests rather than Unit tests
    @ContextConfiguration defines class-level metadata that is used to determine how to load and configure an ApplicationContext for integration tests. Specifically, @ContextConfiguration declares the application context resource locations or the annotated classes used to load the context.
    @ContextConfiguration("/test-config.xml")
    @ContextConfiguration(classes = TestConfig.class)
    @ContextConfiguration(initializers = CustomContextIntializer.class)
    @ContextConfiguration provides support for inheriting resource locations or configuration classes as well as context initializers that are declared by superclasses.

7. How does Spring Boot simplify writing tests?
    It allows easy mocking of objects and dependencies. There are also many built in annotations specifically for testing different aspects of an application. (test configs and setup is done by spring boot)

    Spring Boot provides a number of utilities and annotations to help when testing your application. Test support is provided by two modules: spring-boot-test contains core items, and spring-boot-test-autoconfigure supports auto-configuration for tests. Most developers use the spring-boot-starter-test “Starter”, which imports both Spring Boot test modules as well as JUnit, AssertJ, Hamcrest, and a number of other useful libraries.

    TestPropertyValues is a Spring Boot utility thats assists in adding properties.

    Spring Boot has a starter module called spring-boot-starter-test which adds the following
    test-scoped dependencies that can be useful when writing tests:
    JUnit, Spring Test, Spring Boot Test, AssertJ, Hamcrest, Mockito, JSONassert and JsonPath.
    • Spring Boot provides the @MockBean and @SpyBean annotations that allow for creation of
    Mockito mock and spy beans and adding them to the Spring application context.
    • Spring Boot provide an annotation, @SpringBootTest, which allows for running Spring
    Boot based tests and that provides additional features compared to the Spring TestContext
    framework.
    • Spring Boot provides the @WebMvcTest and the corresponding @WebFluxTest annotation
    that enables creating tests that only tests Spring MVC or WebFlux components without
    loading the entire application context.
    • Provides a mock web environment, or an embedded server if so desired, when testing Spring
    Boot web applications.
    • Spring Boot has a starter module named spring-boot-test-autoconfigure that includes a
    number of annotations that for instance enables selecting which auto-configuration classes
    to load and which not to load when creating the application context for a test, thus avoiding
    to load all auto-configuration classes for a test.
    • Auto-configuration for tests related to several technologies that can be used in Spring Boot
    applications. Some examples are: JPA, JDBC, MongoDB, Neo4J and Redis.

8. What does @SpringBootTest do?
    Integration testing focused - bootstraps entire container

    Used alongside @TestPropertySource to load files for test configs

    It sets up the same config for tests that the app uses. It automatically searches for springBootConfiguration for the SpringBootApplication. The @SpringBootTest annotation can be used as an alternative to the standard spring-test @ContextConfiguration annotation when you need Spring Boot features. The annotation works by creating the ApplicationContext used in your tests through SpringApplication. In addition to @SpringBootTest a number of other annotations are also provided for testing more specific slices of an application. Don’t forget to also add @RunWith(SpringRunner.class) to your test, otherwise the annotations will be ignored. By default, @SpringBootTest will not start a server.

    How does it interact with @SpringBootApplication and @SpringBootConfiguration?
    The search algorithm works up from the package that contains the test until it finds a class annotated with @SpringBootApplication or @SpringBootConfiguration.

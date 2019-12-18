1. What is dependency injection and what are the advantages?
    Dependency injection is the process that establish relationship between different parts of an application.
        Advantages: reduce coupling,
                    increase cohesion
                    increase testability
                    increase reuseabaility
                    increase maintainability
                    No code needs to be written to establish relationships in domain classes. Such code or
                    configuration is separated into XML or Java configuration classes

2. What is an interface and what are the advantages of making use of them in Java?
    Why are they recommended for Spring beans?
    A pattern is template for solving one type of problem in software development. (ex: proxy design pattern)
       An anti-pattern is a commonly used template that attempts to solve a type of problem but turns out
    to be counterproductive and inefficient.

        DI is a pattern

3. Interface: A way of implementing multiple inheritance (polymorphism), interfaces only contain abstract methods and cannot be instantiated. Advantages include providing different implementations at runtime, the ability to inject dependencies, and polymorphism. An interface is a reference type in Java. It is similar to a class. It is a collection of abstract methods. Interfaces are Java's way to implement multiple inheritance, ie: A set of methods you can call without any knowledge of their implementation.

    Why interface is important for Spring Beans:
    * Increased testability
    * Allows for easier switching of Spring bean definiation
    * Allows for use of JDK dynamic proxying mechanism
    * Allows for hiding implementation

4. What is meant by “application-context?
    The application context in a Spring application is a Java object that implements the ApplicationContext interface and is responsible for:
        * Instantiating beans in the application context.
        * Configuring the beans in the application context.
        * Assembling the beans in the application context.
        * Managing the life-cycle of Spring beans.

        It is a BeanFactory(basic IOC) use lazy, application context is (advance iOC) use eager, it's a sub interface that implements BeanFactory

        There can be more than one application context in a single Spring application

5. How are you going to create a new instance of an ApplicationContext?
    ApplicationContext is just an interface so first you have to choose the implementation that best suits your needs.
    In case of XML you just supply an Spring XML Configuration Metadata file; and in case of annotations you will need a configuration class annotated with @Configuration.

6. Can you describe the lifecycle of a Spring Bean in an ApplicationContext?
    1. Bean is instantiated(created) using bean definition(such as in xml)
    2. Spring populates all of the properties using the dependency injection, as specified in the bean definition
    3. If bean implements BeanNameAware interface, then call setBeanName()
    4. If bean implements BeanFactoryAware interface, then call setBeanFactory()
    5. If bean implements BeanPostProcessor interfaceBeanPostProcessor: postProcessBeforeIniialization() is called
    6. Method annotated with @PostConstruct is called
    7. If bean implements Initializating interface, afterPropertiesSet() is called
    8. If bean has
    -method in xml, then Init-method set in xml bean configuration

    Now bean is ready to use!

    9. If bean implements BeanPostProcessor interfaceBeanPostProcessor: postProcessAfterIniialization() is called
    10. Method annotated with @PreDestory is called
    11. If bean implements DispoableBean interface, then destory() is called
    12. If bean has destory-method in xml, then destory-method set in xml bean configuration

7. How are you going to create an ApplicationContext in an integration test?
    TO DO.

8. What is the preferred way to close an application context? Does Spring Boot do this for
you?
    Non Web:
    context.close()
    context.registerShutdownHook();
    Web:
    closing of the Spring application context is taken care of by the ContextLoaderListener, which implements the ServletContextListener interface

    Yes, spring boot do this for us.

9. Can you describe:
    • Dependency injection using Java configuration?
    For DI using Java configuration you have to use a JavaConfig class. It is annotated with @Configuration. Inside that class you declare some methods that return beans (configured instances of certain POJO classes that will be available in Spring context) and these methods should be annotated with @Bean.

    • Dependency injection using annotations (@Autowired)?
    @Component marks the class as a Java Bean which signals Spring to pick it up and pull it into the Application Context so that it can be injected into @Autowired instances.

    The @Autowired annotation can be applied to constructors, methods, parameters and properties of a
    class. The Spring container will attempt to satisfy dependencies by inspecting the contents of the
    application context and inject appropriate references or values

    • Component scanning, Stereotypes?
    Components are scanned at startup based off the specified classpath or marked with @Component
    Stereotypes: An annotation classification for classes.
    @Component: Root stereotype annotation that indicates that a class is a candidate for autodetection.
    @Controller: Indicates that a class is a web controller
    @Service: Indicates that a class is a service
    @Repository: for beans that represent a DAO

    • Scopes for Spring beans? What is the default scope?
    Scopes define how the bean will be used. For example singleton scope will use the same instance of the requested object each time.

    singleton – the default scope for the beans. Will ensure that only one instance of the bean will be created per current IoC container (Spring container). Used for stateless beans.

    prototype – any number of bean instances may be created. Used for stateful beans. For this type of beans configured destruction lifecycle callbacks are not called. User must clean up this type of objects and release the resources that they are holding; usually using custom bean post-processor for that.

    request – a bean instance will be created per each HTTP request. @RequestScope may be used with @Component to assign this scope.

    session – one instance per HTTP Session lifecycle. @SessionScope may be used with @Component to assign this scope.

    websocket – one instance per WebSocket

10. Are beans lazily or eagerly instantiated by default? How do you alter this behavior?
    Beans are eagerly instantiated by default. You can override this by marking the bean @Lazy

11. What is a property source? How would you use @PropertySource?
    A property source in Spring’s environment abstraction represents a source of key-value pairs.

    The @PropertySource annotation can be used to add a property source to the Spring environment.
    The annotation is applied to classes annotated with @Configuration. Example:
    @Configuration
    @PropertySource("classpath:testproperties.properties")
    public class TestConfiguration {

12. What is a BeanFactoryPostProcessor and what is it used for? When is it invoked?
    BeanFactoryPostProcessor is an interface that allows for defining customizing BeanDefinitions for future beans before those beans are created.
    @Component
    public class FactoryProcessor  implements BeanFactoryPostProcessor{
        @Override
        public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
            BeanDefinition definition = configurableListableBeanFactory.getBeanDefinition("foo");
            definition.setBeanClassName(Boo.class);
        }

    • Why would you define a static @Bean method?
    this type of beans require very early initialization and thus the must not be affected by @Autowire, @PostConstruct and @Value annotations from a @Configuration annotated class. The static method declaration will avoid requirement to have the declaring class to be instantiated

    • What is a ProperySourcesPlaceholderConfigurer used for?
    This bean is used for defining the location of the properties file to be used in assigning values to bean properties. But it also allows for searching the required value in the system and/or environment variables. Resolves ${...} placeholders within bean definition property values and @Value annotations

13. What is a BeanPostProcessor and how is it different to a BeanFactoryPostProcessor?
    Both BeanPostProcessor and BeanFactoryPostProcessor are interfaces that allow for definition of
    container extensions. Examples of such container extensions are declarative transaction handling,
    Spring AOP, property placeholders, bean initialization and destruction methods.
    Both BeanPostProcessor and BeanFactoryPostProcessor instances operate only on beans and bean
    definitions respectively that are defined in the same Spring container.

    * BeanFactoryPostProcessor is called before beans are initialized (works on BeanDefinitions) whilst BeanPostProcessor is called on existing beans.

14. What do they do? When are they called?
    • What is an initialization method and how is it declared on a Spring bean?
    @Bean(initMethod="init",destroyMethod="destroy")
    Initialization method is a method that does some initialization work after all the properties of the bean were set by the container. It can be declared using the initMethod of @Bean, using @PostConstruct, or implementing InitializingBean and overriding afterPropertiesSet(discouraged). The order the methods are called is @PostConstruct, then afterPropertiesSet, and then init-method.
    XML:
    @Bean:
    @PostConstruct
    afterPropertiesSet

    • What is a destroy method, how is it declared and when is it called?
    Destroy method is the method that is called when the container containing it is destroyed (closed). The same way as with initialization method, here we have several ways (that mirror the init-method ways) to declare a destroy method:
    XML:
    @Bean:
    @PreDestory
    destory

        • Consider how you enable JSR-250 annotations like @PostConstruct and
        @PreDestroy? When/how will they get called?
        For these annotation to be available you must have the CommonAnnotationBeanPostProcessor registered in context. They are declared with the init or destroy methods and also are called upon instantiation or just before beans are removed from the container

        When creating a Spring application context using an implementation that uses annotation-based
        configuration, for instance AnnotationConfigApplicationContext, a default
        CommonAnnotationBeanPostProcessor is automatically registered in the application context and no
        additional configuration is necessary to enable @PostConstruct and @PreDestroy

        • How else can you define an initialization or destruction method for a Spring bean?
        See above

15. What does component-scanning do?
    Component scanning allows for scanning a certain package or set of packages for candidate beans; which in turn will be added automatically to the ApplicationContext without the developer to be required to declare them explicitly using @Bean in a @Configuration or in an XML configuration metadata file

16. What is the behavior of the annotation @Autowired with regards to field injection, constructor injection and method injection?
    1. The Spring container examines the type of the field or parameter that is to be dependency
    injected
    2. The Spring container searches the application context for a bean which type matches the
    type of the field or parameter
    3. If there are multiple matching bean candidates and one of them is annotated with @Primary,
    then this bean is selected and injected into the field or parameter
    4. If there are multiple matching bean candidates and the field or parameter is annotated with
    the @Qualifier annotation, then the Spring container will attempt to use the information
    from the @Qualifier annotation to select a bean to inject
    5. If there is no other resolution mechanism, such as the @Primary or @Qualifier annotations,
    and there are multiple matching beans, the Spring container will try to resolve the
    appropriate bean by trying to match the bean name to the name of the field or parameter.
    This is the default bean resolution mechanism used when autowiring dependencies
    6. If still no unique match for the field or parameter can be determined, an exception will be
    thrown

    * @Autowired and Field Injection
    Fields of a Spring bean annotated with @Autowired can have any visibility and are injected after
    the bean instance has been created, before any initialization-methods are invoked.

    * @Autowired and Constructor Injection
    If there is only one single constructor with parameters in a Spring bean class, then there is no need
    to annotate this constructor with @Autowired – the Spring container will perform dependency
    injection anyway.

    If there are multiple constructors in a Spring bean class and autowiring is desired, @Autowired may
    be applied to one of the constructors in the class. Only one single constructor may be annotated with
    @Autowired.

    Constructors annotated with @Autowired does not have to be public in order for Spring to be able
    to create a bean instance of the class in question, but can have any visibility.

    If a constructor is annotated with @Autowired, then all the parameters of the constructor are
    required. Individual parameters of such constructors can be declared using the Java 8 Optional
    container object, annotated with the @Nullable annotation or annotated with
    @Autowired(required=false) to indicate that the parameter is not required. Such parameters will be
    set to null, or Optional.EMPTY if the parameter is of the type Optional.

    * @Autowired and Method Injection
    Methods can be annotated with @Autowired. Such methods can be:
        • Regular setter-methods.
        • Methods with arbitrary names.
        • Methods with more than one parameter.
        • Methods with any visibility.
        • Methods that do not have a void return type.

    If a method annotated with @Autowired(required = false) has multiple parameters then this method
    will not be invoked by the Spring container, and thus no dependency injection will take place,
    unless all the dependencies can be resolved in the Spring context.

    If a method annotated with @Autowired, regardless of whether required is true or false, has
    parameters wrapped by the Java 8 Optional, then this method will always be invoked with the
    parameters for which dependencies can be resolved having a value wrapped in an Optional object.
    All parameters for which no dependencies can be resolved will have the value Optional.EMPTY.

17. What do you have to do, if you would like to inject something into a private field? How does
this impact testing?
    From Spring point of view it doesn’t matter whether you use a private or a public field.
    Using @Autowired or @Value
    Using Constructor Parameters

    Testing and Private Fields: TO DO

18. How does the @Qualifier annotation complement the use of @Autowired?
    @Qualifier at Injection Points
    @Qualifier at Bean Definitions

19. What is a proxy object and what are the two different types of proxies Spring can create?
    Caller --> Proxy --> Real object
    JDK Dynamic Proxy(default)
    CGLIB Proxy
    • What are the limitations of these proxies (per type)?
    Limitations of JDK Dynamic Proxies:
    1. Requires the proxied object to implement at least one interface.
    2. Only methods found in the implemented interface(s) will be available in the proxy object
    3. Proxy objects must be referenced using an interface type and cannot be referenced using a
    type of a superclass of the proxied object type

    Limitations of CGLIB Proxies:
    1. Requires the class of the proxied object to be non-final.
    Subclasses cannot be created from final classes.
    2. Requires methods in the proxied object to be non-final.
    Final methods cannot be overridden.
    3. Requires a third-party library

    • What is the power of a proxy object and where are the disadvantages?
    Powers:
    1. Can add behavior to existing beans.
    Examples of such behavior: Transaction management, logging, security
    2. Makes it possible to separate concerns such as logging, security etc from business logic.

    Disadvantages:
    1. If multiple layers of proxy objects are used, developers may need to take into account the
    order in which the proxies are applied
    2. It may not be obvious where a method invocation is handled when proxy objects are used.
    A proxy object may chose not to invoke the proxied object.
    3. Can only throw checked exception

20. What does the @Bean annotation do?
    @Bean annotation is used on methods that will return beans of the specified return type. @Bean is usually used in configuration classes that don’t use components scan; but may be used as part of a component – that is a @Bean lite mode.

21. What is the default bean id if you only use @Bean? How can you override this?
    The default bean name, also called bean id, is the name of the @Bean
    annotated method. This default id can be overridden using the name, or its alias value, attribute of
    the @Bean annotation.

22. Why are you not allowed to annotate a final class with @Configuration?
    The Spring container will create a subclass of each class annotated with @Configuration when
    creating an application context using CGLIB. Final classes cannot be subclassed, thus classes
    annotated with @Configuration cannot be declared as final.

    • How do @Configuration annotated classes support singleton beans?
    Singleton beans are supported by the Spring container by subclassing classes annotated with
    @Configuration and overriding the @Bean annotated methods in the class. Invocations to the
    @Bean annotated methods are intercepted and, if a bean is a singleton bean and no instance of the
    singleton bean exists, the call is allowed to continue to the @Bean annotated method, in order to
    create an instance of the bean. If an instance of the singleton bean already exists, the existing
    instance is returned (and the call is not allowed to continue to the @Bean annotated method).

    • Why can’t @Bean methods be final either?
    As earlier the Spring container subclass classes @Configuration classes and overrides the methods
    annotated with the @Bean annotation, in order to intercept requests for the beans. If the bean is a
    singleton bean, subsequent requests for the bean will not yield new instances, but the existing
    instance of the bean.

23. How do you configure profiles? What are possible use cases where they might be useful?
    Define active beans for profiles
    can used on @Configuration, @Component, @Bean
    ex: @Profile({"dev", "qa"})

    Then activating profiles in following ways:
    1. Programmatic registration of active profiles when the Spring application context is created
    final AnnotationConfigApplicationContext theApplicationContext =
     new AnnotationConfigApplicationContext();
    theApplicationContext.getEnvironment().setActiveProfiles("dev1", "dev2");
    theApplicationContext.scan("se.ivankrizsan.spring");
    theApplicationContext.refresh();

    2. Using the spring.profiles.active property
    java -Dspring.profiles.active=dev1,dev2 -jar myApp.jar

    3. In tests, the @ActiveProfiles annotation may be applied at class level to the test class
       specifying which the profile(s) that are to be activated when the tests in the class are run

    In integration tests: Using @ActiveProfiles

24. Can you use @Bean together with @Profile?
    Yes

25. Can you use @Component together with @Profile?
    Yes

26. How many profiles can you have?
    There does not seem to be any limitation concerning how many profiles that can be used in a Spring
    application. The Spring framework (in the class ActiveProfilesUtils) use an integer to iterate over an
    array of active profiles, which implies a maximum number of 2^32 – 1 profiles.

27. How do you inject scalar/literal values into Spring beans?
    Scalar/literal values can be injected into Spring beans using the @Value annotation. Such values can
    originate from environment variables, property files, Spring beans etc

28. What is @Value used for?
    This annotation is used for assigning some value to a bean field. It can be used at different levels:
    constructor
    setter
    field
    It is not accessible in a BeanFactoryPostProcessor and BeanPostProcessor by the time it is applied using a BeanPostProcessor.

29. What is Spring Expression Language (SpEL for short)?
    The Spring Expression Language (SpEL for short) is a powerful expression language that supports querying and manipulating an object graph at runtime.

30. What is the Environment abstraction in Spring?
    The Environment is a part of the application container. The Environment contains profiles and
    properties, two important parts of the application environment.

31. Where can properties in the environment come from – there are many sources for properties – check the documentation if not sure. Spring Boot adds even more

Property Source                                             Originating Environment
JVM system properties                                       StandardEnvironment
System environment variables                                StandardEnvironment
Servlet configuration properties (ServletConfig)            StandardServletEnvironment
Servlet context parameters (ServletContext)                 StandardServletEnvironment
JNDI properties                                             StandardServletEnvironment
Command line properties                                     n/a
Application configuration (properties file)                 n/a
Server ports                                                n/a
Management server                                           n/a

32. What can you reference using SpEL?
    1. Static methods and static properties/fields
    2. Properties and methods in Spring beans
    3. Properties and methods in Java objects with references stored in SpEL variables
    4. (JVM) System properties
    5. System environment properties
    6. Spring application environment

33. What is the difference between $ and # in @Value expressions?
    For property placeholders we use $ whilst for SpEL we use #

    Expressions starting with $.
    Such expressions reference a property name in the application’s environment. These
    expressions are evaluated by the PropertySourcesPlaceholderConfigurer Spring bean prior
    to bean creation and can only be used in @Value annnotations.

    Expressions starting with #.
    Spring Expression Language expressions parsed by a SpEL expression parser and evaluated
    by a SpEL expression instance.


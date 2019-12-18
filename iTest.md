1. RestController used on server side
    RestTemplate used on client side

2. ViewResolver and MultipartResolver are registered as beans, not web.xml
    In web.xml, we can declare the root application context and DispatchServlet

3. JdbcTemplate catches Jdbc exceptions and translates them into Spring's exception hierarhy ()

4. private field @Autowired @Inject @Value

5. The Spring expression language is a seperate language from OGNL

6. Classes annotated with @Configuration are subclassed at startup-time with CGLIB

7. Spring boot has no XML-configuration  / Spring boot doesn't do code generation

8. @EnableAspectJAutoProxy

9. name is "spring-boot-starter-test" not testing

10. You can call the rollback method of PlatformTransactionManager to issue a rollback

11. A RequestMapping is not required on the class level

12. spring-boot-starter-web

14. What is the security filter chain?

15. 200, 201 Created, 202 Accepted, 204 No Content

16. The class constructor method is called efore doing dependency injection

17. @Profile({!production}) no exclude

18. "intercept-url", requires-channel: http,https,any, NO tls/http

19. no need to mark @Required if all @Autowired constructors

20. we do not have to write a implementation of the repository interface. Spring Data Jpa creates an implementation on the fly when we run the application

21. .setDriverClassName
    .setUrl
    .setUserName
    .setPassword

22. DispatcherServlet is an actual Servlet and can be delcared in the web.xml. It is instatiated by the Servlet container

23. @Bean method can be used inside both @Configutation and @Component (not recommoneded)

24. Spring AOP uses JDK dynamic proxies for AOP proxies by default

25. CGLIB classes are under the org.springframework.cglib package and included in the spring-core jar

26. Controller method return

27. properties of @Transaction: rollbackForClassName, noRollbackFor, noRollbackForClassname

28. http forbidden: 403

29. AnnonationConfigWebApplicationContext can be used inside the web.xml when configuring a web application context

30. @Bean does NOT have id attribute , @Bean does NOT have scope attribute

31. Initializaing Bean method they have

32. We can override the default scope with the @Scope annotation

33. Spring secures web requests using standard filters, not AOP

34. http internal server error 500

35. @EnableTransactionManagement enables @Transaction

36. Spring AOP performs weaving at run time

37. Spring has mock objects on Environment, JNDI and Servlet API

38 web: session, request, websocket

39. @RequestBody is annotated on parameters only (not on methods)

40. @ContextConfiguration is commonly used in integeration test

41. @Controller another way: extens AbstractController

42. authentication allows access to the current Authentication object stored in the security context /
    authorize is used to determine whether its context should be evulated or not

43. only @Profile, no @BeanProfile

44. Checked exceptino, by dafult, does not cause a roll back

45. @PathVariable:
    @RequestMapping(value="/users/{userId}")
    public String viewUser(@PathVariable("userid") String personId)
    public String viewUser(@PathVairable String userid)

46. @ConfigurationProperties and @JsonComponent are Spring boot applications

47. The default roll back pollcy is true, no @Default annotation

48. @Value can be annotated on methods parameters

49. @Import @DependsOn @Bean Java-based configuration

50. NEED to flush the database ourself

51. @RunWith is a JUnit annonation

52. TestPropertyValue, no TestPropertyUtil
    OutputCapture, no SystemCapture

53. The @PostConstruct, @PreDestroy and @Resource   annotations are defined in  the JSR-250
"Common Annotations"

54. Auto-injection is possible with class, not interface

55. Destroy methods of prototype beans are never called

56. Mocking or stubbing is more frequent in unit tests than in integration tests

57. The pattern (*) matches a method taking one parameter of any type

58. The (..) param pattern indicates 0, 1 or many parameters

59. A JdbcTemplate requires a DataSource as input parameters

60. Rollback for RuntimeException is the default rollback policy

61. parameter types of a controller types

62. There is a type of Spring proxy that can be trigged only if the proxied method throws exception
    There is a type of Spring proxy that can replace the object being returned by the moethod

63. By default, unchedked exception and java.lang.Error would cause a rollback

64. @PostFiler @PreFilter @PostAuthorize @PreAuthorize allow SPEL language

65. Map collection can be autowired only if the key value is String

66. Java interface makes it easier to mock test

67. Spring provides three JDBC driver:
    1. DriverManagerDataSource: a new connection each time, not pooled, not good for multi requests
    2. SimpleDriverDataSource: similiar as DriverManagerDataSource
    3. SingleConnectionDataSource: not cloes after each time, not multi-threading capable

68. @Profile can be applies on class and method level

69. idempotent: PUT,GET,DELETE, not POST

70. DataAccessException is supported by Spring, not java

71. "required" attribute can be applied on @RequestParm, NO "mandatory"

72. The first parameter of the @Around (only) advice method must be of type ProceddingJoinPoint

73. proceed(), @Around, 0 or more times

74. The core view resolver provided by Spring is the InternalResourceViewResolver; it is the default view resolver

75. Queries annotated to the query method take precedence over queries defined using @NamedQuery or named queries declared in orm.xml

    ** @Query > orm.xml && @NamedQuery

    ** @ nativeQuery flag, NOT native

    // native queries The @Query annotation allows for running native queries by setting the nativeQuery flag to true (nativeQuery = true)

    // Spring Data JPA does not currently support dynamic sorting for native queries

76. For a Spring Data repository a JDK dynamic proxy is created which intercepts all calls to the repository

77. Static @Bean,
    1. Static @Bean methods are called without creating their containing configuration class as an instance
    2. will get initialized early in the container lifecycle and should avoid triggering other parts of the configuration at that point
    3. Calls to static @Bean methods never get intercepted by the container, because CGLIB subclassing can override only non-static methods
    4. In static @bean class, @Autowired and @Value do not work on the class itself, since it is being created as a bean instance too early

78. A "singleton" is intialized even before clients request it

79. incoming request mapped: HandlerMapping interface

80. By default, all methods of the class MyRepositoryTests will be atuomcatically ROLL BACK after completion of the test

81. propagation NOT_SUPPORTED will not be run with a transaction

82. Spring boot environment pull properties from JVM System properties, Operating System Environment variables, Command line arguments and application.yml

83. Spring secure web requests using standart filters, NOT AOP

84. JDBCTemplate simplfies JDBC data access code, NOT Hibernate

85. For AOP, Spring reuses AspectJ's annonation

86. @PersistentUnit on EntityManagerFactory, which to create EntityManager

87. A salt is an additional string of known data for each user which is combined with the password before calculating the hash

88. @PersistentContext , EntityManager entitymanager, update delete data something like that

89. @PostConstruct and afterPropertySet() are called after all the bean's properties been set

90. init-method can be only used with void no argument methods

91. intercept-url, filters

92. instances of the TranslactionTemplate class are threadsafe /
    The transaction name can be explictly set only programmatically

93. CGLIB are included in spring-core /
    proxy-target-class = "true" forces CGLIB

94. TestPropertyValue is spring boot utility, NOT TestPropertyUtil

95. OutputCapture is JUnit Rule, not SystemCapture

96. Spring transaction uses TransactionTemplate class in its AOP proxies in applying transactional advice

97. @EnableAspectJAutoProxy, NOT @EnableAspect

98. To work with @AspectJ, the aspectweaver.jar need to in the claspath

99. URI templates are automatically encoded

100. Spring AOP supports method execution join points, and does not support field interception

101. JDBCTemplate are thread safe

102. Spring MVC provides additional scopes: HTTP request, HTTP session /
    In Spring MVC, any object can be a command or form backing object

103. BeanNameUrlHandlerMapping included in Spring core, default

104. if a bean is session scope, it would be only 1 object in web aware context, not in spring IOC container

105. For JDK proxy, only public interfaces / CGLIB public and protected, not private
    proxy based, calls within not intercepted

106. TransactionTemplate in programmatic transaction demarcation and transaction exception handling /
    excutes code within a transaction using the TransactionCallback interface

107. @ResponseStatus, NO @HTTPResponseStatus

108. @RequestParam, required is true by default

109. ContextLoaderListener used for load root application context in web application

110. @Afterthrowing attribute throwing, further filters matching in those methods associated exception

111. intercept-url. method: DELETE,POST .... combination with pattern

112. <password-encode> NOT <encode-password>

113. DataSourceTransactionManager is the PlatformTransactionManager implementation of single datasource connection

114. PropertySourcesPlaceholderConfiguer doesn't resolve @Autowired,

115. ApplicationConfig is also a bean

116. @Import({A.class,B.class})

117. same bean more than once, the last bean replaced the previous one

118. @Order(1)config2  @Order(2)config1, config1 is loaded after config2

119. @Import annonation is used to import only one or more @Configuration classes

120. .web.context.ContextLoaderlistener

121. Spring's declarative transaction uses TransactionInterceptor in its AOP proxies and applying transaction advice




--- First  Time Conclusion
58 % of 76 %

To work with on: spring actuator
                 spring security
                 spring jpa
                 spring container
                 spring boot

Actuator: healthIndictor, endpoint, default
security: distrbuted

@SpringBootConfiguration

same bean load twice

1. What is the difference between checked and unchecked exceptions?
    Checked exceptions are exceptions that the Java compiler requires to be declared in the signature of
    methods that throw this type of exceptions. If a method calls another method that declares one or
    more checked exceptions in its method signature, the calling method must either catch these
    exceptions or declare the exceptions in its method signature.
    The class java.lang.Exception and its subclasses, except for java.lang.RuntimeException and any
    subclass of RuntimeException, are checked exceptions.
    Unchecked exceptions are exceptions that the Java compiler does not require to be declared in the
    signature of methods or to be caught in methods invoking other methods that may throw unchecked
    exceptions.

    • Why does Spring prefer unchecked exceptions?
    Checked exceptions forces developers to either implement error handling in the form of try-catch
    blocks or to declare exceptions thrown by underlying methods in the method signature.
    This can result in cluttered code and/or unnecessary coupling to the underlying methods.
    Unchecked exceptions gives developers the freedom of choice as to decide where to implement
    error handling and removes any coupling related to exceptions.

    • What is the data access exception hierarchy?
    The data access exception hierarchy is the DataAccessException class and all of its subclasses in the
    Spring Framework. All the exceptions in this exception hierarchy are unchecked.
    The purpose of the data access exception hierarchy is isolate application developers from the
    particulars of JDBC data access APIs, for instance database drivers from different vendors. This in
    turn enables easier switching between different JDBC data access APIs.

2. How do you configure a DataSource in Spring? Which bean is very useful for development/test databases?
    DataSource in a standalone application:
    @Bean
     public DataSource dataSource() {
     final BasicDataSource theDataSource = new BasicDataSource();
     theDataSource.setDriverClassName("org.hsqldb.jdbcDriver");
     theDataSource.setUrl("jdbc:hsqldb:hsql://localhost:1234/mydatabase");
     theDataSource.setUsername("ivan");
     theDataSource.setPassword("secret");
     return theDataSource;
     }

    Spring boot:
    spring.datasource.url= jdbc:hsqldb:hsql://localhost:1234/mydatabase
    spring.datasource.username=ivan
    spring.datasource.password=secret

    DataSource in an application deployed to a server:
    @Bean
     public DataSource dataSource() {
     final JndiDataSourceLookup theDataSourceLookup = new JndiDataSourceLookup();
     final DataSource theDataSource =
     theDataSourceLookup.getDataSource("java:comp/env/jdbc/MyDatabase");
     return theDataSource;
     }

     Spring boot:
     spring.datasource.jndi-name=java:comp/env/jdbc/MyDatabase

     * DriverManagerDataSource: Useful for test or standalone environments outside of a J2EE container, either as a DataSource bean in a corresponding ApplicationContext or in conjunction with a simple JNDI environment.

3. What is the Template design pattern and what is the JDBC template?
    The template (method) design pattern is a design pattern in which an algorithm is defined and the
    steps of the algorithm are refactored to methods with protected visibility. The class defining the
    algorithm may provide abstract methods for the different steps of the algorithm, letting subclasses
    define all steps. Alternatively the class defining the algorithm may define default implementations
    of the different steps of the algorithm, allowing subclasses to customize only selected methods as
    desired.

    The Spring JdbcTemplate class is a Spring class that simplifies the use of JDBC by implementing
    common workflows for querying, updating, statement execution etc.
    JdbcTemplate simplifies the use of JDBC and helps to avoid common errors. It executes core JDBC workflow, leaving application code to provide SQL and extract results. This class executes SQL queries or updates, initiating iteration over ResultSets and catching JDBC exceptions and translating them to the generic, more informative exception hierarchy defined in the org.springframework.dao package.

4. What is a callback? What are the three JdbcTemplate callback interfaces that can be used with queries? What is each used for?
    A callback is code or reference to a piece of code that is passed as an argument to a method that, at
    some point during the execution of the methods, will call the code passed as an argument.
    In Java a callback can be a reference to a Java object that implements a certain interface or, starting
    with Java 8, a lambda expression.

    ResultSetExtractor
    Allows for processing of an entire result set, possibly consisting multiple rows of data, at
    once. Suitable when more than a single row of data is needed to create a Java object holding
    query result data. Result set extractors are typically stateless. Note that the extractData
    method in this interface returns a Java object.

    RowCallbackHandler
    Allows for processing rows in a result set one by one typically accumulating some type of
    result. Row callback handlers are typically stateful, storing the accumulated result in an
    instance variable. Note that the processRow method in this interface has a void return type.

    RowMapper
    Allows for processing rows in a result set one by one and creating a Java object for each
    row. Row mappers are typically stateless. Note that the mapRow method in this interface
    returns a Java object.

5. Can you execute a plain SQL statement with the JDBC template?
    Yes.

6. When does the JDBC template acquire (and release) a connection, for every method called or once per template? Why?
    JdbcTemplate acquire and release a database connection for every method called. That is, a
    connection is acquired immediately before executing the operation at hand and released
    immediately after the operation has completed, be it successfully or with an exception thrown.

    The reason for this is to avoid holding on to resources (database connections) longer than necessary
    and creating as few database connections as possible, since creating connections can be a
    potentially expensive operation. When database connection pooling is used connections are returned
    to the pool for others to use.

7. How does the JdbcTemplate support generic queries? How does it return objects and lists/maps of objects?
    query()
    queryForObject() – if you are expecting only one object
    queryForMap() – will return a map containing each column value as key(column name)/value(value itself) pairs.
    queryForList() – a list of above if you’re expecting more results

    JdbcTemplate contains seven different queryForList methods and three different queryForMap methods.

8. What is a transaction? What is the difference between a local and a global transaction?
    A transaction is an operation that consists of a number of tasks that takes place as a single unit –
    either all tasks are performed or no tasks are performed. If a task that is part of a transaction do not
    complete successfully, the other tasks in the transaction will either not be performed or, for tasks
    that have already been performed, be reverted.

    Global transactions allow for transactions to span multiple transactional resources. As an example
    consider a global transaction that spans a database update operation and the posting of a message to
    the queue of a message broker. If the database operation succeeds but the posting to the queue fails
    then the database operation will be rolled back (undone). Similarly if posting to the queue succeeds
    but the database operation fails, the message posted to the queue will be rolled back and will thus
    not appear on the queue. Not until both operations succeed will the database update come into effect
    and a message be made available for consumption on the queue.
    Note that a transaction that is to span operations on two different databases needs to be a global
    transaction.

    Local transactions are transactions associated with one single resource, such as one single database
    or a queue of a message broker, but not both in one and the same transaction.

9. Is a transaction a cross cutting concern? How is it implemented by Spring?
    As already mentioned in the section on Aspect Oriented Programming, transaction management is a
    cross-cutting concern. In the Spring framework declarative transaction management is implemented
    using Spring AOP.

10. How are you going to define a transaction in Spring?
    • Declare a PlatformTransactionManager bean.
    Choose a class implementing this interface that supplies transaction management for the
    transactional resource(s) that are to be used. Some examples are JmsTransactionManager
    (for a single JMS connection factory), JpaTransactionManager (for a single JPA entity
    manager factory).

    • If using annotation-driven transaction management, then apply the
    @EnableTransactionManagement annotation to exactly one @Configuration class in the
    application.

    • Declare transaction boundaries in the application code.
    This can be accomplished using one or more of the following:
    @Transactional annotation
    Spring XML configuration (not in the scope of this book)
    Programmatic transaction management

    • What does @Transactional do?
    The @Transactional annotation is used for declarative transaction management and can be applied
    to methods and classes. This annotation is used to specify the transaction attributes for the method
    which it annotates or, if applied on class level, all the methods in the class.

    • What is the PlatformTransactionManager?
    An interface that defines the transaction strategy thr
    ough different implementations that match requirements specific to the project they are used in.

11. Is the JDBC template able to participate in an existing transaction?
    Yes, the JdbcTemplate is able to participate in existing transactions both when declarative and
    programmatic transaction management is used. This is accomplished by wrapping the DataSource
    using a TransactionAwareDataSourceProxy.

12. What is a transaction isolation level?
    Transaction isolation in database systems determine how the changes within a transaction are
    visible to other users and systems accessing the database prior to the transaction being committed. A
    higher isolation level reduces, or even eliminates, the chance for the problems described below that
    may appear when the database is updated and accessed concurrently. The drawback of higher
    isolation levels is a reduction of the ability of multiple users and systems concurrently accessing the
    database as well as increased use of system resources on the database server.

    How many do we have and how are they ordered?
    There are four isolation levels, here listed in descending order:
    • Serializable
    • Repeatable reads
    • Read committed
    • Read uncommitted

13. What is @EnableTransactionManagement for?
    The @EnableTransactionManagement annotation is to annotate exactly one configuration class in
    an application in order to enable annotation-driven transaction management using the
    @Transactional annotation.

14. What does transaction propagation mean?
    Defines how transactions relate to each other:

    PROPAGATION_REQUIRED = Uses same Transaction object as Method 1 for Method 2 if exists. Create a new transaction or reuse one if available

    PROPAGATION_MANDATORY = method must run within a transaction. If no existing transaction is in progress, an exception will be thrown

    PROPAGATION_REQUIRES_NEW = Code will always run in a new transaction. Suspend current transaction if one exists.

    PROPAGATION_NOT_SUPPORTED = T1 / M1 , M2 starts / cannot run inside T1

15. What happens if one @Transactional annotated method is calling another @Transactional annotated method on the same object instance?
    Since transaction-management is implemented using AOP @Transactional annotation will have no effect on the method being called as no proxy is created. The same behavior is characteristic for AOP aspects.

    If you call method2() from method1() within the same class, the @Transactional annotation of the second method will not have any effect because it is not called through proxy, but directly. Methods are enhanced with transactional behavior only if called through proxy (autowired bean, or some instance injected in any other way).

    But generally speaking, if method1() and method2() were in different classes, and both were annotated with @Transactional (so using REQUIRED propagation), then they would share the same transaction started in method1()

16. Where can the @Transactional annotation be used? What is a typical usage if you put it
at class level?
    At the class and method levels. Class level automatically applies to all methods.

    Transactional Annotations should be placed around all operations that are inseparable.
    Typically, transactions belong on the Service layer. It's the one that knows about units of work and use cases. It's the right answer if you have several DAOs injected into a Service that need to work together in a single transaction.

17. What does declarative transaction management mean?
    Declarative transaction management means that the methods which need to be executed in the
    context of a transaction and the transaction properties for these methods are declared, as opposed to
    implemented. This is accomplished using annotations or Spring XML configuration.

18. What is the default rollback policy? How can you override it?
    The default rollback policy of Spring transaction management is that automatic rollback only takes
    place in the case of an unchecked exception being thrown.

    The types of exceptions that are to cause a rollback can be configured using the rollbackFor
    element of the @Transactional annotation. In addition, the types of exceptions that not are to cause
    rollbacks can also be configured using the noRollbackFor element.

19. What is the default rollback policy in a JUnit test, when you use the @RunWith(SpringJUnit4ClassRunner.class) in JUnit 4 or @ExtendWith(SpringExtension.class) in JUnit 5, and annotate your @Test annotated method with @Transactional?

    TO DO

20. Why is the term "unit of work" so important and why does JDBC AutoCommit violate this
pattern?
    The unit of work describes the atomicity characteristic of transactions. As earlier, it is either “all or
    nothing”, that is all operations that are transactional and take place in the scope of a transaction
    must either all be successfully performed or not be performed at all.

    JDBC AutoCommit will cause each individual SQL statement as to be executed in its own
    transaction and the transaction committed when each statement is completed. This makes it
    impossible to perform operations that consist of multiple SQL statements as a unit of work.
    JDBC AutoCommit can be disabled by calling the setAutoCommit method with the value false on a
    JDBC connection.

21. What do you need to do in Spring if you would like to work with JPA?
    Declare the appropriate dependencies.
    In a Maven application, this is accomplished by creating dependencies in the pom.xml file.
    The dependencies in question are typically the ORM framework dependency, a database
    driver dependency and a transaction manager dependency.

    • Implement entity classes with mapping metadata in the form of annotations.
    As a minimum, entity classes need to be annotated with the @Entity annotation on class
    level and the @Id annotation annotating the field or property that is to be the primary key of
    the entity.
    It is possible to separate mapping metadata from the implementation of entity classes by
    using an orm.xml file, but that is outside of the scope of this book.

    • Define an EntityManagerFactory bean.
    The JPA support in the Spring framework offer three alternatives when creating an
    EntityManagerFactoryBean:
    1. LocalEntityManagerFactoryBean
    Use this option, which is the simplest option, in applications that only use JPA for
    persistence and in integration tests.
    2. Obtain an EntityManagerFactory using JNDI
    Use this option if the application is run in a JavaEE server.
    3. LocalContainerEntityManagerFactoryBean
    Gives the application full JPA capabilities.

    • Define a DataSource bean.

    • Define a TransactionManager bean.
    Typically using the JpaTransactionManager class from the Spring Framework.

    • Implement repositories.
    A better alternative is using Spring Data JPA, with which you only need to create repository
    interfaces.

22. Are you able to participate in a given transaction in Spring while working with JPA?
    Yes.

23. Which PlatformTransactionManager(s) can you use with JPA?
    You can only use JpaTransactionManager. The Transaction Manager abstraction we are talking about here is Spring's PlatformTransactionManager interface, and JPATransactionManager is the only implementation of that interface that understands JPA.

24. What do you have to configure to use JPA with Spring? How does Spring Boot make this easier?
    To use JPA in a Spring project, an EntityManager needs to be set up.
    LocalEntityManagerFactoryBean or the more flexible LocalContainerEntityManagerFactoryBean.
    (Above)

    Spring Boot provides a starter module that:
    • Provides a default set of dependencies needed for using JPA in a Spring application.
    • Provides all the Spring beans needed to use JPA.
    These beans can be easily customized by declaring bean(s) with the same name(s) in the
    application, as is standard in Spring applications.
    • Provides a number of default properties related to persistence and JPA.
    These properties can be easily customized by declaring one or more properties in the
    application properties-file supplying new values.

    Spring Boot does this automatically when including spring-boot-starter-data-jpa

    Spring Boot configures Hibernate as the default JPA provider, so it’s no longer necessary to define the entityManagerFactory bean unless we want to customize it.

    Spring Boot can also auto-configure the dataSource bean, depending on the database used. In the case of an in-memory database of type H2, HSQLDB and Apache Derby, Boot automatically configures the DataSource if the corresponding database dependency is present on the classpath.

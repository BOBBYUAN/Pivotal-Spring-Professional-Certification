1. What is Spring Boot?
    Spring Boot is a project gathering a number of modules under a common umbrella. Some of the
    more central modules are:
    • spring-boot-dependencies
    Contains versions of dependencies, Maven plug-ins etc used by Spring Boot, including
    dependencies used by starter-modules. Contains managed dependencies for dependencies.
    • spring-boot-starter-parent
    Parent pom.xml file providing dependency and plug-in management for Spring Boot
    applications using Maven.
    • spring-boot-starters
    Parent for all the Spring Boot starter-modules.
    • spring-boot-autoconfigure
    Contains the autoconfigure modules for the starters in Spring Boot.
    • spring-boot-actuator
    Allows for monitoring and managing of applications created with Spring Boot.
    • spring-boot-tools
    Tools used in conjunction with Spring Boot such as the Spring Boot Maven and Gradle
    plug-ins etc.
    • spring-boot-devtools
    Developer tools that can be used when developing Spring Boot applications.

    Two of the most important parts of Spring Boot are the starter and the autoconfiguration modules.

2. What are the advantages of using Spring Boot?
    Easy Configuration Integrated web containers that allow for easy testing Starters – sets of dependencies that help setting up a “typical” application Decrease amount of boilerplate code

3. Why is it “opinionated”?
    I feel that Spring Boot “has an opinion” on how development of an application is to be done, for
    instance concerning the technology-related modules (starters and autoconfiguration), organization
    of properties, configuration of modules etc.
    The true accomplishment of Spring Boot is that it is opinionated but do allow for developers to
    customize their projects to the extent desired without becoming an obstacle.

4. What things affect what Spring Boot sets up?
    Presence of dependencies in the POM as well as the @EnableAutoConfiguration / @SpringBoot annotations

5. What is a Spring Boot starter POM? Why is it useful?
    Starter POMs are a set of convenient dependency descriptors that you can include in your application. You get a one-stop-shop for all the Spring and related technology that you need, without having to hunt through sample code and copy paste loads of dependency descriptors.

6. Spring Boot supports both properties and YML files. Would you recognize and understand
them if you saw them?
    A Java properties file consists of one or more rows with a key-value pair defined on each row
    (disregarding empty lines and comments). Example:
    # This is a comment.
    se.ivan.username=ivan
    se.ivan.password=secret
    city=Gothenburg

    YML files are files containing properties in the YAML format. YAML stands for YAML Ain’t
    Markup Language. The above properties would look like this if defined in an YML file:
    # This is a comment.
    se:
     ivan:
     username: ivan
     password: secret
    city: Gothenburg

7. Can you control logging with Spring Boot? How?
    Several aspects of logging can be controlled in different ways in Spring Boot applications.

    Controlling Log Levels:
    logging.level.root=WARN
    logging.level.se.ivankrizsan.spring=DEBUG
    The above rows show how log levels can be configured in the application.properties file. The first
    line sets the log level of the root logger to WARN and the second line sets the log level of the
    se.ivankrizsan.spring logger to DEBUG.

    Customizing the Log Pattern:

    Color-coding of Log Levels:
    logging.pattern.console=%clr(%d{yyyy-MM-dd HH:mm:ss}){yellow}

    Choosing a Logging System

    Logging System Specific Configuration

    The various logging systems can be activated by including the appropriate libraries on the classpath and can be further customized by providing a suitable configuration file in the root of the classpath or in a location specified by the following Spring Environment property: logging.config.

    You can force Spring Boot to use a particular logging system by using the org.springframework.boot.logging.LoggingSystem system property. The value should be the fully qualified class name of a LoggingSystem implementation. You can also disable Spring Boot’s logging configuration entirely by using a value of none.

    Since logging is initialized before the ApplicationContext is created, it is not possible to control logging from @PropertySources in Spring @Configuration files. The only way to change the logging system or disable it entirely is via System properties.

8. Where does Spring Boot look for property file by default?
    The default properties of a Spring Boot application are stores in the application’s JAR in a file
    named “application.properties”. When developing, this file is found in the src/main/resources
    directory.
    Individual property values defined in this application.properties file can be customized using for
    example command-line arguments when starting the application. The default properties can be
    overridden in their entirety using an external application.properties file or a YAML equivalent.

9. How do you define profile specific property files?
    In addition to application.properties files, profile-specific properties can also be defined by using the following naming convention: application-{profile}.properties. The Environment has a set of default profiles (by default, [default]) that are used if no active profiles are set. In other words, if no profiles are explicitly activated, then properties from application-default.properties are loaded.

    Profile-specific properties are loaded from the same locations as standard application.properties, with profile-specific files always overriding the non-specific ones, whether or not the profile-specific files are inside or outside your packaged jar.

    If several profiles are specified, a last-wins strategy applies. For example, profiles specified by the spring.profiles.active property are added after those configured through the SpringApplication API and therefore take precedence.

10. How do you access the properties defined in the property files?
    Spring provides the @Value annotation to bind any property value to a bean property.

    jdbc.driver=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/test
    jdbc.username=root jdbc.password=secret
    @Configuration
    public class AppConfig {

        @Value("${jdbc.driver}")
        private String driver;

        @Value("${jdbc.url}")
        private String url;

        @Value("${jdbc.username}")
        private String username;

        @Value("${jdbc.password}")
        private String password;
    }

    Bind a set of properties to a bean's properties automatically in a type-safe manner.

    @ConfigurationProperties(prefix="jdbc") to automatically bind the properties that start with jdbc.*

    @Component
    @ConfigurationProperties(prefix="jdbc")
    public class DataSourceConfig {

      private String driver;
      private String url;
      private String username;
      private String password;

    //setters and getters
    }

11. What properties do you have to define in order to configure external MySQL?
    1. dependencies: Spring boot,and msql
    2. config properties
    In your application.properties file, add:

    spring.jpa.hibernate.ddl-auto=none
    spring.datasource.url=jdbc:mysql://<dbhost>:<dbport>/<db>
    spring.datasource.username=<username>
    spring.datasource.password=<password>
    spring.datasource.driver-class-name=com.mysql.jdbc.Driver

12. How do you configure default schema and initial data?
    Spring Boot uses the spring.datasource.initialize property value, which is true by default, to determine whether to initialize the database. If it's set to true, Spring Boot will use the schema.sql and data.sql files in the root classpath to initialize the database.

    schema.sql to initialize the schema (tables, views, etc.)
    and a data.sql to insert data into the tables.
    Spring Boot will load the schema-${platform}.sql and data-${platform}.sql files if they are available in the root classpath to do database initialization. The value for is read from the spring.datasource.platform property. This allows you to switch to database-specific scripts if necessary.

13. What is a fat far? How is it different from the original jar?
    Executable jars, known as “fat jars”, are archives containing your compiled classes along with all of the jar dependencies that your code needs to run.

    In your project target directory, you should see myproject-0.0.1-SNAPSHOT.jar. This the far Jar. Inside of it, you would see myproject-0.0.1-SNAPSHOT.jar.original. This is the original jar file that Maven created before it was repackaged by Spring Boot.

    Spring Boot Executable jars VS uber jars

    An uber jar packages all the classes from all the application’s dependencies into one single archive. The problem with this approach is that

    it becomes hard to see which libraries are in your application.
    It can also be problematic if the same filename is used (but with different content) in multiple jars.
    Spring Boot lets you actually nest jars directly.

    you can run your application as you would any other
    You do not need any special IDE plugins or extensions.
    Executable jars can be used for production deployment. As they are self-contained, they are also ideally suited for cloud-based deployment.
    Spring Boot’s executable jars are ready-made for most popular cloud PaaS (Platform-as-a-Service) providers. These providers tend to require that you “bring your own container”. They manage application processes (not Java applications specifically), so they need an intermediary layer that adapts your application to the cloud’s notion of a running process.
    To create an executable jar, we need to have the spring-boot-maven-plugin to our pom.xml.
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
    Running as a Packaged Application

    1. java -jar
    $ java -jar target/myapplication-0.0.1-SNAPSHOT.jar
    2. Maven Plugin
    $ mvn spring-boot:run
    3. Gradle Plugin
    $ gradle bootRun

14. What is the difference between an embedded container and a WAR?
    An embedded container is packaged in the application JAR-file and will contain only one single
    application. A WAR-file will need to be deployed to a web container, such as Tomcat, before it can
    be used. The web container to which the WAR-file is deployed may contain other applications.

    WAR's allow you to use JSP as Tomcat's JSP support is tightly coupled with that of a WAR layout.
    WAR's give the option of deploying your app to a standalone container or app server
    JAR's are for an embedded container that comes with the resulting application
    Change 'packaging' value to 'war' in the maven pom.xml

15. What embedded containers does Spring Boot support?
    Tomcat,
    Jetty,
    Undertow servers.

16. How does Spring Boot know what to configure?
    Spring Boot doesn’t generate code or make edits to your files. Instead, when you start up your application, Spring Boot dynamically wires up beans and settings and applies them to your application context.

    Spring Boot autoconfiguration represents a way to automatically configure a Spring application based on the dependencies that are present on the classpath.
    The Spring Boot autoconfiguration mechanism heavily depends on the @Conditional feature.

    A specific class is present in the classpath
    A Spring bean of a certain type isn’t already registered in the ApplicationContext
    A specific file exists in a location
    A specific property value is configured in a configuration file
    A specific system property is present/absent
    The key to Spring Boot’s autoconfiguration is its @EnableAutoConfiguration annotation

17. What does @EnableAutoConfiguration do?
    The @EnableAutoConfiguration annotation enables Spring Boot auto-configuration. As earlier,
    Spring Boot autoconfiguration attempts to create and configure Spring beans based on the
    dependencies available on the class-path to allow developers to quickly get started with different
    technologies in a Spring Boot application and reducing boilerplate code and configuration.

    1. scanning the classpath components and
    2. registering the beans that match various conditions.

18. What does @SpringBootApplication do?
    1. Turns on autoconfig
    2. Enables auto-scanning
    3. Defines a configuration class

    The @SpringBootApplication is a convenience-annotation that can be applied to Spring Java
    configuration classes. The @SpringBootApplication is equivalent to the three annotations
    @Configuration, @EnableAutoConfiguration and @ComponentScan.

    1. @EnableAutoConfiguration: enable Spring Boot’s auto-configuration mechanism
    2. @ComponentScan: enable @Component scan on the package where the application is located
    3. @Configuration: allow to register extra beans in the context or import additional configuration classes

19. Does Spring Boot do component scanning? Where does it look by default?
    @SpringBootApplication automatically includes @ComponentScan which searches in the current and child packages for @Components
    Spring Boot does not do component scanning unless a configuration class, annotated with
    @Configuration, that is also annotated with the @ComponentScan annotation or an annotation, for
    instance @SpringBootApplication, that is annotated with the @ComponentScan annotation.

20. How are DataSource and JdbcTemplate auto-configured?
    DataSource configuration is controlled by external configuration properties in spring.datasource.*. For example, you might declare the following section in application.properties:

    spring.datasource.url=jdbc:mysql://localhost/test
    spring.datasource.username=dbuser
    spring.datasource.password=dbpass
    spring.datasource.driver-class-name=com.mysql.jdbc.Driver
    Spring’s JdbcTemplate and NamedParameterJdbcTemplate classes are auto-configured, and you can @Autowire them directly into your own beans.

    @Component
    public class MyBean {

      private final JdbcTemplate jdbcTemplate;

      @Autowired
      public MyBean(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
      }
    }

21. What is spring.factories file for?
    the META-INF/spring.factories defined all the auto-configuration classes that will be used to guess what kind of application you are running. It's the secret behind the auto-configuration.

    You need to specify the class that will be picked up by the EnableAutoConfiguration class. This class imports the EnableAutoConfigurationImportSelector that will inspect the spring.factories and loads the class and executes the declaration.

    Some events are actually triggered before the ApplicationContext is created, so you cannot register a listener on those as a @Bean. You can register them with the SpringApplication.addListeners(…) method or the SpringApplicationBuilder.listeners(…) method.

    If you want those listeners to be registered automatically, regardless of the way the application is created, you can add a META-INF/spring.factories file to your project and reference your listener(s) by using the org.springframework.context.ApplicationListener key.

    org.springframework.context.ApplicationListener=com.example.project.MyListener

    Spring Boot checks for the presence of a META-INF/spring.factories file within your published jar. The file should list your configuration classes under the EnableAutoConfiguration key, as shown in the following example:

    org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.mycorp.libx.autoconfigure.LibXAutoConfiguration, com.mycorp.libx.autoconfigure.LibXWebAutoConfiguration

22. How do you customize Spring auto configuration?
    Creating a Custom Auto-Configuration

    1. To create a custom auto-configuration, we need to create a class annotated as @Configuration and register it
    @Configuration
    public class MySQLAutoconfiguration {}

    2. The next mandatory step is registering the class as an auto-configuration candidate, by adding the name of the class under the key org.springframework.boot.autoconfigure.EnableAutoConfiguration in the standard file resources/META-INF/spring.factories:
    org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.baeldung.autoconfiguration.MySQLAutoconfiguration

    3. Auto-configuration is designed using classes and beans marked with @Conditional annotations so that the auto-configuration or specific parts of it can be replaced.

    Note that the auto-configuration is only in effect if the auto-configured beans are not defined in the application. If you define your bean, then the default one will be overridden.

23. What are the examples of @Conditional annotations? How are they used?
    The Spring Boot autoconfiguration mechanism heavily depends on the @Conditional feature. Using the @Conditional approach, you can register a bean conditionally based on any arbitrary condition.

    For example, you may want to register a bean when:
    A specific class is present in the classpath
    A Spring bean of a certain type isn’t already registered in the ApplicationContext
    A specific file exists in a location
    A specific property value is configured in a configuration file
    A specific system property is present/absent

    @ConditionalOnBean: Matches when the specified bean classes and/or names are already registered.
    @ConditionalOnMissingBean: Matches when the specified bean classes and/or names are not already registered.
    @ConditionalOnClass: Matches when the specified classes are on the classpath.
    @ConditionalOnMissingClass: Matches when the specified classes are not on the classpath.
    @ConditionalOnProperty: Matches when the specified properties have a specific value.
    @ConditionalOnResource: Matches when the specified resources are on the classpath.
    @ConditionalOnWebApplication: Matches when the application context is a web application context.






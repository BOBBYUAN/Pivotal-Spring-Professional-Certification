1. What is the concept of AOP? Which problem does it solve? What is a cross cutting concern?
    Aspect Oriented Programming (alias AOP) is a concept of separating cross-cutting parts of an application. For example suppose you want to add logging or some security functions to different elements of your application. Usually you would add some code to each element you want to add that functionality to. But using AOP you can externalize/separate these functions into something called aspects.

    Solve:
    * Avoid code duplication
    * Avoid mixing of, for instance, business logic and cross cutting concerns

    Cross cutting concern is functionality that is used in multiple locations throughout an application.
    Such functionality is applied/used in areas of an application that normally are not related to each
    other. Commonly such concerns are not related to business logic, but there are exceptions.
    Cross cutting concerns do not usually fit well in the model of object.

    Name three typical cross cutting concerns:
    • Logging
    • Caching
    • Security
    • Data validation
    • Transaction management
    • Error handling
    • Monitoring
    • Custom business rules

    What two problems arise if you don't solve a cross cutting concern via AOP?
    • Code duplication
    Same, or similar, code duplicated in several locations.
    • Mixing of concerns
    Code of the cross cutting concern, for instance transaction handling, would become mixed
    with service layer code etc. Makes the code more difficult to read and maintain.

2. What is a pointcut, a join point, an advice, an aspect, weaving?
    Join Point:
    A join point is a point in the execution of a program at which additional behavior can be inserted
    using AOP.
    Spring AOP only supports public method invocation join points. Compare to AspectJ which
    supports all of the above listed join point types and more.

    Pointcut
    A pointcut selects one or more join points out of the set of all join points in an application.
    execution(Entity *Repository.find*(..))

    Advice
    Advice is the additional behavior, typically a cross cutting concern, that is to be executed at certain
    places (at join points) in a program. The example in the above Pointcut section shows an around
    advice that prints a message before and after the execution of a join point.
    Please refer to subsequent sections that describes the different types of advice available and how
    advice is implemented in Spring AOP.

    Aspect
    An aspect brings together one or more pointcuts with one or more advice. Typically one aspect
    encapsulates one cross cutting concern, as to adhere to the single responsibility principle.

    Weaving: linking aspects with other application types or objects to create an advised object. This can be done at compile time (using the AspectJ compiler, for example), load time, or at runtime. Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.

3. How does Spring solve (implement) a cross cutting concern?
    Spring uses proxy objects to implement the method invocation interception part of AOP. Such proxy
    objects wrap the original Spring bean and intercepts method invocations as specified by the set of
    pointcuts defined by the cross cutting concern.
    Spring AOP uses two slightly different types of proxy techniques:
    • JDK dynamic proxies
    • CGLIB proxies

    JDK dynamic proxying (creating some class that implements the same interface as the proxied bean) CGLib proxying (creating subclass for the proxied bean) important to mention here is the fact that starting from version 3.2 you don’t have to attach any additional libraries to support CGLib proxying.

4. Which are the limitations of the two proxy-types?
    Common:
    Invocation of advised methods on self.
    A proxy implements the advice which is executed prior to invoking the method on a Spring
    bean. The Spring bean being proxied is not aware of the proxy and when a calling a method
    on itself, the proxy will not be invoked.

    JDK Dynamic Proxies
    Limitations of JDK dynamic proxies are:
    • Class for which a proxy is to be created must implement an interface.
    This is due to the way that JDK dynamic proxies are implemented – see section on JDK
    dynamic proxies above!
    • Only public methods in implemented interfaces will be proxied.
    Invocation of methods on self will never be proxied, see above limitation common to both
    proxy types. Thus only public methods declared in an implemented interface, which are the
    only type of method that can be invoked from other objects, will be proxied.

    Limitations of CGLIB proxies are:
    • Class for which a proxy is to be created must not be final.
    Since CGLIB creates a subclass of the class for which a proxy is to be created, the latter
    class must not be final since this prevents subclassing.
    • Method(s) in class for which a proxy is to be created must not be final.
    The motivation is analogous to above; final methods cannot be overridden in subclasses.
    • Only public and protected methods will be proxied.
    Invocation of methods on self will never be proxied, see above limitation common to both
    proxy types.

    What visibility must Spring bean methods have to be proxied using Spring AOP?
    JDK Proxies: Only public interface calls on proxy are intercepted.
    CGLIB Proxies: Public , protected and package level method calls on proxies will be intercepted.

5. How many advice types does Spring support. Can you name each one?
    Before, After, Around, AfterReturning, AfterThrowing

    • What are they used for?
    Before advice.
    Executed before a join point. Cannot prevent proceeding to the join point unless the advice
    throws an exception.
    • After returning advice.
    Executed after execution of a join point completed without throwing exceptions.
    • After throwing advice.
    Executed after execution of a join point that resulted in an exception being thrown.
    • After (finally) advice.
    Executed after execution of a join point, regardless of whether an exception was thrown or
    not.
    • Around advice.
    Executed before and after (around) a join point. Can chose to execute the join point or not.
    Can chose to return any return value from the execution of the join point or return some
    other return value.

    • Which two advices can you use if you would like to try and catch exceptions?
    Only around advice allows you to catch exceptions in an advice that occur during execution of a
    join point.

6. What do you have to do to enable the detection of the @Aspect annotation?
     Annotate advice classes with @Component.
    Thus advice classes should be annotated with both @Aspect and @Component. With
    component scanning enabled, advice classes will be auto-detected.
    • Create Spring beans from the advice classes.
    Use a regular method annotated with @Bean in a @Configuration class.

    • What does @EnableAspectJAutoProxy do?
    The @EnableAspectJAutoProxy annotation should be applied to a @Configuration class

    The @EnableAspectJAutoProxy annotation enables support for handling Spring beans that are
    annotated with AspectJ’s @Aspect annotation. It is important to note that the @Aspect annotation
    in itself does not cause any Spring beans to be created from the annotated classes

7. If shown pointcut expressions, would you understand them?
    execution(public * *(..)) && within(se.ivankrizsan.spring..*)
    1. Execution of public methods within all packages that have any return type and take zero or
    more parameters.
    2. AND
    3. Within the package se.ivankrizsan.spring or any subpackage of this package.

    [method visibility] [return type] [package].[class].[method]([parameters] [throws exceptions])
    [package].[class]
    Wildcard “..” may be used last in package name to include all sub-packages. Wildcard * may be used in
    package name.

    this(MySuperService)
    In this example, MySuperService is an interface. The above pointcut expression will match join
    points in proxy objects that implement the MySuperService interface. Wildcards can not be used in type names

    target(MySuperServiceImpl)
    The target pointcut designator matches all join point where the target object, for instance the object
    on which a method call is being made, is of specified type (class or interface). With Spring AOP, the
    target object will reference the Spring bean being proxied.

    args(long, long)
    The args pointcut designator matches join points, in Spring AOP method execution, where the
    argument(s) are of the specified type(s).
    The .. wildcard may be used to specify zero or more parameters of arbitrary type.
    The * wildcard can be used to specify one parameter of any type.
    The following example selects all join points where the arguments are of any type from the java.util
    package:
    args(java.util.*)

    @target(org.springframework.stereotype.Service)
    The above example selects all join points in all classes annotated with the Spring @Service
    annotation

    @Before("@args(se.ivankrizsan.spring.aopexamples.CleanData) && within(se.ivankrizsan.spring..*)")
    The @args pointcut designator matches join points where an argument type (class) is annotated with
    the specified annotation. Note that it is not the argument that is to be annotated, but the class

    @within(org.springframework.stereotype.Service)
    The above pointcut will select all join points in all classes annotated with the Spring @Service
    annotation.

    @annotation(se.ivankrizsan.spring.aopexamples.MySuperSecurityAnnotation)
    The above pointcut will select all join points in all methods annotated with the @MySuperSecurity
    Annotation annotation in all classes.

    bean(mySuperService)
    While all of the above pointcut designators are supported in AspectJ, the bean pointcut designator is
    not. This pointcut designator selects all join points within a Spring bean.

    The above pointcut will select all join points in the Spring bean named “mySuperService”
    The pattern specifying which join points to select consists of the name or id of a Spring bean. The
    wildcard * may be used in the pattern, making it possible to match a set of Spring beans with one
    pointcut expression.

    • For example, in the course we matched getter methods on Spring Beans, what would
    be the correct pointcut expression to match both getter and setter methods?

    execution(void set*(*)) || execution(* get*())

8. What is the JoinPoint argument used for?
    As seen in earlier examples, a parameter of the type JoinPoint can be added to methods
    implementing the following types of advice:
    • Before
    • After returning
    • After throwing
    • After (finally)
    The parameter must, if present, be the first parameter of the advice method.
    When the advice is invoked, the parameter will hold a reference to an object that holds static
    information about the join point as well as state information.

    Any advice method may declare as its first parameter, a parameter of type org.aspectj.lang.JoinPoint (please note that around advice is required to declare a first parameter of type ProceedingJoinPoint, which is a subclass of JoinPoint. The JoinPoint interface provides a number of useful methods such as:
      getArgs() (returns the method arguments)
      getThis() (returns the proxy object)
      getTarget() (returns the target object)
      getSignature() (returns a description of the method that is being advised)
      toString() (prints a useful description of the method being advised).

9. What is a ProceedingJoinPoint? When is it used?
    Used ONLY in the @Around method
    proceed() will call the advised method
    @Around("execution(* com.mumz.test.spring.aop.BookShelf.addBook(..))") public void aroundAddAdvice(ProceedingJoinPoint pjp){...
    ProceedingJoinPoint is used to reference the method that is being advised with an @Around advice. Call the ProceedingJoinPoint’s proceed() method for the advised method to run.


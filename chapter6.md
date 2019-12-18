1. What are authentication and authorization? Which must come first?
    The short explanation of authentication is that it is the process of verifying that, for instance, a user
    of a computer system is who he/she claims to be

    The short explanation of authorization is the process of determining that a user of a computer
    system is permitted to do something that the user is attempting to do.

    Unless there is some type of authorization that specifies what resources and/or functions that can be
    accessed by anonymous users, authentication must always come before authorization.

2. Is security a cross cutting concern? How is it implemented internally?
    Yes security is a cross-cutting concern. Spring Security internally is implemented using AOP - the same way as Transactions management.

3. What is the delegating filter proxy?
    It can be declared in the web.xml. It provides the link between the web.xml and the application context. It delegates the Filter's methods through to a bean which is obtained from the Spring application context.

4. What is the security filter chain?
    @EnableGlobalMethodSecurity can enable annotation-based security

    Spring Security's web infrastructure is based entirely on standard servlet filters. It doesn't use servlets or any other servlet-based frameworks (such as Spring MVC) internally, so it has no strong links to any particular web technology. It deals in HttpServletRequests and HttpServletResponses and doesn't care whether the requests come from a browser, a web service client, an HttpInvoker or an AJAX application.

    The order that filters are defined in the chain is very important. Irrespective of which filters you are actually using, the order should be as follows:

    ChannelProcessingFilter

    SecurityContextPersistenceFilter

    ConcurrentSessionFilter

    UsernamePasswordAuthenticationFilter

    CasAuthenticationFilter

    BasicAuthenticationFilter

    SecurityContextHolderAwareRequestFilter

    JaasApiIntegrationFilter

    RememberMeAuthenticationFilter

    AnonymousAuthenticationFilter

    ExceptionTranslationFilter

    FilterSecurityInterceptor

5. What is a security context?
    Context that holds security information about the current thread of execution. This information includes details about the principal. Context is held in the SecurityContextHolder.

6. What does the ** pattern in an antMatcher or mvcMatcher do?
    There are two wildcards that can be used in URL patterns:
    • *
    Matches any path on the level at which the wildcard occur.
    Example: /services/* matches /services/users and /services/orders but not
    /services/orders/123/items.
    • **
    Matches any path on the level at the wildcard occurs and all levels below.
    If only /** or ** then will match any request.
    Example: /services/** matches /services, /services/, /services/users and /services/orders and
    also /services/orders/123/items etc.

7. Why is the usage of mvcMatcher recommended over antMatcher?
    mvcMatcher can also restrict the URLs by HTTP method

    As an example antMatchers("/services") only matches the exact “/services” URL while
    mvcMatchers("/services") matches “/services” but also “/services/”, “/services.html” and
    “/services.abc”. Thus the mvcMatcher matches more than the antMatcher and is more forgiving as
    far as configuration mistakes are concerned. In addition, the mvcMatchers API uses the same
    matching rules as used by the @RequestMapping annotation. Finally, the mvcMatchers API is
    newer than the antMatchers API.

8. Does Spring Security support password hashing? What is salting?
    Yes, Password hashing is the process of calculating a hash-value for a password. The hash-value is
    stored, for instance in a database, instead of storing the password itself. Later when a user attempts
    to log in, a hash-value is calculated for the password supplied by the user and compared to the
    stored hash-value. If the hash-values does not match, the user has not supplied the correct password.
    In Spring Security, this process is referred to as password encoding and is implemented using the
    PasswordEncoder interface.

    A salt used when calculating the hash-value for a password is a sequence of random bytes that are
    used in combination with the cleartext password to calculate a hash-value. The salt is stored in
    cleartext alongside the password hash-value and can later be used when calculating hash-values for
    user-supplied passwords at login.
    The reason for salting is to avoid always having the same hash-value for a certain word, which
    would make it easier to guess passwords using a dictionary of hash-values and their corresponding
    passwords.

9. Why do you need method security? What type of object is typically secured at the method level (think of its purpose not its Java type).
    Method security is an additional level of security in web applications but can also be the only layer
    of security in applications that do not expose a web interface.

    Method-level security is commonly applied to services in the service layer of an application.

10. What do @PreAuthorized and @RolesAllowed do? What is the difference between them?
    @RolesAllowed (JAVA based) SPEL NO defines a list of security configuration attributes for business methods.
    @PreAuthorize (Spring based) SPEL YES Writes expressions that limit method invocation based on the roles/permissions, the current authenticated user, and the arguments passed into the method.

    The Secured annotation is used to define a list of security configuration attributes for business methods. This annotation can be used as a Java 5 alternative to XML configuration. @Secured and @RolesAllowed perform identical functionality in Spring. The difference is that @Secured is a Spring specific annotation while @RolesAllowed is a Java standard annotation (JSR250).

    • How are these annotations implemented?
    Method-level security is accomplished using Spring AOP proxies

    • In which security annotation are you allowed to use SpEL?
    PreAuthorize, PostAuthorize, PreFilter, PostFilter

1. What is a Repository interface?
    An instant repository, also known as a Spring Data repository, is a repository that need no
    implementation and that supports the basic CRUD (create, read, update and delete) operations. Such
    a repository is declared as an interface that typically extend the Repository interface or an interface
    extending the Repository interface. The Repository uses Java generics and takes two type
    arguments; an entity type and a type of the primary key of entities.

2. How do you define a Repository interface?
    The following example shows how a repository that
    store entities of the type Person, each with an
    id of the type SocialSecurityNumber is declared:
    public interface PersonRepository extends JpaRepository<Person, SocialSecurityNumber> {
}
    Why is it an interface not a class?
    The first and most obvious reason for using an interface to define a Spring Data repository is that
    there usually is no need for implementation of the methods defined in the interface.
    Repositories are defined as interfaces in order for Spring Data to be able to use the JDK dynamic
     prox y mechanism to create the proxy objects that intercept calls to repositories.
    This also allows for supplying a custom base repository implementation class that will act as a
    (custom) base class for all the Spring Data repositories in the application.

3. What is the naming convention for finder methods in a Repository interface?
    find(First[count])By[property expression][comparison operator][ordering operator]
    Finder method names always start with “find”.
    • Optionally, “First” can be added after “find” in order to retrieve only the first found entity.
    When retrieving only one single entity, the finder method will return null if no matching
    entity is found. Alternatively the return-type of the method can be declared to use the Java
    Optional wrapper to indicate that a result may be absent.
    If a count is supplied after the “First”, for example “findFirst10”, then the count number of
    entities first found will be the result.
    • The optional property expression selects the property of a managed entity that will be used
    to select the entity/entities that are to be retrieved.
    Properties may be traversed, in which case underscore can be added to separate names of
    nested properties to avoid disambiguities. If the property to be examined is a string type,
    then “IgnoreCase” may be added after the property name in order to perform caseinsensitive comparison.
    Multiple property expressions can be chained using “AND” or “OR”.
    • The optional comparison operator enables creation of finder methods that selects a range of
    entities.
    Some comparison operators available are:
    LessThan, GreaterThan, Between, Like.
    • Finally the optional ordering operator allows for ordering a list of multiple entities on a
    property in the entity.
    This is accomplished by adding “OrderBy”, a property expression and “Asc” or “Desc”.
    Example: findPersonByLastnameOrderBySocialsecuritynumberDesc – find persons that
    have a supplied last name and order them in descending order by social security number.

4. How are Spring Data repositories implemented by Spring at runtime?
   First of all, there's no code generation going on, which means: no CGLib, no byte-code generation at all. The fundamental approach is that a JDK proxy instance is created programmatically using Spring's ProxyFactory API to back the interface and a MethodInterceptor intercepts all calls to the instance and routes the method into the appropriate places

5. What is @Query used for?
    The @Query annotation allows for specifying a query to be used with a Spring Data JPA repository
    method. This allows for customizing the query used for the annotated repository method or
    supplying a query that is to be used for a repository method that do not adhere to the finder method
    naming convention described earlier.
    public interface PersonRepository extends JpaRepository<Person, Long> {
     @Query("select p from Person p where p.emailAddress = ?1")
     Person findByEmailAddress(String emailAddress);
    }


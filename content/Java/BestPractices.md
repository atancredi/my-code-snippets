
# Best Practices in Java

- Follow the SOLID Principles
    
    **SOLID** acronym stands for -
    
    - Single Responsibility Principle (SRP)
    - Open Closed Principle (OCP)
    - Liskov Substitution Principle (LSP)
    - Interface Segregation Principle (ISP)
    - Dependency Inversion Principle (DIP)
    
    [What are the SOLID principles in Java?](https://www.educative.io/answers/what-are-the-solid-principles-in-java)
    
- Don’t Repeat Yourself - Keep It Stupid Simple
    
    *The DRY principle is stated as, “Every piece of knowledge or logic must have a single, unambiguous representation within a system.”*
    
    *Divide your code and logic into smaller reusable units and use that code by calling it where you want. Don’t write lengthy methods, but divide logic and try to use the existing piece in your method.*
    
    ***KISS** is a design principle which states that designs and/or systems should be as simple as possible. Wherever possible, complexity should be avoided in a system — as simplicity guarantees the greatest levels of user acceptance and interaction.*
    
    *Each method should only solve one small problem **(the S. from SOLID)**, not many use cases. If you have a lot of conditions in the method, break these out into smaller methods.*
    
- Stop Hardcoding Stuff
- Declare Class Members Private Wherever Possible
    
    *Rule of thumb is to try to hide information as much as possible, sharing it only when absolutely necessary.*
    
    I’d say make everything private by default, and then expose only those parts that absolutely have to be public. The more can be made private the better.
    
- Use StringBuilder/Buffer For String Manipulation
    
    The memory is allocated only once for the Object. Buffer and Builder objects are allocated memory in **HEAP.** 
    Every time you modify the string the variable gets modified. So memory is not wasted.
    
    [String vs StringBuffer vs StringBuilder | DigitalOcean](https://www.journaldev.com/538/string-vs-stringbuffer-vs-stringbuilder)
    
- Efficient Null Checking
    1. java.util.Optional
    2. Utility classes of apache.commons
    3. Objects::nonNull in Streams
    4. requireNonNull methods of java.util.Objects
    5. Lombok’s Builder.Default
    6. NotNull, NotEmpty, NotBlank Annotations
    
    [Checking for Nulls in Java? Minimize Using "If Else"](https://medium.com/better-programming/checking-for-nulls-in-java-minimize-using-if-else-edae27016474)
    
- Use Design Patterns
    
    They are basically reusable solutions to common programming problems
    

# Best Practices in Spring Boot

- Proper packaging style
    
    Helps to understand the code and the flow of the application easily
    
    It is possible to structure the application with meaningful packaging
    
    Meaningful code can be included into separate packages, in order to have a tidier code (especially convenient in small-size microservices)
    
    When working on a huge codebase a feature-based approach can be useful
    
- Use design patterns
    
    [How to use the Builder design pattern with Spring Boot](https://www.javastackflow.com/2022/09/how-to-use-builder-design-pattern-with.html)
    
- Use Spring Boot starters
    
    Starter dependencies already bundled with the required dependencies (for example spring-boot-starter-web is bundled with jackson, spring-core, spring-mvc, spring-boot-starter-tomcat)
    
    It makes the dev avoid the task of adding dependencies separately, and also helps avoid version mismatches.
    
- Use proper versions of the dependencies
    
    Use the latest stable GA versions
    
    Do not use different versions of the same package and always use <properties> to specify the version if there are multiple dependencies.
    
- Use Lombok
    
    a java library that can be use to reduce the code and to write clean code using annotations
    
    Lombok with Slf4j is recommended.
    
    [springboot-gcs-demo/InputFile.java at master · raviyasas/springboot-gcs-demo](https://github.com/raviyasas/springboot-gcs-demo/blob/master/src/main/java/com/app/entity/InputFile.java)
    
- Use constructor injection with Lombok
    
    When we talk about dependency injection, there are two types.
    
    One is “**constructor injection**” and the other is “**setter injection**”. Apart from that, you can also use “**field injection**” using the very popular **@Autowired** annotation.
    
    But we highly recommend using **Constructor injection** over other types. Because it allows the application to initialize all required dependencies at the initialization time.
    
    This is very useful for unit testing.
    
    The important thing is, that we can use the **@RequiredArgsConstructor** annotation by Lombok to use constructor injection.
    
    Check this *[sample controller](https://github.com/raviyasas/springboot-gcs-demo/blob/master/src/main/java/com/app/controller/FileController.java)* for your reference.
    
- Use slf4j logging
    
    Do not use System.out.print()
    
    Slf4j is recommended to use along with logback which is the default logging framework in Spring Boot.
    
    Always use slf4j { } and avoid using String interpolation in logger messages. Because string interpolation consumes more memory.
    
    Please check *[this file](https://github.com/raviyasas/springboot-gcs-demo/blob/master/src/main/java/com/app/service/FileServiceImpl.java)* for your reference to get an idea about, implementing a logger.
    
    You can use Lombok ***@Slf4j** annotation to create a logger very easily.*
    
    If you are in a micro-services environment, you can use the ELK stack.
    
- Use controllers only for routing
    
    Controllers are dedicated to routing.
    
    It is **stateless** and **singleton**.
    
    The DispatcherServlet will check the *@RequestMapping* on *Controllers*
    
    Controllers are the ultimate target of requests, then requests will be handed over to the service layer and processed by the service layer.
    
    The business logic **should not be** in the controllers.
    
- Use Services for business logic
    
    The **complete business logic goes here** with validations, caching…etc.
    
    Services are also singleton.
    
    Services communicate with the persistence layer and receive the results.
    
- Avoid NullPointerException
    
    To avoid **NullPointerException** you can use **Optional** from java.util package.
    
    You can also use null-safe libraries. Ex: **Apache Commons StringUtils**
    
    Call **equals()** and **equalsIgnoreCase()** methods on known objects.
    
    Use **valueOf()** over **toString()**
    
    Use IDE-based **@NotNull** and **@Nullable** annotations.
    
- Use best practices for the Collection framework
    
    Use appropriate collection for your data set.
    
    Use **forEach** with Java 8 features and avoid using legacy for loops.
    
    Use **interface type** instead of the implementation.
    
    Use **isEmpty()** over **size()** for better readability.
    
    Do not return null values, you can return an empty collection.
    
    If you are using objects as data to be stored in a hash-based collection, you should override equals() and hashCode() methods. Please check this article “*[How does a HashMap internally work](https://medium.com/@raviyasas/how-a-hashmap-internally-works-93e3887978f3)*”.
    
- Use pagination
    
    This will improve the performance of the application.
    
    If you’re using **Spring Data JPA**, the ***PagingAndSortingRepository*** makes using pagination very easy and with little effort.
    
- Use caching
    
    Caching is another important factor when talking about application performance.
    
    By default Spring Boot provides caching with **ConcurrentHashMap** and you can achieve this by **@EnableCaching** annotation.
    
    If you are not satisfied with default caching, you can use **Redis**, **Hazelcast,** or any other distributed caching implementations.
    
    Redis and Hazelcast are **in-memory** caching methods. You also can use database cache implementations as well.
    
- Use custom exception handler with global exception handling
    
    This is very important when working with large enterprise-level applications.
    
    Apart from the general exceptions, we may have some scenarios to identify some specific error cases.
    
    Exception adviser can be created with **@ControllerAdvice** and we can create separate exceptions with meaningful details.
    
    It will make it much easier to identify and debug errors in the future.
    
    Please check *[this](https://github.com/raviyasas/springboot-exceptions-demo/tree/master/src/main/java/com/app/exception)* and *[this](https://github.com/raviyasas/springboot-gcs-demo/tree/master/src/main/java/com/app/exception)* for your reference.
    
- Use custom response object
    
    A custom response object can be used to return an object with some specific data with the requirements like HTTP status code, API code, message…etc.
    
    We can use the **builder design pattern** to create a custom response object with custom attributes.
    
    Please check ***[this article](https://www.javastackflow.com/2022/09/how-to-use-builder-design-pattern-with.html)*** for your reference.
    
- Remove unnecessary codes, variables, methods, and classes.
    
    **Unused variable** declarations will acquire some memory.
    
    Remove unused methods, classes…etc because it will impact the performance of the application.
    
    Try to avoid nested loops. You can use maps instead.
    
- Using comments
    
    Commenting is a good practice unless you abuse it.
    
    DO NOT comment on everything. Instead, you can write descriptive code using meaningful words for classes, functions, methods, variables…etc.
    
    Remove commented codes, misleading comments, and story-type comments.
    
    You can use comments for warnings and explain something difficult to understand at first sight.
    
- Use meaningful words for classes, methods, functions, variables, and other attributes.
    
    This looks very simple, but the impact is huge.
    
    Always use proper **meaningful and searchable** naming conventions with proper case.
    
    Usually, we use **nouns or short phrases** when declaring **classes**, **variables**, and **constants**. Ex: String firstName, const isValid
    
    You can use **verbs and short phrases with adjectives** for **functions** and **methods**. Ex: readFile(), sendData()
    
    Avoid using **abbreviating variable** names and **intention revealing** names. Ex: int i; String getExUsr;
    
    If you use this meaningfully, declaration comment lines can be reduced. Since it has meaningful names, a fresh developer can easily understand by reading the code.
    
- Use SonarLint
    
    This is very useful for identifying small bugs and best practices to avoid unnecessary bugs and code quality issues.
    
    You can install the plugin into your favorite IDE.

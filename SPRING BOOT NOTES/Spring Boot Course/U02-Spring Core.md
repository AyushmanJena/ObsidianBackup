---
lastSync: Tue Oct 08 2024 14:41:06 GMT+0530 (India Standard Time)
---
### IOC : Inversion Of Control
- IOC is basically creating objects and managing them automatically instead of creating them manually using new keyword, which is the usual way of doing it in java.
- ![[Screenshot 2024-10-06 142740.png]]
- *Based on the configuration, the object factory (or spring container) returns the object*

- Spring container's primary function : 
	- Create and manage objects (Inversion Of Control)
	- Inject Object Dependencies (Dependency Injection)

- Configuring Spring Container : 
	- XML configuration file (old and not recommended)
	- Java Annotations
	- Java source code

### Spring Dependency Injection :
- Inserting an object instead of creating it.
- Dependency inversion principle : The client delegates to another object the responsibility of providing its dependencies.
- ![[Screenshot 2024-10-06 143634.png]]

- Lets say The DemoController wants to use a coach
	- New Helper : Coach
	- This is a dependency
	and we need to inject this dependency

- There are multiple types of injection 
- Recommended two types are : 
	- Constructor Injection (recommended)(when you have required dependencies)
	- Setter Injection (when you have optional dependencies)

### Autowiring (For Dependency Injection)
Spring will look for a class that matches 
	- matches by type : class or interface 
Spring will inject it automatically... hence it is autowired

Autowiring example : 
![[Screenshot 2024-10-06 145159.png]]
### STEPS for CONSTRUCTOR INJECTION : 
#### 1. Define the dependency interface and class
	Coach.java
```java
public interface Coach{
	String getDailyWorkout();
}
```
	CricketCoach.java
```java
@Component
public class CricketCoach implements Coach{
	@Override
	public String getDailyWorkout(){
		return "practice fast bowling for 15 minutes";	
	}
}
```
##### Component Annotation : 
- Marks the class as a spring bean
- A spring bean is just a regular java class that is managed by Spring
- @Component also makes the bean available for dependency injection.

#### 2.  Create Demo REST Controller
`DemoController.java`
```java
@RestController
public class DemoController{
	private Coach myCoach;
	@Autowired
	public DemoController(Coach theCoach){ // contstructor injection
		myCoach = theCoach;
	}
}
```
- @Autowired annotation tells Spring to inject a dependency
- If you only have one constructor then Autowired on constructor is optional
note : as of now we only have one Coach implementation : CricketCoach

#### 3. Add @GetMapping 
`DemoController.java`
```java
@RestController
public class DemoController{
	...
	@GetMapping("/dailyWorkout")
	public String getDailyWorkout(){
		return myCoach.getDailyWorkout();
	}
}
```

What Spring automatically does for you (in constructor injection) :
`Coach theCoach = new CricketCoach();`
`DemoController demoController = new DemoController(theCoach);`

### Component Scanning :
(Spring scanning for component classes)
By Default spring scans in main spring boot application package for annotations (@Component, etc.)
Then sub packages are scanned recursively
- Automatically registers the beans in the spring container

- For packages outside (we need to explicitely list them)
`SpringcoredemoApplication`
```java
@SpringBootApplication(
scanBasePackages={"com.luv2code.springcoredemo",
					"com.luv2code.util",
					"org.acme.cart",
					"edu.cmu.srs"})
public class SpringcoredemoApplication {
…
}
```

### Setter Injection
- Inject dependencies by calling setter methods on your class

`DemoController.java`
```java
@RestController
public class DemoController{
	private Coach myCoach;
	
	@Autowired
	public void setCoach(Coach theCoach){
		myCoach = theCoach;
	}
	...
}
```

What spring does for you (in setter injection) :
`Coach theCoach = new CricketCoach();`
`DemoController demoController = new DemoController();`
`demoController.setCoach(theCoach);`

## Autowiring and Qualifiers Annotations
Multiple Coach Implementations 
-> Multiple classes implement Coach, all having @Component Annotation
Cricket Coach
Baseball Coach
Track Coach
Tennis Coach
-> issue : Required a single bean but 4 were found 

Solving this issue : 
using **@Qualifier** or using **@Primary**
##### 1. using @Qualifier
specify the bean id : cricketCoach
bean id-> same as the class name but the first character is lower case
```java
@Autowired
public DemoController(@Qualifier("cricketCoach") Coach theCoach){
	myCoach = theCoach;
}
```
#### 2. using @Primary
- Mark the bean as primary
```java
@Component 
@Primary
public class CricketCoach implements Coach{
	@Override
	public String getDailyWorkout(){
		return "Practice bowling";
	}
}
```
- You can have only 1 Primary bean
- If you have both qualifier and primary annotations, then qualifier will have the higher priority

### Lazy Initialization
- By default when application starts, all beans are initialized (Ex : Components, etc.)
- Spring will create an instance of each and make them available
- Using Lazy Initialization :
	- A bean will only be initialized if : 
		- it is needed for dependency injection
		- it is requested explicitly
	- Add **@Lazy** to a given class
```java
@Component
@Lazy
public class TrackCoach implements Coach{
	....
}
```
Global Configuration for lazy initialization :
`spring.main.lazy-initialization=true`

**PROS of lazy initialization :**
-> Gives faster startup times
**CONS of lazy initialization :**
-> If you have web related components like @RestController, not created until request
-> May not discover configuration issues until its too late
-> Need to make sure you have enough memory for all beans once created

### Bean Scopes
- Lifecycle of a bean and number of instances created
- Default Bean scope is **singleton** :
	- one instance of the bean is created
	- cached in memory
	- all dependency injections for the bean reference the same bean
```java
@RestController
public class DemoController{
	private Coach myCoach;
	private Coach theCoach;

	@Autowired
	public DemoController(@Qualifier("cricketCoach" Coach coachOne),
							@Qualifier("cricketCoach")Coach coachTwo){
		myCoach = coachOne;
		theCoach = coachTwo;
	}
}
```
In the example both coachOne and coachTwo refer to the same CricketCoach bean instance

#### Explicitly specifying Bean Scope
```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
public class CricketCoach implements Coach{
...
}
```

**Additional Spring Bean scopes :** 
- singleton : Create a single instance of the bean, default scope
- prototype : Creates a new bean instance for ech container request
- request : Scoped to HTTP web request (only used for web apps)
- session : scoped to HTTP web session (only used for web apps)
- application : scoped to a web app servletContext (only used for web apps)
- websocket : Scoped to a web socket (only used for web apps)

### Bean Lifecycle
![[Bean Lifecycle.png]]
##### Init : Method Configuration : 
`CricketCoach.java`
```java
	...
	// startup stuff
	@PostConstruct
	public void doMyStartupStuff(){
		System.out.println("Startup Stuff : "+ getClasS().getSimpleName());
	}
```

##### Destroy : Method Configuration : 
`CricketCoach.java`
```java
	...
	// cleanup stuff
	@PreDestroy
	public void doMyCleanupStuff(){
		System.out.println("Cleanup Stuff : "+ getClasS().getSimpleName());
	}
```


### Configuring Beans with Java Code
-> Using @Bean instead of @Component
steps : 
1. create java class and annotate with @Configuration
2. Define @Bean method to configure the bean
	Note : bean id defaults to the method name
```java
@Configuration
public class SportConfig{
	@Bean
	public Coach swimCoach(){
		return new SwimCoach();
	}
}
```
3. Inject the bean into our controller
```java
@RestController
public class DemoController{
	private Coach myCoach;
	@Autowired
	public DemoController(@Qualifier("swimCoach") Coach theCoach){
		myCoach = theCoach;
	}
}
```

##### Use case of @Bean (instead of @Component)
- Make an existing third party class available to spring framework
- You may not have access to the source code of third party class i.e. SwimCoach and it does not have @Component
	- But still you want to use the third party class as your spring bean.
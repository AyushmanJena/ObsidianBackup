---
lastSync: Sun Oct 06 2024 13:45:58 GMT+0530 (India Standard Time)
---
## Maven
- Maven is a project management tool used for build management and dependencies.
- Tell Maven the projects(tools) you're working with (dependencies) like spring, hibernate, etc.
- maven will automatically download the Jar files for you and make them available during compile/ run time.

### How Maven Works :
1. You give maven the project config file.
2. Maven will check local repository for the files.
3. If not found it will get them from remote repository and save them to the local repo.
4. Build and run.
- Maven will handle class/ build path for you automatically.

### Maven standard directory structure :
	pom.xml -> Maven config file
	Directories : 
		src/main/java -> java source codes
		src/main/resources -> Properties or config files
		src/main/webapp -> JSP files and web config files
		src/test -> unit testing code and properties
		target -> Destination directory for compiled code
**POM - Project Object Model file**
		⌊ Project meta data
		⌊ dependencies
		⌊ Plugins
		
	_GAV -> Group id, Artifact id, Version_

	Note : Do not use src/main/webapp dir; if your app is packaged as JAR
		it only works with WAR packaging

### How to run app using command line :
1. Go to project location
	> mvnw package
2. You will get your .jar file (e.g. myApp-0.0.1-SNAPSHOT.jar)
3. To start your application 
	> java - jar target/myApp-0.0.1-SNAPSHOT.jar
4. To stop : ctrl + c
- To run the app using spring boot maven plugin : 
	> mvnw spring-boot:run

## REST
**Representational State Transfer**
- REST is an architectural style that specifies constraints such as uniform interface, to induce desirable properties from web services.
- RESTController is a special type of controller in spring framework that is designed to handle RESTful web services.
	- It combines @Controller and @ResponseBody
```java
@RestController
public class FunRestController {
	@GetMapping("/")
	public String sayHello(){
		return "Hello";
	}
}
```
- Similarly `@GetMapping("/workout")` & `@GetMapping("/messages")`
### Spring Boot Starters
- A curated list of maven dependencies grouped together.
- Removes the confusion of which dependencies you need, (which is there while using spring MVC)
#### Spring MVC : 
- Spring MVC is a java framework for building web applications.
- It is a module of the spring framework that uses the MVC pattern to separate an application into three components (model, view and controller)
- MVC -> Model View Controller
	*note : Apache Tomcat : a free and opensource java application server that allows java code to run in a pure HTTP web server environment*
	
	Ex : For Spring MVC normally you need : spring support, Hibernate validator, web template
	In spring boot you will need : spring boot starter - web
```xml
<dependency>
	<groupId> org.springframework.book</groupId>
	<articactId>spring-boot-starter-web</artifactId>
<dependency>
```
- spring boot starter web contains : spring-web, spring-webmvc, spring-validator, json, tomcat, etc.

- There are 30+ spring boot starters.
- spring-boot-starter-parent : provides maven defaults
- spring-boot-devtools : automatically restarts your application when the code is updated
- spring-boot-actuator : Exposes endpoints to monitor and manage your application
	- Ex : /actuator/health, /info , .... (10+ actuator endpoints)

### Exposing Endpoints :
in `application.properties` : 
```
management.endpoints.web.exposure.include = health, info
management.endpoints.web.exposure.include = *
management.info.env.enabled = true
```

info endpoint custom properties : 
```
info.app.name = My app
info.app.description = created by humans
info.app.version = 1.0.0
```
app.version -> custom

Excluding endpoints : 
management.endpoints.web.exposure.exclude = health
## security
spring-boot-starter-security : dependency
- now you are asked for username and password 
- **default username = user**
- **default password = generated in console log**
to override defaults : (in application.properties)
	`spring.security.user.name = scott`
	`spring.security.user.password = tiger`

## Custom Applicaton properties 
`coach.name = Alecks`
`team.name = paper rex`

Injecting properties into spring boot app : 
```java
@RestController
public class FunRestController{
	@Value("${coach.name}")
	private String coachName;
	
	@Value("${team.name}")
	private String teamName;

	@GetMapping("/teaminfo") // new endpoint
	private String getTeamInfo{
		return "coach:"+coachName + ", Team : "+teamName;
	}
}
```

## Spring Boot Properties
- There are 1000+ properties 
- Grouped into :
	- Core
	- Web
	- Security
	- Data
	- Actuator
	- Integration
	- DevTools
	- Testing
- Web example : 
	- `server.port = 7070  (default is 8080)`
	- `server.servlet.context-path = /myApp (must add /myApp to the url)`
	- `server.servlet.session.timeout = 15m (default is 30m)`
- Core example : 
	- `logging.level.org.springframework = DEBUG`
	- `logging.level.org.hibernate = TRACE`
	- `logging.file.name = my-app.log`
	- `logging.file.path = c:/myapps/demo`
	**Different Logging levels :** 
	- TRACE
	- DEBUG 
	- INFO
	- WARN
	- ERROR
	- FATAL
	- OFF
- Security examples : 
	- `spring.security.user.name = admin`
	- `spring.security.user.password = topSecret`
- Actuator property examples : 
	- `management.endpoints.web.exposure.include = *`     # Endpoints to include by name or wildcard
	- `management.endpoints.web.exposure.exclude = beans, mapping`   # endpoints to exclude
	- `management.endpoints.web.base-path`     # base path for actuator endpoints
- Data Properties : 
	- `spring.datasource.url = jdbc:mysql://localhost:3306/ecommerce`   # JDBC URL of database
	- `spring.datasource.username = scott`  
	- `spring.datasource.password = tiger`


## Controller vs RestController  : 
- Controller : Used for traditional web applications, a controller maps HTTP requests to view names. It's best for returning a view, such as an HTML page.
- RestController : Used for building RESTful web services, a rest controller returns JSON response. It's best for returning data, such as JSON or XML, rather than a view.
Note : A @RestController combines both @Controller and @ResponseBody annotations.
@ResponseBody -> The ResponseBody annotation is used to indicate that the return value of a controller method should be directly bound to the body of the HTTP response.


# SPRINGBOOT
Spring Boot is a java based framework, generally used as backend of enterprise level applications. However due to its lightweight and low resource requirements it is also used for web applications.
It is used for building RESTful web services. From which API calls can be made to fetch data for example. 
Spring boot provides auto configuration, embedded servers and starter dependencies. 

## REST
**Representational State Transfer**
- REST is an architectural style that specifies constraints such as uniform interface, to induce desirable properties from web services.
#### Spring MVC : 
- Spring MVC is a java framework for building web applications.
- It is a module of the spring framework that uses the MVC pattern to separate an application into three components (model, view and controller)
- MVC -> Model View Controller


### IOC : Inversion Of Control
- IOC is basically creating objects and managing them automatically instead of creating them manually using new keyword, which is the usual way of doing it in java.
- ![[Screenshot 2024-10-06 142740.png]]
- *Based on the configuration, the object factory (or spring container) returns the object*

- Spring container's primary function : 
	- Create and manage objects (Inversion Of Control)
	- Inject Object Dependencies (Dependency Injection)

### Spring Dependency Injection :
- Inserting an object instead of creating it. It is handled automatically by spring boot.
Autowiring (For Dependency Injection)
- Spring will look for a class that matches 
	- matches by type : class or interface 
- Spring will inject it automatically... hence it is autowired
##### Component Annotation : 
- Marks the class as a spring bean
- A spring bean is just a regular java class that is managed by Spring
### Component Scanning :
(Spring scanning for component classes)
By Default spring scans in main spring boot application package for annotations (@Component, etc.)


# HIBERNATE
## Hibernate :
- Framework and ORM (Object Relational Mapping) tool 
- Most popular implementation of JPA spec (or interface) and is default in spring boot.
- It is a framework for persisting/ saving java objects in a database.
![[hibernate.png]]

### Benefits of Hibernate :
- Handles low level SQL
- Minimizes JDBC code
- Provides ORM : Object to Relational Mapping

### Object to Relational Mapping (ORM) : 
- The developer defines mapping between java class and database table
## JPA
Jakarta Persistence  API / Java Persistence API
- JPA is like a bridge between the Spring Application models and the relational database that is used for managing and accessing the data between the objects in application and database.
### Entity Manager
- Entity manager is an interface in JPA
- It is the main component for creating queries, etc

# REST APIS
### REST Web Services

Example : App that provides weather report for a city
 ![[REST api 01.png]]

**How to Connect :**
- we will use REST API calls over HTTP
- REST is language independent
**Data Format :** 
 - Commonly  used are XML and JSON

### REST HTTP Basics
REST over HTTP for CRUD applications :
**POST :** Create a new entity
**GET :** Read a list of entities or a single entity
**PUT :** Update an existing entity
**DELETE :** Delete an existing entity
etc.

### Java JSON data binding
- Data binding is the process of converting JSON data to Java POJO
- (POJO -> Plain Old Java Object)
- This process is also known as Mapping, serialization/ deserialization, marshalling/ unmarshalling
- Spring uses **Jackson Project** behind the scenes for data binding : 
- Spring automatically handles Jackson Integration
- ![[REST api 03.png]]
- Our REST service will return `List<Student>` and Jackson in between will automatically convert the list to JSON array.


> [!info]
> SPRING SECURITY COULD HAVE BEEN IMPORTANT BUT WE HAVE NOT USED SPRING SECURITY IN DR CROP :)
> We have used our own authentication, which is not as secure as spring security, but minimizes the security checks while making sure no endpoints are publicly accessible.

# Thymeleaf
- Java templating Engine
- used to generate HTML views for web pages
- Can be used outside of web apps as well
- Can access Java code, objects, spring beans.

imp : 
Thymeleaf is processed on the server side and results are included in the HTML returned to the browser where as other js front end libraries like angular and react are processed on the client side.

- Thymeleaf adds certain elements to our html files to access or modify data.
- There is a communication between the application and the templates through models.

**Model** : A model is a special object that allows us to share information between controllers and view pages (thymeleaf)

Generally we display webpages with getmapping and retrieve data from the web pages using postmapping through html forms.

## MVC STRUCTURE : 
**Controller** :
- Contains Business logic
- Handles Business requests
- Store/retrieve data (database, web-services, etc.)
- Places data in the model

**Model** : 
- Contains your data
- Store/ retrieve data via backend systems (database, web services, spring bean, etc.)

**View Template :** 
- Displays data
- Other view templates(Groovy, Velocity, FreeMarker)


OTHER THINGS TO ADD : 
1. Directory Structure
2. Flow of the project
3. How tokenization and authentication works
4. PDF Dependency 
5. Spring Mail dependency

# FOR OUR PROJECT

## Directory Structure : 
```
/src -> soruce code for our app
/resources -> files required by our app : images, html templates, css, images, etc.
/test -> code for unit testing

in src : 
/Controller and /WebController -> Controller code defines endpoints that are hit by the html user directly or indirectly throught html

/Model -> Holds input and output Classes for operations like ConditionRequest for request object format
ConditionResult for holding data returned by api call
User for objects for holding user details 

/Service -> The actual operations performed in the java application. This contains methods which perform various operations like api calls, sending email, downloading pdf, etc.
These methods are called by the controller methods

/repository -> for holding a special classes which extend MongoRepository which is essential for performing database operations while not writing code for basic CRUD operations.



in resources : 
/static -> css files, images like logos, video files
/templates -> for html templates or layouts of web pages


.gitignore -> mentions files to be ignored by git while pushing code to git/github
mvnw -> maven wrapper, allows us to run maven project without having maven installed on our system
mvnw for linux
mvnw.cmd for windows
pom.xml -> specifies the dependencies we are working with in our maven project 
pom : project object model


/input-storage -> holds all user uploaded images that are to be shared with the python app for disease detection
/output-storage -> holds all the PDF reports generated after detecting the disease.
```

## Flow of the project 
User hits an endpoint :
with the home url user hits a controller endpoint : / which sends him to the index.html page. 
where based on which button he clicks another request is made to open another webpage e.g. login with /login, store with /store
then is asked to login through email otp verification
then user reaches homepage
then he chooses based on which option user chooses, he is sent to disease detection form page or crop recommendation form pages through GET requests
then user puts his data and when submits the form, POST request is made due to which the app receives the data.
Then the backend app communicates with python app to retrieve ML results based on the data through HTTP api call.
For a api call : 
- The user submitted data is stored in an request object model, which is sent to the python app after being converted into JSON request.
- Then the python app returns another JSON as response, which is again converted into another response object model and is displayed to the user.


## User Login and Token Based Authentication 
```
User enters email → OTP generated + stored → User enters OTP → Token created + stored → 
Token stored in session → Each page uses token to find user ID → Authenticated user actions
```
#### Basic Authentication Flow
User Enters Email (/web/login via POST /get-email):
User enters email on the login page.
The system checks if the user exists.
If the user exists, an OTP is generated (generateOTP()), and ideally sent to the user's email (sendOtpToMail()).
The OTP and email are stored in the HTTP session (backend memory, specific to the user's browser).
User Verifies OTP (/web/verify):
User enters the OTP they received.
It is verified against the one in the session.
If correct, a random token (UUID) is generated (authenticate()), linked to the user's ID, and stored in a map (tokenStore).
This token is saved in the session and used as a way to recognize the user in future requests.

####  How Every Page Knows the User
Each time a protected page (like /homepage, /upload, /soil-recommendation) is accessed:
The controller checks the session for the token.
The token is used to get the user ID from tokenStore.
This is how you know which user is making the request, without them needing to log in again.

#### Why This Is Useful
Custom Control: You're not using Spring Security, so you have full flexibility.
Session-Based Simplicity: As long as the session is alive, the user stays logged in.
Stateless Token Mapping: Tokens act as unique user session identifiers — can later be expanded to support things like logout, expiration, or roles.
Email-Based Auth: No passwords needed. OTP login is simple and secure for users.

### MongoDB Integration :
- Use of mongoDB is suitable due to its less complex nature and our usecase which does not involve a interlinked table schema.
- it runs on port 27017
- Database name : drcrop
- Collections : 
```
user -> to store user details
baki sai ku pachari diya @saisp2003 
```


### PDF Dependency :
```xml
<dependency>  
    <groupId>com.github.librepdf</groupId>  
    <artifactId>openpdf</artifactId>  
    <version>2.0.3</version>  
</dependency>
```
This is a third party dependency used to create and manage pdfs. We are using this to create a pdf and add the condition detection results to the pdf and let the user download the pdf


### Mail Dependency :
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-mail</artifactId>  
    <version>3.4.1</version>  
</dependency>
```
This is another dependency by spring boot which allows us to send emails to the specified user's email address through SMTP (Simple Mail Transfer Protocol)

It takes the sender email and a few other attributes in application.properties and sends the email when a specific method is called.


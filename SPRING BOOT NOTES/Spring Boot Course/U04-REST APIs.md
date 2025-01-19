### REST Web Services

Example : App that provides weather report for a city
 ![[REST api 01.png]]

**How to Connect :**
- we will use REST API calls over HTTP
- REST is language independent
**Data Format :** 
 - Commonly  used are XML and JSON
 
> [!note] 
> REST API, REST Web Services, REST Services, RESTful API, RESTful Web Services, RESTful Services : all mean the same thing


### JSON basics
- JavaScript Object Notation
- Lightweight data format for storing and exchanging data ... plain text
- Language independent i.e. can be used with any programming language

Example : 
```json
{
"id":14,
"firstName" : "Tom",
"lastName" : "Ramachandra",
"active" : true,
"courses" : null
}
```
nested objects
```json
"address" : {
	"plot" : 99,
	"city" : Bhubaneswar,
	"state" : Odisha
}
```
JSON Arrays
```json
"languages" : ["Java", "Kotlin", "Python", "Javascript"]
```

### REST HTTP Basics
REST over HTTP for CRUD applications :
**POST :** Create a new entity
**GET :** Read a list of entities or a single entity
**PUT :** Update an existing entity
**DELETE :** Delete an existing entity

##### HTTP Messages : 
![[REST api 02.png]]

**HTTP Request Message consists of :** 
- Request Line : the HTTP Command
- Header Variable : Request's Metadata
- Message Body : contents of the message

**HTTP Response Message consists of :** 
- Response Line : Server Protocol and status code
- Header Variable : Response's Metadata
- Message Body : Contents of the message

**Status Codes :** 
100-199 -> Informational
200-299 -> Successful
300-399 -> Redirection
400-499 -> Client error
500-599 -> Server error

**MIME :** Multipurpose Internet Mail Extension 
	Syntax : type/sub-type
	Ex : text/html, text/plain
### Java JSON data binding
- Data binding is the process of converting JSON data to Java POJO
- (POJO -> Plain Old Java Object)
- This process is also known as Mapping, serialization/ deserialization, marshalling/ unmarshalling
- Spring uses **Jackson Project** behind the scenes for data binding : 
- Spring automatically handles Jackson Integration
- ![[REST api 03.png]]
- Our REST service will return `List<Student>` and Jackson in between will automatically convert the list to JSON array.

##### Development Process : 
1. Create a Java POJO class for student
	- With its own constructors, getters and setter methods
```java
public class Student{
	private String firstName;
	private String lastName;
	
	public Student(){}
	
	public Student(String firstName, String lastName){
		this.firstName = firstName;
		this.lastName = lastName;
	}
	
	public String getFirstName(){
		return firstName;
	}
	
	public String getLastName(){
		return lastName;
	}
	
	public void setFirstName(String firstName){
		this.firstName = firstName;
	}
	
	public void setLastName(String lastName){
		this.lastName= lastName;
	}
}
```
1. Create @RestController and return the java object or list, etc.
Ex : 
```java
@RestController
@RequestMapping("/api")
public class StudentRestController {
	// define endpoint for "/students" - return list of students
	@GetMapping("/students")
	public List<Student> getStudents(){
		List<Student> theStudents = new ArrayList<>();
		
		// adding students hard coded for now
		theStudents.add(new Student("Mario", "Rossi"));
		
		return theStudents; // Jackson will convert the list to JSON array
	}
}
```

##### Path Variables 
- Retrieve a single student by ID
GET  /api/students/{studentId} 
// {studentId} is the pathvariable and  can be 1, 2, 3...

```java
// define endpoint for "/students/id" - return student with id 
@GetMapping("/students/{studentId}")
public Student getStudents(@PathVariable int studentId){ 
	List<Student> theStudents = new ArrayList<>();
	
	// adding students hard coded for now
	theStudents.add(new Student("Mario", "Rossi"));
	...	
	return theStudents.get(studentId);
}
```
> [!warning]
> The variable studentId must match with the path variable 



### Exception Handling #revise
- and returning error as JSON instead of typical "Internal Server error", etc.
- Ex : Student Id entered does not exist
##### Development Process
1. Create a custom error response class ( a POJO class) (automatic JSON converting)
```java
public class StudentErrorResponse{
	private int status;
	private String message;
	private long timeStamp;
	// add constructors
	//add getters and setters
}
```

2. Create Custom error response class
```java
public class StudentNotFoundException extends RuntimeException {
	public StudentNotFoundException(String message){
		super(message);
	}
}
```

3. Update REST service to throw exception
`StudentRestController`
```java
@GetMapping("/students/{studentId}")
public Student getStudent(@PathVariable int studentId){
	if( (studentId >= theStudents.size()) || (studentId < 0) ){
		throw new StudentNotFoundException(studentId + " not found");
	}
	return theStudents.get(studentId);
}
```

4. Add exception handler method 
- Define exception handler methods with @ExceptionHandler
- Exception handler will return a ResponseEntity
- ResponseEntity provides control to specify HTTP status code, HTTP headers and response body.
`StudentRestController`
```java
@ExceptionHandler
public ResponseEntity<StudentErrorResponse> handleException(StudentNotFoundException exc){
	StudentErrorResponse error = new StudentErrorResponse();
	
	error.setStatus(HttpStatus.NOT_FOUND.value());
	error.setMessage(exc.getMessage());
	error.setTimeStamp(System.currentTimeMillis());
	
	return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
}
```

#### Global Exception Handling
- Exception handler is only used for specific REST controllers
- Cannot be used globally

- Spring @ControllerAdvice similar to an interceptor/filer is used for global exception handling

##### Development Process
1. Create new @ControllerAdvice 
2. Remove previous exception handling i.e. `handleException` method from StudentRestController and add it to StudentRestExceptionHandler
```java
@ControllerAdvice
public class StudentRestExceptionHandler{
	@ExceptionHandler
	public ResponseEntity<StudentErrorResponse> handleException(StudentNotFoundException exc){
		StudentErrorResponse error = new StudentErrorResponse();
	
		error.setStatus(HttpStatus.NOT_FOUND.value());
		error.setMessage(exc.getMessage());
		error.setTimeStamp(System.currentTimeMillis());
	
		return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
	}
}
```

### API Design Conventions :

| **HTTP method** | Endpoint               | Tasks/ Action            |
| --------------- | ---------------------- | ------------------------ |
| POST            | /api/employees         | Create a new employee    |
| GET             | /api/employees         | Read all employees       |
| GET             | /api/employees/{empId} | Read Individual Employee |
| PUT             | /api/employees         | Update existing employee |
| DELETE          | /api/employees/{empId} | Delete Existing employee |

### Real life example of REST API with spring boot that connects to a database
**Application Architecture :** 
![[Screenshot 2024-10-14 141410.png]]
(EmployeeDAO instead of 2nd Employee REST Controller)

1. Setup database table and load Sample data 
```
Employee
> id INT
> first_name VARCHAR
> last_name VARCHAR
> email VARCHAR
```

2. Create DAO Interface : EmployeeDAO
3. DAO Implementation : EmployeeDAOJpaImpl implements EmployeeDAO 
- Similar to what we have done earlier

#### Service Layer
- follows service facade design pattern
- Intermediate layer for custom business logic
- Integrate data from multiple sources (DAO, Repositories)
- ![[Screenshot 2024-10-14 142419.png]]
- @Service is used for service layer use 
- @Service is a implementation of @Component and works with component scanning
- Spring will automatically register the service implementation, thanks to component scanning

##### Service layer implementation process :
1. Define Service Interface
```java
public interface EmployeeService{
	List<Employee> findALl();
}
```

2. Define Service Implementation and Inject EmployeeDAO
```java
@Service 
public class EmployeeServiceImpl implements EmployeeService {
	// inject EmployeeDAO
	EmployeeDAO employeeDAO = new EmployeeDAO();
	@Autowired
	public EmployeeService(EmployeeDAO employeeDAO){
		this.employeeDAO = employeeDAO;
	}

	@Override
	public List<Employee> findAll(){
		return employeeDAO.findAll();
	}
}
```

> [!tip]
> You Should add transactional boundaries to service layer instead of DAO implementations 
> So add @Transactional if required on service methods and remove from DAO methods 

### Spring Data JPA in Spring Boot
- If we need multiple DAO's for like Customer, Student, Product, Book, etc. 
- to avoid writing the same code for DAO interfaces and implementations for all the entities

What we want spring to do :
- create DAO for me
- Plugin entity type and primary key
- give basic CRUD features

Solution is Spring data JPA, it helps minimize boiler plate DAO code
##### Steps : 
1. Extend JpaRepository interface
```java
public interface EmployeeRepository extends JpaRepository<Employee, Integer>{
	// no code is required here 
}
```
Employee -> Entity Type
Integer -> Primary Key

we get these methods automatically : 
`findAll(), findById(), save(), findReferenceById(), deleteById(),` etc.
- More in JpaRepository docs

2. Use your Repository in your app : 
```java
@Service 
public class EmployeeServiceImpl implements EmployeeService{
	private EmployeeRepository employeeRepository;

	@Autowired
	public EmployeeServiceImpl(EmployeeRepository theEmployeeRepository){
		employeeRepository = theEmployeeRepository;
	}

	@Override
	public List<Employee> findAll(){
		return employeeRepository.findAll();
	}
	... // add other methods as you need
}
```
##### Advanced features of Spring Data JPA : 
- Extending and adding custom queries with JPQL
- Query Domain Specific Language (Query DSL)
- Defining Custom Methods (Low level coding)

### Spring Data REST in Spring Boot
What we want spring to do ? 
- Create a REST API for me
- Use existing JpaRepository(entity, primary key) 
- give all the basic REST API crud features automatically
Solution -> Spring Data REST

- By default Spring Data REST will create endpoints based on entity type : 
- Example : 
	- if entity is Employee
	- endpoint will be /employee==**s**==

##### Development steps :
1. Add spring data REST to maven POM file
```xml
<dependency>
	<groupId>org.springframework.book</groupId>
	<artifactId>spring-boot-starter-data-rest</artifactId>
</dependency>
```
- That's it

##### In a nutshell for spring data REST you need :
1. Your Entity : Employee
2. JpaRepository : EmployeeRepository extends JpaRepository
3. Maven POM dependency

### HATEOS
- Spring REST endpoints are HATEOS compliant 
- HATEOS : Hypermedia As Engine Of Application State
- i.e. along with JSON data we get response meta-data information about the page

- To specify different endpoints (instead of employees i.e. entity named) :
```java
@RepositoryRestResource(path = "members")
public interface EmployeeRepository extends JpaRepository<Employee, Integer>{

}
```

### Other Configurations for page : 
`spring.data.rest.base-path = /magic-api`
`spring.data.rest.default-page-size = 50` // default is 20

Sorting properties : 
localhost:8080/employees?sort=lastName
localhost:8080/employees?sort=firstName,desc
localhost:8080/employees?sort=lastName,firstName,asc


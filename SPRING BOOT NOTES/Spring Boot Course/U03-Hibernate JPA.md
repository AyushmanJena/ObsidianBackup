
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

| **Java Class**     |               | **Database Table**     |
| ------------------ | ------------- | ---------------------- |
| id : int           |               | id INT                 |
| firstName : String | **Hibernate** | first_name VARCHAR(45) |
| lastName : String  |               | last_name VARCHAR(45)  |
| email : String     |               | email VARCHAR(45)      |

## JPA
Jakarta Persistence  API / Java Persistence API
- JPA is like a bridge between the Spring Application models and the relational database that is used for managing and accessing the data between the objects in application and database.
- Standard API for ORM
- Only a specification 
	- defines a set of interfaces
	- Requires an implementation to be usable
	JPA  : 
		⌊ Hibernate
		⌊ EclipseLink

- Hibernate or JPA uses JDBC for all database communications
- Hikari : Hikari is a JDBC datasource implementation that provides a connection pooling mechanism.

notes : 
to turn off spring boot banner in console: 
> 	`spring.main.banner-mode = off`

to reduce the logging level in console : 
> 	`logging.level.root = warn`

### Entity Manager
- Entity manager is an interface in JPA
- It is the main component for creating queries, etc.
- Based on configs, spring boot will automatically create the beans: 
	- Datasource, EntityManager, ...
	- you can then inject these into you app, ex: DAO
	DAO -> Data Access Object
	- It is a design Pattern that separates the data access logic from the business logic in an application.

#### Spring Boot - Auto Configuration of Data Source : 
Dependencies to add : 
- MySQL Driver
- Spring Data JPA

`application.properties`
```properties
spring.datasource.url = jdbc:mysql://localhost:3306/student_tracker 
spring.datasource.username = springstudent  
spring.datasource.password = springstudent
```
OR if it does not work : 
```properties
spring.datasource.driverClassName = com.mysql.cj.jdbc.Driver
spring.datasource.url = jdbc:mysql://127.0.0.1:3306/student_tracker
spring.datasource.username = springstudent
spring.datasource.password = springstudent
spring.jpa.database-platform = org.hibernate.dialect.MySQLDialect
```


### Creating Command Line App : 
In main java application :
```java
@Bean
public CommandLineRunner commandLineRunner(String[] args){
	return runner -> {
		System.out.println("Hello World");
	};
}
```
- When the spring boot application starts up, it will detect any beans that implement the CommandLineRunner interface and automatically invoke their run method.

### Entity Class
- Java class that is mapped to a database
		ex : Student
- The entity class must have `@Entity`
- Public or protected no argument constructor
- The class can also have other constructors
#### Steps: 
1. Map class to database table
2. Map fields to database columns 
```java
@Entity
@Table(name="student") // must match table name in the database
public class Student{
	@Id
	@Column(name="id") // must match field name in the table
	private int id;

	@Column(name="first_name")
	private String firstName;
}
```

- @Column is optional, if not specified => column name and java field name are same. 
- But this approach is not recommended.

#### For Auto incrementing primary key : 
```
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
@Column(name="id")
private int id
```

Other Generation strategies are : AUTO, SEQUENCE, TABLE, UUID
##### You can even define your own strategy : 
> [!NOTE]
> 1. Create implementation of org.hibernate.id.IdentifierGenerator
> 2. Override the method : public Serializable generate(...)

## CRUD
Create Read Update Delete
- DAO -> Responsible for interacting with the database
![[Screenshot 2024-10-09 165824.png]]
- JPA Repository also provides similar functionalities like Entitymanager but : 
	- Entity manager is used for 'low level control and flexibility'
	- JPA repository is used when you want 'higher level of abstraction'

### CREATING OBJECTS
#### STEPS : 
##### 1. Define DAO Interface
```java
public interface StudentDAO{
	void save(Student theStudent); // Student is the entity class
}
```

##### 2. Define DAO Implementations
```java
@Repository // for component scanning
public class StudentDAOImpl implements StudentDAO{
	private EntityManager entityManager;
	
	@Autowired
	public StudentDAOImpl(EntityManager entityManager){ // Injecting entity manager
		this.entityManager = entityManager;
	}

	@Override
	@Transactional 
	public void save(Student theStudent){
		entityManager.persist(theStudent); // to save the object
	}
}
```
- @Transactional is used when changes are made to database object and are to be saved.
- It automatically begins and ends transaction for JPA code
- Not required for read and query operations.

- Spring will automatically register the DAO implementation due to component scanning. 

##### 3. Update the main app
```java
@SpringBootApplication
public class CruddemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(CruddemoApplication.class, args);
	}
	@Bean
	public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
		return runner -> {
			createStudent(studentDAO);
		};
	}
	private void createStudent(StudentDAO studentDAO) {
		// create the student object
		Student tempStudent = new Student("Paul", "Doe", "paul@luv2code.com");
		// save the student object
		studentDAO.save(tempStudent);
		// display id of the saved student
		System.out.println("Saved student. Generated id: " + tempStudent.getId());
	}
}
```

### READING OBJECTS
- retrieving java object using the primary key
#### STEPS
1. Add new Method to DAO Interface
```java
public interface StudentDAO{
	Student findById(Integer id); // Student is the entity class
}
```

2. Add new method to DAO Implementation 
```java
@Repository // for component scanning
public class StudentDAOImpl implements StudentDAO{
	...
	@Override
	public Student findById(Integer id){
		return entityManager.find(Student.class, id);
	}
	...
}
```
- @Transactional is not needed for operation which just reads data

3. Update Main app 
```java
@SpringBootApplication
public class CruddemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(CruddemoApplication.class, args);
	}
	@Bean
	public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
		return runner -> {
			createStudent(studentDAO);
			readStudent(studentDAO);
		};
	}
	private void readStudent(StudentDAO studentDAO) {
		// retrieve student based on the id: primary key
		Student myStudent = studentDAO.findById(1);
		System.out.println("Found the student: " + myStudent);
	}
}
```

### QUERY OBJECTS
- Using JPQL : JPA Query Language
- JPQL is based on entity name and entity fields but use concepts similar to SQL 
##### Retrieving all Students : 
```
TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student", Student.class);
List<Student> students = theQuery.getResultList();
```
- Student -> Name of JPA entity, i.e. the class name ( not the name of database table)
##### Retrieving students with lastName = 'Doe' : 
```
TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student WHERE lastName='Doe'", Student.class);
```
##### Similarly : 
`"FROM Student WHERE lastName='Das' OR firstName='Tom'"`
`"FROM Student WHERE email LIKE '%gmail.com'"`
##### Named Parameters : (TO REMOVE HARDCODED VALUES)
```
TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student WHERE lastName=:theData", Student.class);
theQuery.setParameter("theData", theLastName);
return theQuery.getResultList();
```
:theData -> the placeholder that will be replaced with another value later
##### select clause
("select s FROM Student s", Student.class)
	- s is an identification variable 
	- it provides reference to Student entity object 
	- Useful for complex queries 
	- Ex : 
`"select s FROM Student s WHERE s.email LIKE '%gmail.com'"`
`"select s FROM Student s WHERE s.lastName = :Data"`

#### STEPS
1. Add new Method to DAO Interface
```java
public interface StudentDAO{
	List<Student> findAll(); // Student is the entity class
}
```

2. Add new method to DAO Implementation 
```java
@Repository // for component scanning
public class StudentDAOImpl implements StudentDAO{
	...
	@Override
	public List<Student> findAll(){
		TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student", Student.class);
		return theQuery.getResultList();
	}
	...
}
```

3. Update the Main App
```java
@SpringBootApplication
public class CruddemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(CruddemoApplication.class, args);
	}
	@Bean
	public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
		return runner -> {
			queryForStudents(studentDAO);
		};
	}
	private void queryForStudents(StudentDAO studentDAO) {
		// get list of students
		List<Students> theStudents = studentDAO.findAll();

		// display the list of all students
		for(Student tempStudent : theStudents){
			System.out.println(tempStudent);
		}
	}
}
```

### UPDATE AN STUDENT
entityManager.merge(theStudent);
#### Steps :
1. Add new Method to DAO Interface
```java
public interface StudentDAO{
	void update(Student theStudent); // Student is the entity class
}
```

2. Add new method to DAO Implementation 
```java
@Repository // for component scanning
public class StudentDAOImpl implements StudentDAO{
	...
	@Override
	@Transactional // adding transactional since we are updating data
	public void update(Student theStudent){
		entityManager.merge(theStudent);
	}
	...
}
```


3. Update the Main App
```java
@SpringBootApplication
public class CruddemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(CruddemoApplication.class, args);
	}
	@Bean
	public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
		return runner -> {
			updateStudent(studentDAO);
		};
	}
	private void updateStudents(StudentDAO studentDAO) {
		// retrieve student based on primary key i.e. ID
		int studentId = 1;
		Student myStudent = studentDAO.findById(studentId);

		// change first Name to "Tommy"
		myStudent.setFirstName("Tommy")
		studentDAO.update(myStudent);
	}
}
```


### DELETE AN STUDENT
entityManager.remove(theStudent);

- Delete based on condition using JPQL
`int numRowsDeleted = entityManager.createQuery("DELETE FROM Student WHERE lastName = 'Ramachandra'").executeUpdate();`
- Returns number of rows deleted  (stored in numRowsDeleted)

- Delete All Students
`int numRowsDeleted = entityManager.createQuery("DELETE FROM Student").executeUpdate();`

> [!note]
> Use executeUpdate() when you want to perform data manipulation operation that does not return a result list
#### Steps :
1. Add new Method to DAO Interface
```java
public interface StudentDAO{
	void delete(Integer id); 
}
```

2. Add new method to DAO Implementation 
```java
@Repository // for component scanning
public class StudentDAOImpl implements StudentDAO{
	...
	@Override
	@Transactional // adding transactional since we are updating data
	public void delete(Integer id){
		Student theStudent = entityManager.find(Student.class, id);
		entityManager.remove(theStudent);
	}
	...
}
```

3. Update the Main App
```java
@SpringBootApplication
public class CruddemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(CruddemoApplication.class, args);
	}
	@Bean
	public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
		return runner -> {
			deleteStudent(studentDAO);
		};
	}
	private void deleteStudent(StudentDAO studentDAO) {
		int studentId = 3;
		studentDAO.delete(studentId);
	}
}
```

### Creating Database Tables from Java Code
- Automatically create database tables
- Useful for development and testing

Hibernate will automatically generate table creation SQL code and execute it

`application.properties`
```properties
spring.jpa.hibernate.ddl-auto = PROPERTY-VALUE
```

| property-value | Description                                            |
| -------------- | ------------------------------------------------------ |
| none           |                                                        |
| create-only    | creates the table only                                 |
| drop           | drops database tables                                  |
| create         | database tables are dropped followed by table creation |
| create-drop    | same as create, but on app shutdown, drop db tables    |
| validate       | validate the database tables schema                    |
| update         | updates the existing tables                            |


> [!WARNING]
> When database tables are dropped, all data is LOST

Note : 
SQL scripts are preferred over auto generation 
	- can be used for governance and code review
	- Customize and fine tune for complex database designs
	- can be version controlled
	- can be used in case of needed migration 
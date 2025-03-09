Dependency : 
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-mongodb</artifactId>  
</dependency>
```

add configuration to application.properties : 
```properties
# mongodb config  
spring.data.mongodb.host=localhost  
spring.data.mongodb.port=27017  
spring.data.mongodb.database=journaldb
# if auth is there add username and password
spring.data.mongodb.username=user
spring.data.mongodb.password=pass
```

Model structure : 
use `@Document(collection = "collection-name")` from `org.springframework.data.mongodb.core.mapping.Document;`
```java
@Document(collection = "students")  
public class Student {  
	@Id
    private int id;  
    private String name;  
    private String city;  
    private String college;  
	// add constructors, getters and setters as required.
}
```

MongoRepository : 
Similar to JPA repository in MySQL, in MongoDb we have MongoRepository
to reduce code for basic functions
syntax : 
`public interface RepositoryName extends MongoRepository<Entity, Id>{ }`
```java
public interface StudentRepository extends MongoRepository<Student, Integer> {  
  
}
```

Controller code : 
```java
@RestController  
@RequestMapping("/student")  
public class MyController {  
  
    @Autowired  
    private StudentRepository studentRepository;  
  
    @PostMapping("/")  
    public ResponseEntity<?> addStudent(@RequestBody Student student){  
        Student save = this.studentRepository.save(student);  
        return ResponseEntity.ok(save);  
    }  
  
    @GetMapping("/")  
    public ResponseEntity<?> getStudent(){  
        return ResponseEntity.ok(this.studentRepository.findAll());  
    }  
}
```
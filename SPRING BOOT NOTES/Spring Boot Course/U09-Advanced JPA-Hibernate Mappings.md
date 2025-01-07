---
lastSync: Thu Oct 24 2024 22:19:22 GMT+0530 (India Standard Time)
---
**Multiple Tables, Relationships between tables** 
One-to-One
One-to-Many
Many-to-One
Many-to-Many

### Database Concepts 
##### Primary Key : 
- Identifies a unique row in a table

##### Foreign Key : 
- Link tables together
- A field in one table that refers to primary key in another table

##### Cascade : 
- Apply the same operation to the related entities
- If we delete an instructor, we should also delete their instructor_detail
- Cascade depends on the use case
- If we delete a student we should not delete related course or vise-versa
Cascade Types : 
- **PERSIST** : If entity is persisted / saved, related entity will also be persisted
- **REMOVE** : If entity is removed / deleted, related entity will also be deleted
- **REFRESH** : If entity is refreshed, related entity will also be refreshed
- **DETACH** : If entity is detached (not associated w/ session), then related entity will also be detached
- **MERGE** : If entity is merged, then related entity will also be merged
- **ALL** : All of above cascade types

##### Fetch Types : 
- What to retrieve
- EAGER -> will retrieve everything (performance nightmare in cases)
- LAZY -> will retrieve only on request (preferred)
Default Fetch types : 
**@OneToOne** : FetchType.EAGER
**@OneToMany** : FetchType.LAZY
**@ManyToOne** : FetchType.EAGER
**@ManyToMany** : FetchType.LAZY
- can be overridden by `@ManyToOne(fetch = FetchType.LAZY)`
- Retrieving requires an open Hibernate session i.e. connection to database
- if the hibernate session is closed and you attempt to retrieve lazy data, hibernate will throw an exception

##### Unidirectional relations :
Instructor ----> Instructor Detail
##### Bidirectional relations :
Instructor <---> Instructor Detail

### One-to-One Mapping
```
instructor                                instructor_detail
-----------                               --------------------
id INT                                    id INT
first_name VARCHAR(45)                    youtube_channel VARCHAR(128)
last_name VARCHAR(45)      -------------- hobby VARCHAR(45)
email VARCHAR(45)                          
instructor_detail_id INT                  
```
- An instructor can have a single instructor-detail
#### Uni Directional : Instructor ----> Instructor Detail
##### Development Process : 
1. Define Database Tables 
	with foreign key :
```sql
CREATE TABLE `instructor` (
	`id` int(11) NOT NULL AUTO_INCREMENT,
	`first_name` varchar(45) DEFAULT NULL,
	`last_name` varchar(45) DEFAULT NULL,
	`email` varchar(45) DEFAULT NULL,
	`instructor_detail_id` int(11) DEFAULT NULL,
	PRIMARY KEY (`id`)
	
	CONSTRAINT 'FK_DETAIL' FOREIGN KEY('instructor_detail_id')
	REFERENCES 'instructor_detail'('id')
);
```

3. Create InstructorDetail Class
```java
@Entity
@Table(name="instructor_detail")
public class InstructorDetail {
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	
	@Column(name="youtube_channel")
	private String youtubeChannel;
	
	@Column(name="hobby")
	private String hobby;
	
	// constructors and getters / setters
}
```
4. Create Instructor Class
```java
@Entity
@Table(name="instructor")
public class Instructor {
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;
	@Column(name="first_name")
	private String firstName;
	@Column(name="last_name")
	private String lastName;
	@Column(name="email")
	private String email;
	

	@OneToOne(cascade=CascadeType.ALL)
	@JoinColumn(name=“instructor_detail_id")
	private InstructorDetail instructorDetail;

	// constructors, getters / setters
}
```
- By default no operations are cascaded
- Configure multiple cascade types : 
`@OneToOne(cascade={CascadeType.DETACH,CascadeType.MERGE,CascadeType.PERSIST,CascadeType.REFRESH,CascadeType.REMOVE})`

5. Update Main app
- Define DAO interface method 
- Define DAO implementation
```java
@Override
@Transactional
public void save(Instructor theInstructor) {
entityManager.persist(theInstructor);
}
```
- Update main app
```java
private void createInstructor(AppDAO appDAO) {
	// create the instructor
	Instructor tempInstructor =
	new Instructor("Chad", "Darby", "darby@luv2code.com");
	// create the instructor detail
	InstructorDetail tempInstructorDetail =
	new InstructorDetail(
	"http://www.luv2code.com/youtube",
	"Luv 2 code!!!");
	// associate the objects
	tempInstructor.setInstructorDetail(tempInstructorDetail);
	// save the instructor
	System.out.println("Saving instructor: " + tempInstructor);
	appDAO.save(tempInstructor);
	System.out.println("Done!");
}
```
save()  -> will also save the instructor-detail object due to cascade  

#### One-to-One Bi Directional 
When you want to have cascade but when you delete instructor detail you dont want to delete the instructor 
`InstructorDetail.java`
```java
@OneToOne(mappedBy = "instructorDetail", 
cascade = {CascadeType.DETACH, CascadeType.MERGE, CascadeType.PERSIST, CascadeType.REFRESH}) 
private Instructor instructor;
```
i.e. add every cascade type except remove

Later remove the bidirectional link in the delete function : 
`AppDAOImpl.java`
```java
@Override
@Transactional
public void deleteInstructorDetailsById(int theId){
	// retrieve instructor detail
	InstructorDetail tempInstructorDetail = entityManager.find(InstructorDetail.class, theId);

	// remove the associated object reference
	// break bi-directional link
	// updates instructor id reference in instructor to null
	tempInstructorDetail.getInstructor().setInstructorDetail(null);

	// delete the instructor detail
	entityManager.remove(tempInstructorDetail);
}
```


### One-to-Many Bi-directional Mapping
- An instructor can have many courses
- Similarly **One-to-Many** : Many Courses can have one instructor

Casading requirements : 
- If you delete an instructor, DO NOT delete the courses
- If you delete a course, DO NOT delete the instructor i.e. do not apply cascading deletes

##### Development process :
1. Define database tables 
```sql
CREATE TABLE `course` (
	`id` int(11) NOT NULL AUTO_INCREMENT,
	`title` varchar(128) DEFAULT NULL,
	`instructor_id` int(11) DEFAULT NULL,
	PRIMARY KEY (`id`),
	UNIQUE KEY `TITLE_UNIQUE` (`title`), -- prevent duplicate course titles
	...
);

CREATE TABLE `course` (
	…
	KEY `FK_INSTRUCTOR_idx` (`instructor_id`),
	CONSTRAINT `FK_INSTRUCTOR`
	FOREIGN KEY (`instructor_id`)
	REFERENCES `instructor` (`id`)
	…
);
```
2. Create Course class
```java
@Entity
@Table(name="course")
public class Course {
	…
	@ManyToOne
	@JoinColumn(name="instructor_id")
	private Instructor instructor;
	…
	// constructors, getters / setters
}
```

3. Update Instructor class
```java
@Entity
@Table(name="instructor")
public class Instructor {
	…
	@OneToMany(mappedBy="instructor", fetch = FetchType.LAZY, 
	cascade={CascadeType.PERSIST, CascadeType.MERGE CascadeType.DETACH, CascadeType.REFRESH}) // all types except DELETE
	private List<Course> courses;
	
	public List<Course> getCourses() {
		return courses;
	}
	public void setCourses(List<Course> courses) {
		this.courses = courses;
	}

	//Add Convenience methods for bi-directional
	public void add(Course tempCourse){
		if(courses == null){
			courses = new ArrayList<>();
		}
		courses.add(tempCourse);
		tempCourse.setInstructor(this);
	}
	…
}
```
mappedBy :
- tells hibernate tolook at the instructor property in Course class
- Use information from Course class @JoinColumn
- To help find associated courses for instructor

6. Update Main app
with EAGER 
`AppDAOImpl.java`
```java
@Override
public List<Course> findCoursesByInstructorId(int theId) {
	// create query
	TypedQuery<Course> query = entityManager.createQuery("from Course where instructor.id = :data", Course.class);
	query.setParameter("data", theId);
	// execute query
	List<Course> courses = query.getResultList();
	return courses;
}
```
`MainApplication.java`
```java
private void findCoursesForInstructor(AppDAO appDAO) {
	int theId = 1;
	// find the instructor
	Instructor tempInstructor = appDAO.findInstructorById(theId);
	System.out.println("tempInstructor: " + tempInstructor);
	// find courses for instructor
	List<Course> courses = appDAO.findCoursesByInstructorId(theId);
	// associate the objects
	tempInstructor.setCourses(courses);
	System.out.println("the associated courses: " + tempInstructor.getCourses());
}
```

## OR
with LAZY :
`AppDAOImpl.java`
```java
@Override
public Instructor findInstructorByIdJoinFetch(int theId) {
	// create query
	TypedQuery<Instructor> query = entityManager.createQuery(
								"select i from Instructor i "
								+ "JOIN FETCH i.courses "
								+ "where i.id = :data", Instructor.class);
	query.setParameter("data", theId);
	// execute query
	Instructor instructor = query.getSingleResult();
	return instructor;
}
```
- Even with LAZY, the code will still retrieve Instructor and Courses.
- This JOIN FETCH is similar to EAGER loading

`MainApplication.java`
```java
private void findCoursesForInstructor(AppDAO appDAO) {
	int theId = 1;
	// find the instructor
	Instructor tempInstructor = appDAO.findInstructorById(theId);
	System.out.println("tempInstructor: " + tempInstructor);
	// find courses for instructor
	List<Course> courses = appDAO.findCoursesByInstructorId(theId);
	// associate the objects
	tempInstructor.setCourses(courses);
	System.out.println("the associated courses: " + tempInstructor.getCourses());
}
```

#### OneToMany : Update Instructor :
`AppDAOImpl.java`
```java
@Override
@Transactional
public void update(Instructor tempInstructor) {
	entityManager.merge(tempInstructor);
}
```
MainApp.java
```java
private void updateInstructor(AppDAO appDAO) {
	int theId = 1;
	System.out.println("Finding instructor id: " + theId);
	Instructor tempInstructor = appDAO.findInstructorById(theId);
	System.out.println("Updating instructor id: " + theId);
	tempInstructor.setLastName("TESTER");
	appDAO.update(tempInstructor);
	System.out.println("Done");
}
```
#### OneToMany : Update Course :
`AppDAOImpl.java`
```java
@Override
@Transactional
public void update(Course tempCourse) {
	entityManager.merge(tempCourse);
}
```
`MainApp.java`
```java
private void updateCourse(AppDAO appDAO) {
	int theId = 10;
	System.out.println("Finding course id: " + theId);
	Course tempCourse = appDAO.findCourseById(theId);
	System.out.println("Updating course id: " + theId);
	tempCourse.setTitle("Enjoy the Simple Things");
	appDAO.update(tempCourse);
	System.out.println("Done");
}
```

#### OneToMany : Delete Instructor :
`AppDAOImpl.java`
```java
@Override
@Transactional
public void deleteInstructorById(int theId) {
	// retrieve the instructor
	Instructor tempInstructor = entityManager.find(Instructor.class, theId);
	List<Course> courses = tempInstructor.getCourses();
	// break associations of all courses for instructor
	for (Course tempCourse : courses) {
		tempCourse.setInstructor(null);
	}
	// delete the instructor
	entityManager.remove(tempInstructor);
}
```
- If you do not remove instructor from courses : constraint violations
`MainApp.java`
```java
private void deleteInstructor(AppDAO appDAO) {
	int theId = 1;
	System.out.println("Deleting instructor id: " + theId);
	appDAO.deleteInstructorById(theId);
	System.out.println("Done!");
}
```

#### OneToMany : Delete Course :
`AppDAOImpl.java`
```java
@Override
@Transactional
public void deleteCourseById(int theId) {
	// retrieve the course
	Course tempCourse = entityManager.find(Course.class, theId);
	// delete the course
	entityManager.remove(tempCourse);
}
```
MainApp.java
```java
private void deleteCourseById(AppDAO appDAO) {
	int theId = 10;
	System.out.println("Deleting course id: " + theId);
	appDAO.deleteCourseById(theId);
	System.out.println("Done!");
}
```

### One-to-Many Uni-Directional Mapping :
- A course can have many reviews, 
- deleting a course deletes all reviews but deleting a review does not delete the course

##### Development Process
1. Define Database table review:
```java
CREATE TABLE `review` (
	`id` int(11) NOT NULL AUTO_INCREMENT,
	`comment` varchar(256) DEFAULT NULL,
	`course_id` int(11) DEFAULT NULL,

	KEY `FK_COURSE_ID_idx` (`course_id`),
	CONSTRAINT `FK_COURSE`
	FOREIGN KEY (`course_id`)
	REFERENCES `course` (`id`)
);
```
course -> referenced table and id-> referenced column

2. Define Review entity class : 
`Review.java`
```java
@Entity
@Table(name = "review")
public class Review{
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name = "id")
	private int id;

	@Column(name = "comment")
	private String comment;

	// add constructors, getters and setters
}
```

3. Update Course to reference reviews
`Course.java`
```java
...
@OneToMany(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
@JoinColumn(name = "course_id") // refers to course_id in review table
private List<Review> reviews;

// Convinience method for adding new reviews
public void add(Review tempReview){
	if(reviews == null){
		reviews = new ArrayList<>();
	}
	reviews.add(tempReview);
}

```

### Many-to-Many Mapping
- A course can have many students
- A student can have many courses
- Arises a need to track which student is in which course and vice versa
- so we use a third table : Join Table

Join Table :  A table that provides a mapping between two tables
- It has foreign keys for each table to define the mapping relationship
```
course_student
course_id INT(11)
student_id INT(11)
```

##### Development Process
1. Define Database tables :
```sql
CREATE TABLE `course_student` (
	`course_id` int(11) NOT NULL,
	`student_id` int(11) NOT NULL,
	PRIMARY KEY (`course_id`, `student_id`),

	CONSTRAINT `FK_COURSE_05`
	FOREIGN KEY (`course_id`)
	REFERENCES `course` (`id`),
	
	CONSTRAINT `FK_STUDENT`
	FOREIGN KEY (`student_id`)
	REFERENCES `student` (`id`)
);
```

2. Update Course Entity class to reference students
`Course.java`
```java
@ManyToMany
@JoinTable(name="course_student",
			joinColumns=@JoinColumn(name="course_id"),
			inverseJoinColumns=@JoinColumn(name="student_id"))
private List<Student> students;

```

joinColumns to reference course_id in course_student from the same class i.e. Course
inverseJoinColumns to reference student_id in course_student from the other class i.e. student

-> The Student class is on the other side.. so it is considered the inverse

3. Similarly update Student to reference courses
Student.java
```java
@ManyToMany
@JoinTable( name="course_student",
			joinColumns=@JoinColumn(name="student_id"),
			inverseJoinColumns=@JoinColumn(name="course_id"))
private List<Course> courses;
```
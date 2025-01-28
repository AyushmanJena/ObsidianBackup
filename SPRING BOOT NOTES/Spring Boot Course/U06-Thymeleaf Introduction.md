---
lastSync: Sun Oct 20 2024 20:31:55 GMT+0530 (India Standard Time)
---
## Thymeleaf
- Java templating Engine
- used to generate HTML views for web pages
- Can be used outside of web apps as well
- Can access Java code, objects, spring beans.
- Thymeleaf is processed on the server side and results are included in the HTML returned to the browser


> [!warning]
>You cannot return webpage with @RestController. Use @Controller

##### Development Process :
1. Add Thymeleaf dependencies to Maven POM file 
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-thymeleaf</artifactId>  
</dependency>
```

2. Develop spring MVC controller
`DemoController.java`
```java
@Controller
public class DemoController {
	@GetMapping("/hello")
	public String sayHello(Model theModel){
		theModel.addAttribute("theDate", java.time.LocalDateTime.now());
		return "helloworld"; // returns the template name
	}
}
```

3. Create Thymeleaf template :
`helloworld.html`
```html
<!DOCTYPE HTML>  
<html xmlns:th="http://www.thymeleaf.org">  
    <head>  
        <title>Thymeleaf Demo</title>  
        <!--  reference css file -->  
        <link rel="stylesheet" th:href="@{/css/demo.css}"/>
    </head>  
  
    <body>  
        <p th:text="'Time on the server is' + ${theDate}" class = "funny"/>  
    </body>  
</html>
```
Note : `${theDate}` must match with the Model attribute name `"theDate"

##### Additional features we get with Thymeleaf : 
- Looping and conditionals
- CSS and JS integration
- Template layouts and fragments

#### Referencing CSS file in thymeleaf :
`<link rel="stylesheet" th:href="@{/css/demo.css}"/>`
demo.css file in resources/static/css

@ symbol represents context path of your application (app root)

 **3rd party css libraries : bootstrap**
`<link rel = "stylesheet" th:href = "@{/css/bookstrap/min.css}"/>`

### Behind the Scenes how templating engine works : 

![[templating engine.png]]

**Model** : A model is a special object that allows us to share information between controllers and view pages (thymeleaf)

Note : Front Controller or DispatcherServlet is already developed by Spring

We will create **MVC -> Model View Controller**

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

#### Reading Form Data from Spring MVC

![[Form Thymeleaf.png]]

##### Codes :
`HelloWorldController.java`
```java
@Controller
public class HelloWorldController{
	// getmapping controller method to show initial form
	@GetMapping("/showForm")
	public String showForm(){
		return "helloworld-form";
	}

	// post mapping controller method to process html form
	@RequestMapping("/processForm")
	public String processForm(){
		return "helloworld";
	}
}
```

Adding data to model and viewing using thymeleaf
`HelloWorldController.java`
```java
@RequestMapping("/processFormVersionTwo")
public String processForm(HttpServletRequest request, Model model){
	// read the request parameter from the HTML form
	String theName = request.getParameter("studentName");

	String result = "hello, " + theName;

	// add message to the model
	model.addAttribute("message", result);

	return "helloworld";
}
```

In html :
`hellloworld.html`
```html
<!DOCTYPE HTML>  
<html xmlns:th="http://www.thymeleaf.org">  
    <head>  
        <title>Thymeleaf Demo</title>  
  
        <!--  reference css file -->  
    </head>  
  
    <body>    

		The message : <span th:text="${message}"/>  
<!--    message is the attribute name from the model-->  
  
    </body>  
</html>
```

`th:text="${message}` must match `"message"` model attribute in controller

### Reading HTML form data with @RequestParam
(instead of using HttpServletRequest)
`HelloWorldController.java`
```java
@RequestMapping("/processForm")
public String processForm(@RequestParam("studentName")String name, Model model){
	String result = "Hello !!!" + name;
	model.addAttribute("message", result);
	return "helloworld";
}
```
 `String theName = request.getParameter("studentName");`  
// ^ this code is not required anymore automatically done by spring

#### @GetMapping and @PostMapping

![[mapping thymeleaf.png]]
##### Using GET method
`<form th:action="@{/processForm}" method = "GET"...>...</form>`
- form data is added to the end of URL 
```java
@GetMapping("/processForm")
public String processForm(){
	...
}
```

@RequestMapping : Supports any HTTP Requests (GET, POST, etc.)
for @GetMapping and @PostMapping other requests will be automatically rejected

else 
`@RequestMapping(path="/processForm", method=RequestMethod.GET)`

Similarly for POST use @PostMapping
but form data is passed in the body of the HTTP request message

Major Differences between GET and POST:
GET : 
- Good for Debugging
- Limitations of data length
POST : 
- Cannot bookmark or email the URL
- No data length limitations
- Can also send binary data


### SPRING MVC form tag
to get user input
###### STEPS :
1. Show form - add model attribute
```java
@GetMapping("/showStudentForm")
public String showForm(Model theModel){
	theModel.addAttribute("student", new Student());
	...
	return "student-form";
}
```

2. Thymeleaf template for the form
`student-form.html`
```html
<form th:action = "@{/processStudentForm}" th:object="${student}" method = "POST">
	Name : <input type = "text" th:field="*{firstName}"/>
	<input type = "submit" value = "SUBMIT"/>
</form>
```
Note  :  model attribute `"student"` must match `th:object="${student}"`
- "{firstName}" is short syntax for ${student.firstName}
- `*{firstName}` automatically calls student.getFirstName
- @{/processStudentForm} -> where the form is submitted

3. Handling form Submission in Controller
```java
@PostMapping("/processStudentForm")
public String processForm(@ModelAttribure("student") Student theStudent){
	System.out.println(theStudent.getName); // log the student name
	return "student-confirmation"; // open confirmation page
}
```

4. Confirmation Page : 
`student-confirmation.html`
```html
<body>
	Student : <span th:text="${student.firstName}"/>
</body>
```

#### Dropdown List (or Radio Buttons)in form using properties file
###### Using  `<select>` **tag hardcoded**
```java
<select th:field="*{country}">
	<option th:value="India">India</option>
	<option th:value="USA">USA</option>
</select>
```
and update student class and confirmation page 

###### Using application.properties
1. `countries = India, USA, Brazil` in application.properties
2. Update student class : add getters, setters for the new property
3. In respective controller do value injection 
```java
	@Value("${countries}")
	private List<String> countries;	
```
4. Update html template by adding looping instead of hardcoding values :
```html
Country : <select th:field="*{country}">
	<option th:each="tempCountry : ${countries}" th:value="${tempCountry}" th:text = "${tempCountry}"/>
</select>
```


### Spring MVC Form Validation :
- Java has a standard Bean validation API
- Defines a metadata model and API for entity validation

##### Validation Annotations :
@NotNull, @Min, @Max, @NotBlank, @Size, @Pattern, @Future/@Past

#### Example :
1. Update Entity class :
`Customer.java`
```java
...
@NotNull(message = "is required")
@Size(min = 1, message = "is required")
private String lastName;
// add getter and setter methods
...
```

2. Add Controller code to show the form
3. In html form add validation support
```html
Last Name : <input type = "text" th:field = "*{lastName}"/>
<span th:if="${#fields.hasError('lastName')}" th:errors = "*{lastName}" class = "error"> </span>
```

4. Perform Validation in Controller class 
```java
@PostMapping("/processForm")
public String processForm(@Valid @ModelAttribute("customer") Customer theCustomer, BindingResult theBindingResult){
	if(theBindingResult.hasErrors()){
		return "customer-form";
	}
	else{
		return "customer-confirmation";
	}
}
```
5. Confirmation HTML page

##### @InitBinder (method annotation)
- Works as a preprocessor
- Preprocess each web request to our controller

Using @InitBinder to trim white spaces :
`CustomerController.java`
```java
@InitBinder
public void initBinder(WebDataBinder dataBinder){
	StringTrimmerEditor st = new StringTrimmerEditor(true);
	dataBinder.registerCustomerEditor(String.class, st);
}
```
- StringTrimmerEditor : removes leading and trailing whitespaces and also trim the string to null if it is entirely whitespace
- Can be used to avoid whitespaces not caught by @NotNull fields
- Parameter true means trim empty spring to null

##### Number Ranges @Min and @Max
```java
@Min(value = 0, message = "cannot be less than 0")
@Max(value = 10, message = "cannot be more than 10")
private int freePasses;
// add getter and setter methods
```

##### Applying Regular Expression Regex validation :
`Customer.java`
```java
...
@Pattern(regexp="^[a-zA-Z0-9]{5}" message = "only 5 chars/digits")
private String postalCode;
// add getter and setter methods
...
```

#### Creating Custom Error Message :
1. Create new file messages.properties in resources
`typeMismatch.customer.freePasses = Invalid Number`
typeMismatch -> error type
customer -> spring model attribute name
freePasses ->field name
Invalid Number -> message

How to know what field to give message for : 
`CustomerController.java->processForm()`
```java
System.out.println(theBindingResult.toString()); // console logs
```

##### Note : 
@NotNull, "is required" for int data gives error if value is passed i.e. automatically an empty string is passed.
So instead of int use Integer

### Custom Validation
Ex : Make sure 'course code' starts with "LUV"
- Custom validation returns boolean value for pass/fail (true or false)

1. Create Custom Validation rule 
a) Create @CourseCode annotation 
`CourseCode.java`
```java
@Constraint(validatedBy=CourseCodeConstraintValidator.class)
@Target({ElementType.METHOD, ElementType.FIELD})
@Retention(RetentionPolity.RUNTIME)
public @interface CourseCode{
	public String value() default "LUV"; // default value
	public String message() default "must start with 'LUV'"; // default err msg
	public Class<?>[] groups() default{}; // define default groups
	public Class<? extends Payload>[] Payload() default{}; // default payloads
}
```
Note : Payloads provide custom details about validation failure(severity level, error code, etc)

b) Create CourseCodeConstraintValidator
`CourseCodeConstraintValidator.java`
```java
public class CourseCodeConstraintValidator implements ConstraintValidator<CourseCode, String>{
	private String cousePrefix;
	
	@Override
	public void initialize(CourseCode theCourseCode){
		coursePrefix = theCourseCode.value();
	}

	@Override
	public boolean isValid(String theCode, ConstraintValidatorContext theConstraintValidatorContext){
		boolean result;
		if(theCode != null){
			result = theCode.startsWith(coursePrefix);
		}
		else{
			result = true;
		}
		return result;
	}
}
```

2. Add Validation rule to customer class : 
```java
@CourseCode(value = "LUV", message = "must start with LUV")
private String CourseCode;
```
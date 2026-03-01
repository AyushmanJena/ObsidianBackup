## Creating a Web UI for Employee Directory : 
https://github.com/darbyluv2code/spring-boot-3-spring-6-hibernate-for-beginners/tree/main/07-spring-boot-spring-mvc-crud

![[Screenshot 2024-10-14 170710.png]]
### Thymeleaf Expressions : 

**To use thymeleaf in your html add this to the code :**
```html
<!DOCTYPE html>  
<html lang="en" xmlns:th="http://www.thymeleaf.org">  
<head>
...
</html>
```

| Expression | Data                                              |
| ---------- | ------------------------------------------------- |
| th:action  | Location to send form data                        |
| th:object  | Reference to model attribute                      |
| th:field   | Bind input field to a property on model attribute |
| more...    |  www.luv2code.com/thymeleaf-create-form           |

> [!NOTE]
> XML Namespace is a unique identifier for XML elements, attributes, etc.
> In this case, we use the XML namespace for Thymeleaf(the "th" : attributes)
> The XML Namespace does not connect to the internet ... 
> Just a unique identifier that is configured in the Thymeleaf JAR file 

### Add new Employee : 
1. Add button for list.employees.html
```html
<a th:href="@{/employees/showFormForAdd}"> Add Employee </a>
```
- @ symbol is reference context path of your application (app root)

Controller Code **to show form** 
```java
@Controller
@RequestMapping("/employees")
public class EmployeeController{
	@GetMapping("showFormForAdd")
	public String showFormForAdd(Model theModel){
		Employee theEmployee = new Employee();
		theModel.addAttribute("employee", theEmployee);
		return "employee/employee-form";
	}
}
```
"employee/employee-form" -> src/main/resources/templates/employees/employee-form.html

2. Create HTML form for new Employee
```html
<form action="#" th:action="@{/employees/save}" th:object="${employee}" method="POST">
	<input type = "text" th:field="*{firstName} placeholder="First Name">
	...
	<button type="submit">Save<button>
</form>
```

> [!note]
> employee in th:object must match with the model attribute name in Controller

3. Process Form data to Save the Employee 
`EmployeeController.java`
```java
...
private EmployeeService employeeService;

@Autowired
public EmployeeController(EmployeeService theEmployeeService){
	employeeService = theEmployeeService;
}

@PostMatpping("/save")
public String saveEmployee(@ModelAttribute("employee") Employee theEmployee){
	// save the employee
	employeeService.save(theEmployee);
	// redirect to home
	return "redirect:/employees/list";
}
...
```

### Homepage/ index
index.html is automatically loaded if no page or path is given
html trick to auto redirect to another url:  replace whole page with :
`index.html`
```html
<meta http-equiv="refresh"  
      content="0; URL='employees/list'">
```


to redirect to another page after submission : 
```java
return "redirect:/employees/list";
```

### Updating Employee Data
- with a given employeeId
1. Add Update button
```html
<tr th:each="tempEmployee : ${employees}">
...
<td>
	<a th:href="@{/employees/showFormForUpdate(employeeId=${tempEmployee.id})}"
	class="btn btn-info btn-sm">Update</a>
</td>
</tr>
```
- this will append the `employeeId` provided to the url and hence will give results according to the id

2. Prepopulating the form : (for update forms)
- We can use the same form 
`employee-form`
```html
<form action="#" th:action="@{/employees/save}"
	th:object="${employee}" method="POST">
	<!-- Add hidden form field to handle update -->
	<input type="hidden" th:field="*{id}" />
	<input type="text" th:field="*{firstName}"
		class="form-control mb-4 w-25" placeholder="First name">
	<input type="text" th:field="*{lastName}"
		class="form-control mb-4 w-25" placeholder="Last name">
	<input type="text" th:field="*{email}"
		class="form-control mb-4 w-25" placeholder="Email">
	<button type="submit" class="btn btn-info col-2">Save</button>
</form>
```
- the hidden field will not be visible to the user but will be returned telling the app which employee data has been modified

3. Controller Code
`EmployeeController`
```java
@GetMapping("/showFormForUpdate")
public String showFormForUpdate(@RequestParam("employeeId") int theId, Model theModel){
	// get the employee from the service
	Employee theEmployee = employeeService.findById(theId);

	// set employee as a model attribute to pre populate the form
	theModel.addAttribute("employee", theEmployee);

	// send over to our form
	return "employees/employee-form";
}
```

4. Process form data to save the employee 
- We will use the same method for saving new employee
`EmployeeController`
```java
@PostMapping("/save")
public String saveEmployee(@ModelAttribute("employee") Employee theEmployee) {
	// save the employee
	employeeService.save(theEmployee);
	// use a redirect to prevent duplicate submissions
	return "redirect:/employees/list";
}
```

### Custom method  in JpaRepository:
`EmployeeRepository.java`
```java
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {  
  
    // that's it ... no need to write any code LOL!  
  
	    // add a custom method to sort by last name    
	    public List<Employee> findAllByOrderByLastNameAsc();  
		  // find all by, order by last name ascending
}
```
`EmployeeServiceImpl.java`
```java
@Override  
public List<Employee> findAll() {  
    return employeeRepository.findAllByOrderByLastNameAsc();  
}
```
Spring Data JPA will parse the method name
Looks for a specific format and pattern and creates appropriate query automatically behind the scenes.

### Delete Employee 
1. Add delete button to the page
```html
<a th:href="@{/employees/delete(employeeId=${tempEmployee.id})}"
class="btn btn-danger btn-sm"
onclick="if (!(confirm('Are you sure you want to delete this employee?'))) return false">Delete</a>
```

2. Add Controller Code
`EmployeeController`
```java
@GetMapping("/delete")
public String delete(@RequestParam("employeeId") int theId) {
	// delete the employee
	employeeService.deleteById(theId);
	// redirect to /employees/list
	return "redirect:/employees/list";
}
```
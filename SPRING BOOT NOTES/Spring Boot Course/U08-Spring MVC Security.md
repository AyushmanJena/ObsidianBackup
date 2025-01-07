---
lastSync: Thu Oct 17 2024 17:36:40 GMT+0530 (India Standard Time)
---
**Complete Spring Security Manual :** 
https://docs.spring.io/spring-security/reference/

MORE ABOUT SPRING SECURITY IN UNIT 5

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org"  
      xmlns:sec="http://www.thymeleaf.org/extras/spring-security">
```

### Spring Security Model : 
- Spring Security defines a framework for security
- Implemented using Servlet filters in the background
- Two methods of securing an app : declarative and programmatic

- Servlet Filters are used to pre-process / post-process web requests
- Servlet Filters can route web requests based on security logic
- Spring provides a bulk of security functionality with servlet filters
- ![[Spring Security Filters.png]]

#### Different Login Methods : 
1. HTTP Basic Authentication
2. Default login form : provided by spring security
3. Custom Login form 

DEPENDENCIES FOR APP WITH THYMELEAF AND SPRING SECURITY : 
```xml
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
<groupId>org.thymeleaf.extras</groupId>
<artifactId>thymeleaf-extras-springsecurity6</artifactId>
</dependency>
```


##### STEPS :
1. Modify spring security configuration :
`DemoSecurityConfig.java`
```java
@Configuration
public class DemoSecurityConfig {
	@Bean
	public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		http.authorizeHttpRequests(configurer ->
					configurer
					.anyRequest().authenticated()
		)
		.formLogin(form ->
				form
					.loginPage("/showMyLoginPage")
					.loginProcessingUrl("/suthenticateTheUser")
					.permitAll()
		);

		return http.build();
	}
}
```

`.anyRequest().authenticated()` : Any request to the app must be authenticated i.e. logged in
`.formLogin(...)` : customizing the form login process
`.loginPage("/showMyLoginPage")` : Show the custom form at request mapping
- Need to create controller for this request mapping "/showMyLoginPage"
`.loginProcessingUrl("/suthenticateTheUser")` : Login form should POST data to this URL for processing
- No Controller Request mapping required for this :)

2. Develop a controller to show the custom login form :
`LoginController.java`
```java
@GetMapping("/showMyLoginPage") // must match .loginPage in security config
public String showMyLoginPage(){
	return "plain-login";
}
```

3. Create Custom login form :
`plain-login.html`
```html
...
<form action = "#" th:action = "@{/authenticateTheUser}" method = "POST">
	Username : <input type = "text" name="username">
	Password : <input type = "password" name="password">
	<input type = "submit" value = "Login">
</form>
```
- th:action must match .loginProcessingUrl() in security config
- Spring security defines default names for login form fields username and password, and the names of input must match 

### Failed Login handling
- When login fails, by default spring security will send user back to your login page
- Append an error parameter : ?error
`http://localhost:8080/myapp/showMyLoginPage?error`

**Handling it and displaying the message :** 
Check the error parameter and if error exists, show an error message

```html
<form...>
	<div th:if="${param.error}">
		<i>Sorry! You entered invalid username/password.</i>
	</div>
</form>
```

### Spring Security Logging out : 
##### STEPS :
1. Add logout support to spring security configuration
`DemoSecurityConfig.java`
```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.authorizeHttpRequests(configurer ->
                    configurer
                            .anyRequest().authenticated()
            )
            .formLogin(form ->
                    form
                            .loginPage("/showMyLoginPage")
                            .loginProcessingUrl("/authenticateTheUser")
                            .permitAll()
            )
   .logout(logout -> logout.permitAll() // add this for logout support 
   );
    return http.build();
}
```
- Logout support for default URL /logout

2. Add Logout button
- Send data to default logout URL : /logout
- Logout url will be handled by spring security filters
- You get it for free ... no coding required
```java
<form action="#" th:action="@{/logout}" method="POST">
	<input type="submit" value = "Logout"/>
</form>
```
- Must use POST method
- Spring will append a logout parameter : ?logout

3. Update the login form to display "logged out" message
```java
<div th:if="${param.logout}">
	<i>You have been logged out.</i>
</div>
```
- If logout param then show message

### Displaying User Id and Roles in web page
add spring-security extras support
```html
<html lang="en" xmlns:th="http://www.thymeleaf.org"  
      xmlns:sec="http://www.thymeleaf.org/extras/spring-security">
```


```html
User : <span sec:authentication="principal.username"></span>
Role(s) : <span sec:authentication="principal.authorities"></span>
```

### Restricting Access Based on Roles
/ -> HOME can be accessed by everyone
/leaders -> only by MANAGER
/systems -> Only accessed by ADMIN

##### STEPS 
1. Create supporting controller code and view pages for all the endpoints
2. Restricting Access to Roles
`requestMatchers(<<add path to match on>>).hasRole(<< authorized role >>)`
`requestMatchers(<<add path to match on>>).hasAnyRole(<< multiple roles >>)`

Ex : requestMatchers("/").hasRole("EMPLOYEE")
requestMatchers("/leaders/**").hasRole("MANAGER")
requestMatchers("/systems/**").hasRole("ADMIN")

** -> Match on path and all sub directories

**PUTTING IT ALL TOGETHER** 
`DemoSecurityConfig.java`
```java
...
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
	http.authorizeHttpRequests(configurer ->
			configurer
				.requestMatchers("/").hasRole("EMPLOYEE")
				.requestMatchers("/leaders/**").hasRole("MANAGER")
				.requestMatchers("/systems/**").hasRole("ADMIN")
				.anyRequest().authenticated()
		)
}
```

Note :
- ** -> all paths/ sub directories
- .hasRole(...) -> for single role
- hasAnyRole(...) -> for multiple roles
### Custom Access Denied Page : 
`DemoSecurityConfig.java`
```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    http.authorizeHttpRequests(configurer ->
            configurer
                .requestMatchers(“/").hasRole("EMPLOYEE")
                …
            )
            .exceptionHandling(configurer ->
                configurer
                    .accessDeniedPage("/access-denied")
   );
  …
}
```
- Then add controller code and view page : "/access-denied"


### Display Content based on Roles :
```html
	<div sec:authorize="hasRole('MANAGER')">
	data
	</div>
```

> [!info]
> Login using data stored in database in Unit 5

# NOTES FROM THE pdfs

### No login Public Landing Page

##### STEPS :
1. Update security Configs to allow unrestricted access to the landing page
`DemoSecurityConfig.java` -> `filterChain()`
```java
http.authorizeHttpRequests(configurer ->
			configurer
					.requestMatchers("/").permitAll()
					.requestMatchers("/employees/**").hasRole("EMPLOYEE")
					.requestMatchers("/leaders/**").hasRole("MANAGER")
					.requestMatchers("/systems/**").hasRole("ADMIN")
					.anyRequest().authenticated()
)
```
`.requestMatchers("/").permitAll()` -> is responsible for the unrestricted access

2. Update the logout success to URL to send user back to landing page : 
`DemoSecurityConfig.java` -> `filterChain()`
```java
.logout(logout ->
		logout
			.permitAll()
			.logoutSuccessUrl("/")
```

3. Update Controller to send requests to the landing page :
DemoController.java
```java
@GetMapping("/")
public String showLanding() {
	return "landing";
}
@GetMapping("/employees")
	public String showHome() {
return "home";
}
```

4. Create view for the new landing page "landing.html"

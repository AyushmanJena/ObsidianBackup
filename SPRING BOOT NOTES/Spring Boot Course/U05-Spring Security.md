# Spring Security
- Spring Security is implemented using servlet filters
- Servlet filters are components that intercept and modify HTTP requests before they reach the servlet or controllers.
- Servlet filters are used to pre-process/post-process web requests
![[Spring Security Filters.png]]
- Spring security filters perform 2 tasks : authentication and authorization
1. Authentication : Checks user id and password with credentials stored in app/database
2. Authorization : Checks to see if the user has an authorized role

- All java config : @Configuration  -> Declarative Security
- Spring security APIs for custom Application security -> Programmatic Security

##### Dependency for Spring Security : 
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-security</artifactId>  
</dependency>
```
- Default user : user
- default password : generated in console

Overriding default username and password :
`application.properties`
```properties
spring.security.user.name = scott
spring.security.user.password = test123
```
### Configuring Basic Security (without database)
##### Development Steps : 
1. Create Spring security configuration file
`DemoSecurityConfig.java`
```java
@Configuration
public class DemoSecurityConfig{
	@Bean
	public InMemoryUserDetailsManager userDetailsManager(){
		UserDetails john = User.builder()
						.username("john")
						.password("{noop}test123")
						.roles("EMPLOYEE")
						.build();
		UserDetails susan = User.builder()
						.username("susan")
						.password("{noop}test123")
						.roles("EMPLOYEE", "MANAGER", "ADMIN")
						.build();
		return new InMemoryUserDetailsManager(john, susan);
	}
}
```
- {noop} -> no encryption i.e. data stored in plain text
- Alternative : bcrypt
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

**PUTTING IT ALL TOGETHER** :  #revise
`DemoSecurityConfig.java`
```java
...
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
	http.authorizeHttpRequests(configurer ->
			configurer
				.requestMatchers(HttpMethod.GET, "/").hasRole("EMPLOYEE")
				.requestMatchers(HttpMethod.GET, "/leaders/**").hasRole("MANAGER")
				.requestMatchers(HttpMethod.POST, "/systems/**").hasRole("ADMIN")
		);
	
	http.httpBasic(Customizer.withDefaults());
	
	http.csrf(csrf -> csrf.disable());
	
	return http.build();
}
```

Note :
- ** -> all paths/ sub directories
- .hasRole(...) -> for single role
- hasAnyRole(...) -> for multiple roles

#### CSRF -> Cross Site Request Forgery
- Spring security can protect against CSRF attacks
- Primary use case is traditional web applications (HTML forms, etc.)
- In general not required for stateless REST API's, that use POST, PUT, DELETE, PATCH
- If you are building a REST API for non browser clients, you may disable CSRF protection
`http.csrf(csrf -> csrf.disable());`

Note : 
403 ERROR - PUT with spring data REST
- It needs to modify the security configuration

### Spring Security (Using User Accounts stored in Database)
##### Development Process :
1. Setup Database tables : 
- Default Spring Security database schemas :
- users and authorities are default schemas for spring security 
```sql
users : 
	username VARCHAR(50) PK
	password VARCHAR(50)
	enabled TINYINT(1)

insert values ('john', '{noop}test123', 1), etc.

authorities : 
	username VARCHAR(50) FK
	authority VARCHAR(50)

insert values ('susan', 'ROLE_EMPLOYEE'), etc.
```
 IMP : Internally Spring security uses "ROLE_" prefix

2. Add Database Support to Maven POM file : 
```xml
<dependency>  
    <groupId>com.mysql</groupId>  
    <artifactId>mysql-connector-j</artifactId>  
    <scope>runtime</scope>  
</dependency>
```

3. Add JDBC to properties file (same as jdbc)
```properties
spring.datasource.url = jdbc:mysql://localhost:3306/employee_directory  
spring.datasource.username = springstudent  
spring.datasource.password = springstudent
```

4. Update Spring Security config to use JDBC :
```java
@Bean
public UserDetailsManager userDetailsManager(DataSource dataSource){
	return new JdbcUserDetailsManager(dataSource);
}
```
- Inject data source auto-configured by spring boot

### Password Encryption 
bcrypt -> one way encryption hashing
Note : Needed to Modify DDL for password field 'password'  : char(68)
	- 8 chars for salt and 60 chars for the encrypted bcrypt text

#### Custom Tables (Alternate to default tables)
- Instead of **users** and **authorities** we want to use 
- **members** and **roles**

1. Create Custom sql tables :
```sql
members : 
	user_id VARCHAR(50) PK
	pw VARCHAR(50)
	active TINYINT(1)

insert values ('john', '{noop}test123', 1), etc.

roles : 
	user_id VARCHAR(50) FK
	role VARCHAR(50)

insert values ('susan', 'ROLE_EMPLOYEE'), etc.
```

2. Update Spring Security config file
```java
@Bean
public UserDetailsManager userDetailsManager(DataSource dataSource){
	JdbcUserDetailsManager theUserDetailsManager = new JdbcUserDetailsManager(dataSource);
	
	theUserDetailsManager.setUsersByUsernameQuery("select user_id, pw, active from members where user_id = ?");

	theUserDetailsManager.setAuthoritiesByUsernameQuery("select user_id, role from roles where user_id =  ?");

return theUserDetailsManager;
}
```
- '?' -> Parameter value will be the username from login

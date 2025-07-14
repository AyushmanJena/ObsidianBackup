JWT THEORY notes 

By Telusko : Spring Security 6 with Spring Boot and JWT Tutorial 
#### SOME SPRING SECURITY NOTES : 
###### CSRF Token :
when you do any update requests like PUT, POST, DELETE where you are changing some data, you need to send CSRF Token

In header :
key = X-CSRF-TOKEN
value = value generated from "/csrf-token"

```java
@GetMapping("/csrf-token")
public CsrfToken getCsrfToken(HttpServletRequest request){
	return (CsrfToken) request.getAttribute("_csrf");
}
```
or inspecting the page and finding the hidden csrf element in browser


### Configuring Security Filter Chain
With Spring Security dependency 
by default spring security provides a filter chain
we want to customize it and add our own filter chain
by creating a config class

 NOTE : for custom filter chain we still need the spring security dependency as well.

config package -> SecurityConfig.java
- add @Configuration annotation to notify spring about the config class
- @EnableWebSecurity -> tells spring to not follow the default spring security filter chain
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
	@Bean
	public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception{
		return http.build();
	}
}
```
- since we have not specified any filter, therefore by default no filter is applied
- it does not ask any security, no username and password anymore

##### How to provide a security of our own
- First we need to disable csrf to implement our own security
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
	@Bean
	public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception{

		http.csrf(customizer -> customizer.disable());

		http.authozrizeHttpRequests(request -> request.anyRequest().authenticated()); // any request needs to be authenticated however we cannot use the username and password passed through http request now 
		// to receive the data we need to enable form login

		http.formLogin(Customizer.withDefaults()); // it picks up the default property now for form login 
		// it seems similar to the default spring security form login in browser 
		// however in the postman or other REST client it behaves weirdly and returns the form http as response
		// to fix that we need to add httpBasic

		http.httpBasic(Customizer.withDefaults());
		// now postman works as default spring security as well

		...
		
		return http.build();
	}
}
```
- /logout will not work since we have not implemented it.

making the session stateless or stateful in the same class add : 
```java
http.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolity.STATELESS))
```
- we have different values : ALWAYS, NEVER, IF_REQUIRED

Due to Stateless session, Now you need to send the username and password for each request to the server
It works properly with postman and http, however asks for authentication for each page in the browser (can be solved by removing `formLogin()` from the config)


Instead of doing all the steps one by one, we can alternatively use a builder pattern : 
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
	@Bean
	public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception{

		return http
			.csrf(customizer -> customizer.disable())
			.authozrizeHttpRequests(request -> request.anyRequest().authenticated())
			.httpBasic(Customizer.withDefaults())
			.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolity.STATELESS)).build;
			
	}
}
```


### Implementing multiple Users 
UserDetailsService to verify user details 

Create another Bean in the SecurityConfig.java to specify the source of User Details storage
```java
@Bean
public UserDetailsService userDetailsService(){

	UserDetails user1 = new User
		.withDefaultPasswordEncoder()
		.username("kiran")
		.password("k@123")
		.roles("USER") 
		.build();
	...
	
	return new InMemoryUserDetailsManager(user1, ...);
}
```
- UserDetailsService is a inbuild interface
- For now we are using the inbuilt implementation i.e. InMemoryUserDetailsManager which implements UserDetailsService
- We can pass various UserDetails objects through the constructor 


### Implementing Database for Multiple Users

add 2 dependencies `spring boot starter jpa` & `postgres` (or the dbms specific)

Note: 
- When you pass the details (username and password in the login form) that detail is received by the server as an Authentication Object (Un-authenticated Object)
- It goes to the authentication provider, provides the authentication features and converts it into an Authenticated Object 

- Now we want to use our custom Authentication Provider, which we want to be connected to the database to check for authentication

Here we are creating a bean to change our Authentication Provider and adding it to the SecurityConfig.java

```java
@Autowired
private UserDetailsService userDetailsService;

@Bean
public AuthenticationProvider authenticationProvider(){
	DaoAuthenticationProvider provider = new DaoAuthenticationProvider();

	// specifying the password encoder : here no encoder
	provider.setPasswordEncoder(NoOpPasswordEncoder.getInstance());

	// specifying the UserDetailsService to verify the users
	provider.setUserDetailsService(userDetailsService)

	return provider;
}
```
- again AuthenticationProvider is a interface which is implemented by a class DaoAuthenticationProvider (DAO -> Data Access Object)
- To specify our own UserDetailsService create another class which implements the UserDetailsService (service/MyUserDetailsService.java)
```java
@Service
public class MyUserDetailsService implements UserDetailsService {

	@AutoWired
	private UserRepo repo;

	@Override
	public UserDetails loadUserByUserName(String username) throws UserNotFoundException {
		User user = repo.findByUsername(username);

		if(user == null) {
			System.out.println("User Not Found");
			throw new UserNotFoundException("User not found");
		}
		
		return new UserPrincipal(user);
	}
}
```

create a new database
```sql
create table users (id integer primary key, username text, password text);
insert into users values(1, 'navin', 'n@123');
...
```

also specify the dbms properties in application.properties
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/database
spring.datasource.username=postgres
spring.datasource.password=0000
```

Implementing JPA Repository 
```java
@Repository
public interface UserRepo extends JpaRepository<Users, Integer>{
	 Users findByUsername(String username);
}
```

create Users model class
```java
@Entity
public class Users { 
	@Id
	private int id;
	private String username;
	private String password;

	... getters and setters
}

```

create a class that implements UserDetails (to be used by UserDetailsService)
# Maybe get the whole code from a repo for this class
```java
public class UserPrincipal implements UserDetails{

	private Users user;

	public UserPrincipal(Users user){
		this.user = user;
	}

	// you need to implement all the methods to return the user details as per the pojo 
	
}
```



### Encrypting the Passwords 
Using Bcrypt  twice :
- when user registers
- when we are validating the user

A controller for creating a user 
```java
@RestController
public class UserController{

	@Autowired
	private UserService service;

	@PostMapping("/register")
	public Users register(@RequestBody Users user){
		return service.register(user);
	}
}
```

service for the same
```java
@Service
public class UserService{

	@Autowired
	private UserRepo repo;

	private BCryptPasswordEncoder encoder = new BCryptPasswordEncoder(12); // the number is the rounds for encryption (between 4 and 31)

	public Users register(Users user){
		user.setPassword(encoder.encode(user.getPassword()));
		return repo.save(user);
	}
}
```

Modify the SecurityConfig file to also verify a bcrypt password and not noop encoder
```java
@Autowired
private UserDetailsService userDetailsService;

@Bean
public AuthenticationProvider authenticationProvider(){
	DaoAuthenticationProvider provider = new DaoAuthenticationProvider();

	// specifying the password encoder : here no encoder
	provider.setPasswordEncoder(new BCryptPasswordEncoder(12));

	// specifying the UserDetailsService to verify the users
	provider.setUserDetailsService(userDetailsService)

	return provider;
}
```
 
and now encryption works

### JWT 
Json Web Token
Check JSON notes :)

# Implementing Authentication on the Backend

user logs in -> we give them a jwt token (after authentication)
for further requests he goes with the jwt token and not the username and password for every calls.

1. Implement Spring Security

using JJWT : Java JWT 
Dependency : 
```xml
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-api</artifactId>
	<version>0.12.5</version>
</dependency>
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-jackson</artifactId>
	<version>0.12.5</version>
</dependency>
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-impl</artifactId>
	<version>0.12.5</version>
	<scope>runtime</scope>
</dependency>
```

the three dependencies are : 
1. jjwt api
2. jjwt with jackson for json processing
3. jjwt implementation during runtime



We need to handle the AuthenticationManager, so we specify it in the SecurityConfig
SecurityConfig.java
```java
@Bean
public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception{
	return config.getAuthenticationManager();
}
```

add an endpoint /login to get login  
UserController.java
```java
@RestController
public class UserController{

	@Autowired
	private UserService service;

	@PostMapping("/register")
	public Users register(@RequestBody Users user){
		return service.register(user);
	}

	@PostMapping("/login")
	public String login(@RequestBody Users user){
		return service.verify(user);  
	}
}
```

add verify() to check if the user is valid in the service class
UsesrService.java
```java
@Autowired
AuthenticationManager authManager; // we will verify the user using the AuthenticationManager we created

public String verify(Users user){
	Authentication authentication = 
		authManager.authenticate(new UserPasswordAuthenticationToken(user.getUsername(), user.getPassword()));
	// above you are using the same class object in both the parameter and return, however in the parameter you are sending an unauthenticated object and in return you are getting an authenticated object

	if(authentication.isAuthenticated())
		return "Success";
		
	return "failure";
}
```

issue : as of now we need to be authenticated for every resource 
but we want to open the /login for everyone to be able to login 
so we make changes to the security filter chain in SecurityConfig

SecurityConfig.java
```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception{
	return http
			.csrf(customizer -> customizer.disable())
			.authozrizeHttpRequests(request -> request

			.requestMatchers("register", "login")
			.permitAll()
			
			.anyRequest().authenticated())
			.httpBasic(Customizer.withDefaults())
			.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolity.STATELESS)).build;

}
```
- This is how we differentiate between open links and closed resources

now we can pass id, username and password as json to the body for /login

If the data is correct we get the response "Success"
Now, instead of success we want to generate a token 

UsesrService.java
```java
@Autowired
AuthenticationManager authManager; // we will verify the user using the AuthenticationManager we created

@Autowired
private JWTService jwtService;

public String verify(Users user){
	Authentication authentication = 
		authManager.authenticate(new UserPasswordAuthenticationToken(user.getUsername(), user.getPassword()));
	// above you are using the same class object in both the parameter and return, however in the parameter you are sending an unauthenticated object and in return you are getting an authenticated object

	if(authentication.isAuthenticated())
		return jwtService.generateToken(user.getUsername());
		
	return "failure";
}
```

keeping jwt stuff in another service class
JWTService.java
```java
@Service
public class JWTService{
	public String generateToken(String username){
		return "xyz.abcd.pqrs"; // hard coded token 
	}
}

```

### Generating the JWT Token
- generating a token based on the user (subject)
- issue date, expiry date, etc.

```java
@Service
public class JWTService{

	private String secretKey = ""; // temporary key, will secure later

	public JWTService(){ // to generate a key which is secure enough
		try {
			KeyGenerator keyGen = KeyGenerator.getInstance("HmacSHA256"); // specify the algorithm we are working with
			SecretKey sk = keyGen.generateKey();
			secretKey = Base64.getEncoder().encodeToString(sk.getEncoded());
		} catch(NoSuchAlgorithmException e){
			throw new RuntimeException(e);
		}
		
	}

	public String generateToken(String userName){
		Map<String, Object> claims = new HashMap<>();
		// map to specify all the details we want to mention in the token

		return Jwts.builder()
			.claims()
			.add(claims)
			.subject(userName)
			.issuedAt(new Date(System.currentTimeMillis()))
			.expiration(new Date(System.currentTimeMillis() + 60 * 60 *30)) // for 30 minutes
			.and()
			.signWith(getKey()) // here we pass a key to generate
			.compact();
	}

	private Key getKey(){
		byte[] keyBytes = Decoders.BASE64.decode(secretKey);// convert string to byte array
		return Keys.hmacShaKeyFor(keyBytes); // convert into Key object for hmacsha algo
	}
}

```


### Using the Token for further requests
- Now instead of using the basic auth we want to use Bearer Token generated on login for each further request
- // We want to login using the token if it is valid

1. Validate the token
2. Giving access to the user

By default we are using username and password authentication filter
Now we need to change it to jwt based authentication filter
SecurityConfig.java
```java
@Autowired
private JwtFilter jwtFilter; // this

@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception{
	return http
			.csrf(customizer -> customizer.disable())
			.authozrizeHttpRequests(request -> request

			.requestMatchers("register", "login")
			.permitAll()
			
			.anyRequest().authenticated())
			.httpBasic(Customizer.withDefaults())
			.sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolity.STATELESS))
			.addFilterBefore(jwtFilter, UsernamePasswordAuthenticationFilter.class)            // this
			build;

}
```
- in the changes we are specifying before UsernamePasswordAuthenticationFilter use the our own jwtFilter

JwtFilter.java
```java

@Autowired
ApplicationContext context; // using this class to use the bean (doubt)

@Component
public class JwtFilter extends OncePerRequestFilter{
	@Override
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain){ // implementing the abstract method
		 // we get : "Bearer exhsdfkkjasdf..." from client side in Authorization
		 // we have to trim the Bearer part and validate the token
		 String authHeader = request.getHeader("Authorization");
		 String token = null;
		 String username = null;

		if(authHeader != null && authHeader.startsWith("Bearer ")){
			token = authHeader.substring(7);
			username = jwtService.extractUserName(token);
		}

		if(username != null && SecurityContextHolder.getContext().getAuthentication() == null) { // checking if username is not null and if the user is already authenticated

			//UserDetails userDetails = context.getBean(UserDetails.class);
			// doing the above statement would give us an empty object, but we want to store user details data in the object using a method loadUserByUsername from MyUserDetailsService class

			UserDetails userDetails = context.getBean(MyUserDetailsService.class).loadUserByUserName(user);
			
			if(jwtService.validateToken(token, userDetails)){
				// passing userDetails to make sure the user is part of our database
				  UsernamePasswordAuthenticationToken authToken = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
				  authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
				  SecurityContextHolder.getContext().setAuthentication(authToken);
			}
		}
		filterChain.doFilter(request, response);
	}
}
```
// Plenty of doubts in the above codes to be understood 


add extractUserName() in the JwtService class
```java
public String extractUserName(String token){
	return "";
}

public boolean validateToken(String token, UserDetails userDetails){
	return true;
}
```
# copy the code for JwtService.java from repo 


# Adding Google or Github based login in our website
using Oauth 2
 
We need a new dependency : 
OAuth2 Resource Server -> for creating our own server
or
OAuth2 Client -> for using existing server (here google or github)
- OAuth2 itself gives us Spring Security dependencies

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

HelloController
```java
@RestController
public class HelloController{
	@getMapping("/")
	public String greet(){
		return "Halo noobs";
	 }
}
```

SecurityConfig.java
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig{
	@Bean
	public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception{
		
		http
			.authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
			.oauth2Login(Customizer.withDefaults());
		
		return http.build();
	}
}
```


configuring the server details properties
application.properties
```java
spring.security.oauth2.client.registration.google.client-id=xyz
spring.security.oauth2.client.registration.google.client-secret=abc
```
users can see client-id, but not secret

google cloud console -> apis and services -> credentials -> create credentials -> oAuth 2.0 Client IDs -> Web Application and name -> specify Authorised redirect URI (http://localhost:8080/login/oauth2/code/google)

there you can get the Client Id and Client Secret
 
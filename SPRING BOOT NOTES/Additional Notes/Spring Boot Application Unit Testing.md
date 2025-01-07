
Note : The general naming scheme for test classes is same as the application class + Tests
Ex : UserService -> UserServiceTest

Unit Testing : [Unit Testing notes](obsidian://open?vault=OBS%20notes&file=SPRING%20BOOT%20NOTES%2FAdditional%20Java%2FUnit%20Testing)

@SpringBootTest annotation for the class
Ex : 
```java
@SpringBootTest
public class UserServiceTests {

	@Autowired
	private UserRepository userRepository;

	@Test
	public void testFindUserName(){
		assertNotNull(userRepository.findUserName("ayush"));
	}
}
```

@Disabled  -> If we have multiple tests in the class and one test (method) marked with @Disabled annotation. If we run the whole class the marked test won't execute

#### To find out which test failed : 
- All assert methods can pass an argument "message" to display if it fails
Ex : `assertNotNull(userRepository.findByUserName(name), "failed for "+name);`

## ParameterizedTest
Test multiple values with parameters with the same test
Taking input from csv
This is represented as multiple tests in the output
- CSV : Comma Separated Values
- 

```java
@ParameterizedTest
@CsvSource({
	"1,1,2",
	"2,10, 13",
	"0,0,0"
})
public void test(int a, int b, int expected){
	assertEquals(expected, a + b);
}
```


### Using ArgumentsSource for tests
Use another class to get test inputs 
Test : 
```java
@ParameterizedTest
@ArgumentsSource(UserArgumentsProvider.class)
public void testSaveNewUser(User user){
	assertTrue(userService.saveNewUser(user));
}
```

class which gives arguments for the tests : 
`UserArgumentsProvider.java`
```java
public class UserArgumentsProvider implements ArgumentsProvider{
	@Override
	public Stream<? extends Arguments> provideArguments(ExtensionContent extensionContent) throws Exception {
		return Stream.of(
			Arguments.of(new User("Shyam", "shyam")), // username and password
			Arguments.of(new User("Suraj", "suraj"))
		);
	}
}
```

Method to test : 
`UserService.java`
```java
public boolean savNewUser(User user){
	try{
		user.setPassowrd(passwordEncoder.encode(user.getPassword()));
		user.setRoles(Arrays.asList("USER"));
		userRepository.save(user);
		return true;
	}catch(Exception e){
		return false;
	}
}
```


## Mocking using Mockito
 To replicate real life scenarios for testing you might need database integration, etc.
 i.e. not using userRepository for the test for large databases 

`import static org. mockito.Mockito.*;`

@MockBean -> replaces the actual repository  
@Mock -> Has no importance in Spring application context i.e. using spring @Autowired, etc.
@InjectMocks -> replaces @Autowired if you use @Mock instead of @MockBean, but then you need to initialize the mocks 

UserDetailsServiceImplTests
```java

@Autowired
private UserDetailsServiceImpl userDetailsService;

@MockBean
private UserRepository userRepository;

@Test
void loadUserByUserNameTest(){
	when(userRepository.findByUserName("username")).thenReturn(new User("username", "password"));
	UserDetails user = userDetailsService.loadUserByUserName("username");
	assertNotNull(user);
}
```
Basic Angular Project Setup
To request an endpoint we need Axios

Create a service to handle requests to the backend
Configuring the axios to make requests to backend
`axios.service.ts`
```ts
@Injectable({  
  providedIn: 'root'  
})  
export class AxiosService {  
  
  constructor() {  
    axios.defaults.baseURL = 'http://localhost:8080';  
    axios.defaults.headers.post["Content-Type"] = "application/json";  
  }  
  
  request(method: string, url: string, data: any): Promise<any> {  
    return axios({  
      method: method,  
      url: url,  
      data: data,  
    });  
  }  
}
```

Create another component to display the content returned from frontend.
auth-content
`auth-content.component.ts`
```ts
export class AuthContentComponent {  
  data: string[] = [];  
  
  constructor(private axiosService: AxiosService) { }  
  
  ngOnInit() {  
    this.axiosService.request(  
      "GET","/messages", ()     // replace () with null
    ).then(  
      (response)=> this.data   = response.data  
    );  
  }  
}
```
`auth-content.component.html`
```html
<div class="row justify-content-md-center">  
  <div class="col-4">  
    <div class="card" style="width: '18rem'">  
      <div class="card-body">  
        <h5 class="card-title">Backend response</h5>  
        <p class="card-text">Content:</p>  
        <ul>          <li *ngFor="let line of data">{{line}}</li>  
        </ul>      </div>    </div>  </div></div>
```

while connecting backend and frontend we need to enable CORS on backend 
it allows an external frontend to access the backend
config -> WebConfig.java
```java
@Configuration  
@EnableWebMvc  
public class WebConfig {  
    @Bean  
    public FilterRegistrationBean corsFilter(){  
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();  
        CorsConfiguration config = new CorsConfiguration();  
        config.setAllowCredentials(true);  
        config.addAllowedOrigin("http://localhost:4200");  
        config.setAllowedHeaders(Arrays.asList(  
                HttpHeaders.AUTHORIZATION,  
                HttpHeaders.CONTENT_TYPE,  
                HttpHeaders.ACCEPT  
        ));  
  
        config.setAllowedMethods(Arrays.asList(  
                HttpMethod.GET.name(),  
                HttpMethod.POST.name(),  
                HttpMethod.PUT.name(),  
                HttpMethod.DELETE.name()  
        ));  
        config.setMaxAge(3600L); // time for which the config is accepted  
        source.registerCorsConfiguration("/**", config); // config for all roots  
        FilterRegistrationBean bean = new FilterRegistrationBean(new CorsFilter(source));  
        bean.setOrder(-102); // placing it in lowest position, to be executed before any Spring bean  
        return bean;  
    }  
}
```

Now we have successfully connected the backend with the frontend

### Create a login form
We are creating an event, this way, the submit method will be in the content(parent) component, 
Having the login request in the parent component allows us to handle the response and switch from the login form to the auth-content component once authenticated.

`login-form.component.ts`
```ts
import {Component, EventEmitter, Output} from '@angular/core';  
  
@Component({  
  selector: 'app-login-form',  
  standalone: false,  
  templateUrl: './login-form.component.html',  
  styleUrl: './login-form.component.css'  
})  
export class LoginFormComponent {  
  @Output() onSubmitLoginEvent = new EventEmitter();  
  
  login : string = "";  
  password:  string = "";  
  
  onSubmitLogin() {  
    this.onSubmitLoginEvent.emit({"login": this.login, "password": this.password}) ;  
  }  
}

```

`login-form.component.html`
```html
<div class="row justify-content-center">  
  <div class="col-4">  
    <form (ngSubmit)="onSubmitLogin()" #loginForm="ngForm">  
  
      <div class="form-outline mb-4">  
        <input type="login" id="loginName" name= "login" class="form-control" [(ngModel)]="login" />  
        <label class="form-label" for="loginName">Username</label>  
      </div>  
      <div class="form-outline mb-4">  
        <input type="password" id="loginPassword" name="password" class="form-control" [(ngModel)]="password" />  
        <label class="form-label" for="loginPassword">Password</label>  
      </div>  
      <button type="submit" class="btn btn-primary btn-block mb-4">Sign in</button>  
  
    </form>  
  </div>
</div>
```

Now in the parent (content) component we will create the submit component.

content.component.html
```html
<div>  
<!-- routing will be added later -->  
  <app-login-form (onSubmitLoginEvent)="onLogin($event)"/>  
</div>
```
content.component.ts
```ts
@Component({  
  selector: 'app-content',  
  standalone: false,  
  templateUrl: './content.component.html',  
  styleUrl: './content.component.css'  
})  
export class ContentComponent {  
  
  constructor(private axiosService: AxiosService) {  
  }  
  
  onLogin(input: any){  
    this.axiosService.request(  
      "POST",  
      "/login",  
      {  
        login: input.login,  
        password: input.password  
      }  
    )  
  }  
}
```

### Spring Security Configuration
Now in the backend we will create the login endpoint that accepts the user name and password.
For that add the Spring Security Dependency and protect the messages endpoint

### Preparing the Backend for accepting the login request

```java
@RestController  
public class AuthController {  
    @PostMapping("/login")  
    public ResponseEntity<UserDto> login(@RequestBody CredentialsDto credentialsDto){  
        UserDto user = userService.login(credentialsDto);  
        return ResponseEntity.ok(user);  
    }  
}
```

#### Preparing and Connecting the Database
`docker run -d -e POSTGRES_HOST_AUTH_METHOD=trust -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=Vulcan@24 -e POSTGRES_DB=backenddb -p 5432:5432 postgres:13`

backend properties file
application.yml (replaces application.properties)
```yml
spring:  
  datasource:  
    driver-class-name: org.postgresql.Driver  
    url: jdbc:postgresql://localhost:5432/backenddb  
    username: backend  
    password: backend  
  jpa:  
    database-platform: org.hibernate.dialect.PostgreSQLDialect  
    hibernate:  
      ddl-auto: 'create-drop'
```

create entities : 
`User.java`
```java
@AllArgsConstructor  
@NoArgsConstructor  
@Builder  
@Data  
@Entity  
@Table(name ="app_user")  
public class User {  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @Column(name="first_name", nullable = false)  
    private String firstName;  
  
    @Column(name="last_name", nullable = false)  
    private String lastName;  
  
    @Column(nullable = false)  
    private String login;  
      
    @Column(nullable = false)  
    private String password;  
}
```

Create repositories
UserRepository
```java
public interface UserRepository extends JpaRepository<User, Long> {  
    Optional<User> findByLogin(String Login);  
      
}
```

We need mappers with MapStruct to map User Entity to User Details
folder -> mappers

```java
@Mapper(componentModel = "spring")  
public interface UserMapper {  
    UserDto toUserDto(User user);  
}
```

We need a password Encoder (here using )
PasswordConfig.java
```java

```
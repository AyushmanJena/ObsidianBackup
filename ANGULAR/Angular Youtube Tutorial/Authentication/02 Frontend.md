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
    <ul class="nav nav-pills nav-justified mb-3" id="ex1" role="tablist">  
      <li class="nav-item" role="presentation">  
        <button [ngClass]="active == 'login' ? 'nav-link active' : 'nav-link'" id="tab-login"  
                (click)="onLoginTab()" >Login</button>  
      </li>      <li class="nav-item" role="presentation">  
        <button [ngClass]="active == 'register' ? 'nav-link active' : 'nav-link'" id="tab-register"  
                (click)="onRegisterTab()" >Register</button>  
      </li>    </ul>  
    <div class="tab-content">  
      <div [ngClass]="active == 'login' ? 'tab-pane fade show active' : 'tab-pane fade'">  
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
  
        </form>      </div>      <div [ngClass]="active == 'register' ? 'tab-pane fade show active' : 'tab-pane fade'">  
        <form (ngSubmit)="onSubmitRegister()" #registerForm="ngForm">  
  
          <div class="form-outline mb-4">  
            <input type="text" id="firstName" name="firstName" class="form-control" [(ngModel)]="firstName" />  
            <label class="form-label" for="firstName">First name</label>  
          </div>  
          <div class="form-outline mb-4">  
            <input type="text" id="lastName" name="lastName" class="form-control" [(ngModel)]="lastName" />  
            <label class="form-label" for="lastName">Last name</label>  
          </div>  
          <div class="form-outline mb-4">  
            <input type="text" id="login" name="login" class="form-control" [(ngModel)]="login" />  
            <label class="form-label" for="login">Username</label>  
          </div>  
          <div class="form-outline mb-4">  
            <input type="password" id="registerPassword" name="password" class="form-control" [(ngModel)]="password" />  
            <label class="form-label" for="registerPassword">Password</label>  
          </div>  
          <button type="submit" class="btn btn-primary btn-block mb-3">Sign in</button>  
        </form>      </div>    </div>  </div></div>
```


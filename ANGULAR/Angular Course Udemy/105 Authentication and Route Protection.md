notes for Angular 16 and less probably

### Basic Authentication overview
Client sends Auth data to server
Server sends token to Client (JWT)
The client stores the token in the local browser storage 
For each next requests, client sends the token along with the request to the server


### Adding the Authentication Page
- Create an authentication component AuthComponent
```ts
@Component({
	...
})
export class AuthComponent {
	isLoginMode = true;

	onSwitchMode(){
		this.isLoginMode = !this.isLoginMode;
	}

	onSubmit(form: NgForm){
		console.log(form.value);
		form.reset();
	}
}
```
- isLoginMode -> check if the user is in log in or signup mode


```html
<div class="row">  
  <div class="col-xs-12 col-md-6 col-md-offset-3">  
    
    <form #authForm="ngForm" (ngSubmit)="onSubmit(authForm)">      
	 <div class="form-group">  
        <label for="email">E-Mail</label>  
        <input type="email" id="email" class="form-control" ngModel name="email" required email/>  
     </div>      
     <div class="form-group">  
        <label for="password">Password</label>  
        <input type="password" id="password" class="form-control" ngModel name="password" required minlength="6"/>  
      </div>      
      <div>        
	    <button class="btn btn-primary" type="submit" [disabled]="!authForm.valid">{{isLoginMode ? 'Login' : 'Sign Up'}}</button> |  
        <button class="btn btn-primary" (click)="onSwitchMode()" type="button">Switch {{isLoginMode ? 'Sign Up' : 'Login'}}</button>  
      </div>
    </form>
    
  </div>
</div>
```

add a new path to the routing module: 
`{path: 'auth', component: AuthComponent}`

### Preparing the Backend using Firebase


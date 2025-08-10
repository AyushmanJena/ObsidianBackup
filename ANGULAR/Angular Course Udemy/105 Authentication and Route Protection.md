notes for Angular 16 and less probably

[[#FULL CODE for the Auth Component]]

### Basic Authentication overview
Client sends Auth data to server
Server sends token to Client (JWT)
The client stores the token in the local browser storage 
For each next requests, client sends the token along with the request to the server


### Adding the Authentication Page
- Create an authentication component AuthComponent
auth.component.ts
```ts
@Component({
	...
})
export class AuthComponent {
	isLoginMode = true;

	// injecting AuthService into the component (code below)
	constructor(private authService: AuthService){} 

	onSwitchMode(){
		this.isLoginMode = !this.isLoginMode;
	}

	onSubmit(form: NgForm){
		if(!form.valid){
			return;
		}
		const email = form.value.email;
		const password = form.value.password;

		if(this.isLoginMode){ // login logic
			// ...
		}else{            // signup logic
			this.authService.signup(email, password).subscribe(resData => {
				console.log(resData);
			}, error => {
			console.log(eroror)
			}); // main signup code
			// we have to subscribe since it is a observable 
			// in the response we get resData if it suceeds
		}

		
		form.reset();
	}
}
```
- isLoginMode -> check if the user is in log in or signup mode

auth.component.html
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

Enable email and password method for authentication in Firebase
- Check Firebase Auth REST API for all the api endpoints available for creating and logging users using firebase backend service
- Specifically Sign In with email / password and Sign Up with email / password

`http://www.googleapis.com/identitytoolkit/v3/relyingparty/signupNewUser?key=[API_KEY]`


in the response payload firebase automatically gives us an idToken, for authentication for newly created users and expiresin field as well.
### Handling Authentication in service layer
Create a new service auth.service.ts
```ts
import { Injectable } from '@angular/core';  
import { HttpClient, HttpErrorResponse } from '@angular/common/http';  
import { Router } from '@angular/router';  
import { catchError, tap } from 'rxjs/operators';  
import { throwError, BehaviorSubject } from 'rxjs';  
  
import { User } from './user.model';  
  
export interface AuthResponseData {     // response structure
  kind: string;  
  idToken: string;  
  email: string;  
  refreshToken: string;  
  expiresIn: string;  
  localId: string;  
  registered?: boolean;  
}  
  
@Injectable({ providedIn: 'root' })  
export class AuthService {  
  
  constructor(private http: HttpClient, private router: Router) {}  
  
  signup(email: string, password: string) {  
    return this.http  
      .post<AuthResponseData>(  // response type specified
        'https://www.googleapis.com/identitytoolkit/v3/relyingparty/signupNewUser?key=AIzaSyDb0xTaRAoxyCgvaDF3kk5VYOsTwB_3o7Y',   // api
        {  
          email: email,  
          password: password,  
          returnSecureToken: true  // this is request body
        }  
      )  
      .pipe(  
        catchError(this.handleError),  
        tap(resData => {  
          this.handleAuthentication(  
            resData.email,  
            resData.localId,  
            resData.idToken,  
            +resData.expiresIn      // this is response body
          );  
        })  
      );  
  }  
  
  login(email: string, password: string) {  
    return this.http  
      .post<AuthResponseData>(  
        'https://www.googleapis.com/identitytoolkit/v3/relyingparty/verifyPassword?key=AIzaSyDb0xTaRAoxyCgvaDF3kk5VYOsTwB_3o7Y',  
        {  
          email: email,  
          password: password,  
          returnSecureToken: true  
        }  
      )  
      .pipe(  
        catchError(this.handleError),  
        tap(resData => {  
          this.handleAuthentication(  
            resData.email,  
            resData.localId,  
            resData.idToken,  
            +resData.expiresIn  
          );  
        })  
      );  
  }  
  
  
}
```



#### Loading Spinner logic
Code can be found on various sites.
In the shared component add a folder "loading-spinner"
In the folder add a typescript file `loading-spinner.component.ts` and `loading-spinner.component.css` file
Add the component to declarations array in app.module.ts

`loading-spinner.component.ts`
```ts
@Component({  
  selector: 'app-loading-spinner',  
  template:  
    '<div class="lds-ring"><div></div><div></div><div></div><div></div></div>',  
  styleUrls: ['./loading-spinner.component.css']  
})  
export class LoadingSpinnerComponent {}
```

`loading-spinner.component.css`
```css
.lds-ring {  
  display: inline-block;  
  position: relative;  
  width: 64px;  
  height: 64px;  
}  
.lds-ring div {  
  box-sizing: border-box;  
  display: block;  
  position: absolute;  
  width: 51px;  
  height: 51px;  
  margin: 6px;  
  border: 6px solid #2102cf;  
  border-radius: 50%;  
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;  
  border-color: #2102cf transparent transparent transparent;  
}  
.lds-ring div:nth-child(1) {  
  animation-delay: -0.45s;  
}  
.lds-ring div:nth-child(2) {  
  animation-delay: -0.3s;  
}  
.lds-ring div:nth-child(3) {  
  animation-delay: -0.15s;  
}  
@keyframes lds-ring {  
  0% {  
    transform: rotate(0deg);  
  }  
  100% {  
    transform: rotate(360deg);  
  }  
}
```

Now using another property in the auth.component.ts "isLoading" we can specify if we want the loading component to be visible or the login component to be visible. (Check full code)

#### Error Handling
Similar to isLoginMode 
`error: string = null;`
- to store the error message in the error itself.

then change its value in the error methods in the subscriptions 

and also add a div which checks if error is null or not and displays a error.


### Login 
**note : point 2** 
create method in `auth.service.ts` and send a post request to the url. 
It will have a similar subscribe block in the component. 
authObs of type `Observable<AuthResponseData> ` which is a model for the response. Since both Sign up and sign in both have a similar response. 
And use the same authObs value to store the data for both login or signup observable. 
then the actual subscribe code is called on the authObs. 
Hence we reduce the code by sharing the same code.      

### Creating and Storing User Data
Create a model class for storing user data format `user.model.ts`
in the service class create a new Subject specifically `BehavioursSubject` which is used for storing and managing states and sharing of data between components.

We add another operator along with catchError operator to the pipe i.e. `tap` operator, and pass the response data.it allows us to perform some actions without changing the response
-  in this we create the user model object. 
- create and pass an expiration date to the object model constructor, (we got the expiry limit from the backend along with response)
- set the user as the current user using next() by Subject or BehaviourSubject which emits its value to all the subscribers.

Now for logging users in we create another method handleAuthentication() in service class
similar to tap, we perform the top 3 functionalities in the class as well. 
and call the function in login method in the service class along with all data and the expiresIn time as well.


### Reflecting the Auth state in UI
- Redirect auth when logged in
- In the authentication component ts file in the success case, we add the routing to the view homepage(recipies) component.

With authentication status, we want to add logout option in the header 
also disable other features based on it.
1. import authservice in the headder component
`header.component.ts`
```ts
isAuthenticated = false;
private userSub: Subscription;
ngOnInit(){
	this.userSub = this.authService.user.subscribe(user => {
		// if we have a user and we are logged in.
		this.isAuthenticated = !user ? false : true;
	});
}
ngOnDestroy(){
	this.userSub.unsubscribe();
}
...
```
2. Now in the header component we can use the isAuthenticated property with conditional templates in the header view template to show or hide elements.

## Adding the Token to Outgoing Requests

in the auth.service.ts we create a token property that can be accessed by other services/components  for making http requests
`token: string = null;`
However in our code for storing the current user we can use a different subject called BehaviourSubject by Rxjs which itself provides the subscribers the access to the previously emitted value. 
`user = new BehaviourSubject<User>(null);`

In the service where we are making http requests 
inject AuthService 

```ts
private authService = inject(AuthService);

fetchRecipes(){
	this.authService.user.pipe(take(1), exhaustMa(user => {
		return this.http 
		.get<Recipe[]>(  
		  'https://ng-course-recipe-book-65f10.firebaseio.com/recipes.json?auth='+user.token 
		)  
	}), 
	map(recipes => {  
		return recipes.map(recipe => {  
		    return {  
		    ...recipe,  
			    ingredients: recipe.ingredients ? recipe.ingredients : []  
		    };  
			});  
		}),  
		tap(recipes => {  
		this.recipeService.setRecipes(recipes);  
	});
}
```
take() operator tells the rxjs that we need to take 1 value from the observable and then unsubscribe. And we are not getting the future users.
exhaustMap waits for the first ohbservable(user) to complete then it gives us another observable after performing some operations on it.

alternative to `'https://ng-course-recipe-book-65f10.firebaseio.com/recipes.json?auth='+user.token` 
you can use separate token with http params
```ts
https://ng-course-recipe-book-65f10.firebaseio.com/recipes.json, 
{
	params: new HttpParams().set('auth', user.token)
}
```


### Adding the Token with an Interceptor
Interceptors are supposed to manipulate our requests, instead of manually doing it for each request we make.
create file auth-interceptor.service.ts
- don't inject the service as provided:root
- we will inject it differently
- instead of using the operators like take and exhaustMap in each http request service method, we will do that here and return an observable which can be subscribed in each of the methods.
- We are also adding the params with token within it by modifying the original request 
- check if we have a user, if we dont handle, it. 
- If we have a user then, add the token to the parameter 
provide the token in the app module under the providers 
```ts
...
providers: [  
  ShoppingListService,  
  RecipeService,  
  {  
    provide: HTTP_INTERCEPTORS,  
    useClass: AuthInterceptorService,  
    multi: true  
  }  
],
...
```
now we dont need to add the params individually for calls in the interceptor

### Adding Logout
In authservice add a logout()
- use the user subject and set the user to null
- then when the null value is emitted, the user is logged out
- redirect the user to login or homepage 
call it in the header component method
then add it with a click listener to the logout button

### Adding Auto Login
Untill now, when you reload the page, the user application restarts. 
Since the user detail is stored in a javascript variable.
So the user is automatically logged out.
To solve this, we have to store the user details in a persistence storage.
We can use **Cookies / local storage**
- while handling authentication in the auth.service.ts we can save the token in the localStorage using `setItem('userData', JSON.stringify(user))`
we will also retrieve it when app restarts
create a method `autoLogin()` in auth.service.ts to check if the userData is there in the local storage

### Adding Auto Logout
- Add a autoLogout(expirationDuration: number) in auth service taking time in miliseconds
- set timeout and set it to that duration
- after duration call the other logout() 
- then clear the timeout
// doubt here
Using a new variable in the auth service class, that can store the timeout duration. Note : If user manually logs out then clear the timer to make sure it cannot be logged in again. and if the timeout limit is reached, then it is logged out automatically. 


### Adding an Auth Guard 
- An auth guard can perform some logic before a route is loaded.
- auth.guard.ts


# FULL CODE for the Auth Component
auth.component.html
```ts
<div class="row">  
  <div class="col-xs-12 col-md-6 col-md-offset-3">  
    <div class="alert alert-danger" *ngIf="error">  
      <p>{{ error }}</p>  
    </div>    
    
    <div *ngIf="isLoading" style="text-align: center;">  
      <app-loading-spinner></app-loading-spinner>    
    </div>    
      
      <form #authForm="ngForm" (ngSubmit)="onSubmit(authForm)" *ngIf="!isLoading"> 
      <div class="form-group">  
        <label for="email">E-Mail</label>  
        <input type="email"  
          id="email"  
          class="form-control"  
          ngModel  
          name="email"  
          required  
          email />  
      </div>      
      <div class="form-group">  
        <label for="password">Password</label>  
        <input type="password"  
          id="password"  
          class="form-control"  
          ngModel  
          name="password"  
          required  
          minlength="6"  />  
      </div>      
      <div>
	      <button class="btn btn-primary"  
          type="submit"  
          [disabled]="!authForm.valid"  >  
          {{ isLoginMode ? 'Login' : 'Sign Up' }}  
        </button> |  
        <button class="btn btn-primary" (click)="onSwitchMode()" type="button">  
          Switch to {{ isLoginMode ? 'Sign Up' : 'Login' }}  
        </button>  
      </div>   
    </form>  
  </div>
</div>
```

auth.component.ts
```ts
  
@Component({  
  selector: 'app-auth',  
  templateUrl: './auth.component.html'  
})  
export class AuthComponent {  
  isLoginMode = true;  
  isLoading = false;            // for loading spinner visual component
  error: string = null;  
  
  constructor(private authService: AuthService, private router: Router) {}  
  
  onSwitchMode() {  
    this.isLoginMode = !this.isLoginMode;  
  }  
  
  onSubmit(form: NgForm) {  
    if (!form.valid) {  
      return;  
    }  
    const email = form.value.email;  
    const password = form.value.password;  
  
    let authObs: Observable<AuthResponseData>;  
  
    this.isLoading = true;              // make the loading spinner visible
  
    if (this.isLoginMode) {                             // POINT 2
      authObs = this.authService.login(email, password);  
    } else {  
      authObs = this.authService.signup(email, password);  
    }  
  
    authObs.subscribe(                                   // POINT 2 
      resData => {                     // this whole logic works for both login
        console.log(resData);                         // and signup 
        this.isLoading = false;             // hide the spinner
        this.router.navigate(['/recipes']);  // navigate if login success
      },  
      errorMessage => {                    // error response 
        console.log(errorMessage);  
        this.error = errorMessage;  
        this.isLoading = false;  
      }  
    );  
  
    form.reset();  
  }  
}
```

auth.guard.ts
```ts
import {ActivatedRouteSnapshot, CanActivateFn, Router, RouterStateSnapshot, UrlTree} from "@angular/router";
import {inject, Injectable} from "@angular/core";
import {Observable} from "rxjs";
import {AuthService} from "./auth.service";
import {map} from "rxjs/operators";
 
@Injectable({providedIn: 'root'})
export class AuthGuard {
 
  static authGuardFn: CanActivateFn = (route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean | UrlTree> => {
    const router = inject(Router);
    const authService = inject(AuthService);
 
    return authService.user.pipe(map(user => {
      const isAuth = !!user;
      if(isAuth) {
        return true;
      }
      return router.createUrlTree(['/auth']);
    }));
  }
 
}
```

auth.service.ts
```ts
  
export interface AuthResponseData {  
  kind: string;  
  idToken: string;  
  email: string;  
  refreshToken: string;  
  expiresIn: string;  
  localId: string;  
  registered?: boolean;  
}  
  
@Injectable({ providedIn: 'root' })  
export class AuthService {  
  user = new BehaviorSubject<User>(null);   // class is used for state management
										// and data sharing between components.
  private tokenExpirationTimer: any;  
  
  constructor(private http: HttpClient, private router: Router) {}  
  
  signup(email: string, password: string) {  
    return this.http  
      .post<AuthResponseData>(  
        'https://www.googleapis.com/identitytoolkit/v3/relyingparty/signupNewUser?key=AIzaSyDb0xTaRAoxyCgvaDF3kk5VYOsTwB_3o7Y',  
        {  
          email: email,  
          password: password,  
          returnSecureToken: true  
        }  
      )  
      .pipe(  
        catchError(this.handleError),       // transform the error message into 
        tap(resData => {                    // our suitable format
          this.handleAuthentication(  
            resData.email,  
            resData.localId,  
            resData.idToken,  
            +resData.expiresIn  
          );  
        })  
      );  
  }  
  
  login(email: string, password: string) {  
    return this.http  
      .post<AuthResponseData>(  
        'https://www.googleapis.com/identitytoolkit/v3/relyingparty/verifyPassword?key=AIzaSyDb0xTaRAoxyCgvaDF3kk5VYOsTwB_3o7Y',  
        {  
          email: email,  
          password: password,  
          returnSecureToken: true  
        }  
      )  
      .pipe(  
        catchError(this.handleError),  
        tap(resData => {  
          this.handleAuthentication(  
            resData.email,  
            resData.localId,  
            resData.idToken,  
            +resData.expiresIn  
          );  
        })  
      );  
  }  
  
  autoLogin() {  
    const userData: {  
      email: string;  
      id: string;  
      _token: string;  
      _tokenExpirationDate: string;  
    } = JSON.parse(localStorage.getItem('userData'));  
    if (!userData) {  
      return;  
    }  
  
    const loadedUser = new User(  
      userData.email,  
      userData.id,  
      userData._token,  
      new Date(userData._tokenExpirationDate)  
    );  
  
    if (loadedUser.token) {  
      this.user.next(loadedUser);  
      const expirationDuration =  
        new Date(userData._tokenExpirationDate).getTime() -  
        new Date().getTime();  
      this.autoLogout(expirationDuration);  
    }  
  }  
  
  logout() {  
    this.user.next(null);  
    this.router.navigate(['/auth']);  
    localStorage.removeItem('userData');  
    if (this.tokenExpirationTimer) {  
      clearTimeout(this.tokenExpirationTimer);  
    }  
    this.tokenExpirationTimer = null;  
  }  
  
  autoLogout(expirationDuration: number) {  
    this.tokenExpirationTimer = setTimeout(() => {  
      this.logout();  
    }, expirationDuration);  
  }  
  
  private handleAuthentication(  
    email: string,  
    userId: string,  
    token: string,  
    expiresIn: number  
  ) {  
    const expirationDate = new Date(new Date().getTime() + expiresIn * 1000);  
    const user = new User(email, userId, token, expirationDate);  
    this.user.next(user);  
    this.autoLogout(expiresIn * 1000);  
    localStorage.setItem('userData', JSON.stringify(user));  // save to local
  }  
  
  private handleError(errorRes: HttpErrorResponse) {  
    let errorMessage = 'An unknown error occurred!';  
    if (!errorRes.error || !errorRes.error.error) {  
      return throwError(errorMessage);  
    }  
    switch (errorRes.error.error.message) {  
      case 'EMAIL_EXISTS':  
        errorMessage = 'This email exists already';  
        break;  
      case 'EMAIL_NOT_FOUND':  
        errorMessage = 'This email does not exist.';  
        break;  
      case 'INVALID_PASSWORD':  
        errorMessage = 'This password is not correct.';  
        break;  
    }  
    return throwError(errorMessage);  
  }  
}
```


auth-interceptor.service.ts
```ts
@Injectable()  
export class AuthInterceptorService implements HttpInterceptor {  
  constructor(private authService: AuthService) {}  
  
  intercept(req: HttpRequest<any>, next: HttpHandler) {  
    return this.authService.user.pipe(  
      take(1),  
      exhaustMap(user => {  
        if (!user) {  
          return next.handle(req);  
        }  
        const modifiedReq = req.clone({  
          params: new HttpParams().set('auth', user.token)  
        });  
        return next.handle(modifiedReq);  
      })  
    );  
  }  
}
```

user.model.ts
```ts
export class User {  
  constructor(  
    public email: string,  
    public id: string,  
    private _token: string,  
    private _tokenExpirationDate: Date  
  ) {}  
  
  get token() {  
    // if token expiry date does not exist or is expired, return null
    if (!this._tokenExpirationDate || new Date() > this._tokenExpirationDate) {  
      return null;  
    }  
    return this._token;  
  }  
}
```

app.modult.ts
```
...
providers: [  
  ShoppingListService,  
  RecipeService,  
  {  
    provide: HTTP_INTERCEPTORS,  
    useClass: AuthInterceptorService,  
    multi: true  
  }  
],
...
```

app-routing.module.ts
```ts
const appRoutes: Routes = [
  { path: '',  redirectTo: '/recipes', pathMatch: 'full'},
  { path: 'recipes',
    component: RecipesComponent,
    canActivate: [AuthGuard.authGuardFn],
    children:[
      { path: '', component: RecipeStartComponent},
      { path: 'new', component: RecipeEditComponent},
      { path: ':id',
        component: RecipeDetailComponent,
        resolve: [RecipeResolverService.fetchRecipes]},
      { path: ':id/edit',
        component: RecipeEditComponent,
        resolve: [RecipeResolverService.fetchRecipes]},
    ],
    resolve: [RecipeResolverService.fetchRecipes]},
  { path: 'shopping-list', component: ShoppingListComponent},
  { path: 'auth', component: AuthComponent}
];
 
@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule{
 
}
```
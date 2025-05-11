## Routing and Building Multi-page Single Page Applications


- Up untill now we were using the same page and making changes to it

Routing helps you to change what the user sees in a single page app. 
Showing and hiding portions of the display that correspond to particular components, rather than going out to the server to get a new page.

Angular ships with its own router which allows us to change the url in the url bar, and still use one page, but appears to the user it is a new rendered page.

### for our project : 
In our app, we got three sections:
- Home
- Servers
    - View and Edit Servers
    - A Service is used to load and update Servers
- Users
    - View Users
we want to display only one at a time based on user clicks 

## Setting Up and Loading Routes
we want 
/users -> load users components , etc.

we make modifications to the app.module.ts (where all components and modules of our app are managed)
Class `Routes` is used to give a structure to our routes
It holds a array of js objects
the structure of the objects is pre defined

path : what gets entered in the url after domain
- a string
- ex : if paths = 'users'
- url becomes "localhose:4200/users"

component : signifies the component
- when this path is reached, the certain component should be loaded

empty path generally signifies home component or the starting page

we register the routes in our app with a new import RouterModule in the NgModule
The RouterModule has a special method to call forRoot() with our Routes as a parameter passed to it.

```ts
...
  
const appRoutes: Routes = [  
  { path: '', component: HomeComponent },  
  { path: 'users', component: UsersComponent },  
  { path: 'servers', component: ServersComponent },  
];  
  
@NgModule({  
  declarations: [  
    AppComponent,  
    HomeComponent,  
    UsersComponent,  
    ServersComponent,  
    UserComponent,  
    EditServerComponent,  
    ServerComponent  
  ],  
  imports: [  
    BrowserModule,  
    FormsModule,  
    RouterModule.forRoot(appRoutes)              // THIS
  ],  
  providers: [ServersService],  
  bootstrap: [AppComponent]  
})  
export class AppModule { }
```

Q. How would angular know where to display the component ?
That's where we use `router-outlet` directive
`<router-outlet>` tag is placed in the position where we want to place our routed component

```html
<div class="container">  
  <div class="row">  
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">  
      <ul class="nav nav-tabs">  
        <li role="presentation" class="active"><a href="#">Home</a></li>  
        <li role="presentation"><a href="#">Servers</a></li>  
        <li role="presentation"><a href="#">Users</a></li>  
      </ul>  
    </div>  
  </div>  
  <div class="row">  
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">  
      <router-outlet></router-outlet>            //THIS
    </div>  
  </div>  
</div>
```

- Now we can route to the components through typing the paths in the address bar

### Navigating with Router Links
replacing href links with "/server", etc. works. (not recommended)
```
<li role="presentation"><a href="/servers">Servers</a></li>
```

- However it reloads/refreshes the app and the component is displayed based on it. 
- However it is not the best behavior
- We lose our application state with this method

To solve this issue we have a special directive, that will replace the href attribute
`routerLink`

```
<li role="presentation"><a routerLink="servers">Servers</a></li>
```

You can also use property binding with non string values.
It gives a more fine grained control over routerLinks

### Understanding Navigation Paths

Relative Path vs Absolute Path :
- In a relative path, angular appends the path to the end of the current path, and note that the current path depends on the component you are currently on.
- `<a routerLink="servers">Reload Page</a>`
- 
- In absolute path, it goes to the exact path you want it to.
- `<a routerLink="/servers">Reload Page</a>`

You can also use paths like in folder directory structure
`"../servers"` means you go down one level and then /servers


### Styling Active Router Links

- Changing visual tab if the right route is active
- To add this angular provides a directive 
`routerLinkActive="active"`
- here active is the css class

```html
<li role="presentation" routerLinkActive="active"><a routerLink="/">Home</a></li>
```

add it to all the links (in our case tabs) they will be visible if the link is active 

However we face some issues with routerLinkActive like the home route is always active so it is always visible as active, etc.
To solve this issue we have another directive : `routerLinkActiveOptions` 
We have to apply property binding on it.
`[routerLinkActiveOptions] = "{exact: true}"`
- it is a reserved property
- it tells if the path is exactly true then only make it active

```html
<li role="presentation" routerLinkActive="active" [routerLinkActiveOptions] = "{exact: true}"><a routerLink="/home">Home</a></li>
```

FULL CODE
```html
<div class="container">  
  <div class="row">  
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">  
      <ul class="nav nav-tabs">  
        <li role="presentation" routerLinkActive="active" [routerLinkActiveOptions] = "{exact: true}"><a routerLink="/">Home</a></li>  
        <li role="presentation" routerLinkActive="active"><a routerLink="servers">Servers</a></li>  
        <li role="presentation" routerLinkActive="active"><a [routerLink]="['users']">Users</a></li>  
      </ul>  
    </div>  
  </div>  
  <div class="row">  
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">  
      <router-outlet></router-outlet>  
    </div>  
  </div>  
</div>
```

### Navigating Programmatically
- Ex : We want to trigger the navigation after something heppened, like form submitted, a button is clicked, etc.
- i.e. from our ts class

Here we will have to inject the Router 
router method navigate() helps us to navigate to the new route from our ts class
the navigate() takes a parameter, an array of path
`this.router.navigate(['/users']);`

home.component.ts
```ts
export class HomeComponent implements OnInit {  
  constructor(private router: Router) { }   // dependency injection
	...
  onLoadServer(){  
    // complex calculation  
    this.router.navigate(['/servers']);  
  }
}
```

home.component.html
```html
<button class="btn btn-primary" (click)="onLoadServer()">servers</button>
```


### Using Relative Paths in Programmatic Navigation

`this.router.navigate(['servers']);`
- this works just like the '/servers'
- because unlike the router link the navigate() does not know which route we are currently on.
To tell the method where we currently are we will have to pass a second argument (NavigationExtras), which takes an object, to which we can pass other parameters

```
this.router.navigate(['servers'], {relativeTo : });
```
- relativeTo property tells angular relative to which route this property should be loaded. By default it is always the root domain
- We dont need to manually specify the current route
- We can get the active route by injecting another dependency of type ActivatedRoute

```ts
export class ServersComponent implements OnInit {  
  public servers: {id: number, name: string, status: string}[] = [];  
  
  constructor(private serversService: ServersService,  
              private router: Router,  
              private route: ActivatedRoute) { }  
  
  ngOnInit() {  
    this.servers = this.serversService.getServers();  
  }  
  
  onReload(){  
    this.router.navigate(['servers'], {relativeTo: this.route});  
  }  
  
```

## Passing Parameters to Routes
Ex: "users/1" for user id 1, etc.

we can do it by 'users/:id'
- The colon tells angular, this is a dynamic part of the path
app.module.ts
```ts
const appRoutes: Routes = [  
  { path: '', component: HomeComponent },  
  { path: 'users', component: UsersComponent },  
  { path: 'users/:id', component: UserComponent },      // THIS
  { path: 'servers', component: ServersComponent },  
];
```

Accessing and Fetching the Path Parameters
- here we need to inject ActivatedRoute object 
- to get the current route
- In our case The ActivatedRoute we injected will give us access to the id passed in the URL : Selected User

we already have the user structure defined, we want to get the user

```
this.route.snapshot.params['id']
```
this way we can access the property passed in our path parameter
(must be same) as that passed in app.module.ts

app.module.ts
```ts
const appRoutes: Routes = [  
  { path: '', component: HomeComponent },  
  { path: 'users', component: UsersComponent },  
  { path: 'users/:id/:name', component: UserComponent },  
  { path: 'servers', component: ServersComponent },  
];
```

user.component.ts
```ts
export class UserComponent implements OnInit {
  user: {id: number, name: string};

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.user = {
      id: this.route.snapshot.params['id'],
      name: this.route.snapshot.params['name']
    };
  }
}
```

Now we can access the user properties from the class in the view template using property binding or string interpolation

```html
<p>User with ID {{ user.id }} loaded.</p>  
<p>User name is {{ user.name }}</p>
```


### Fetching Route Parameters Reactively

we can hit dynamic paths with routerLink directive as well
```html
<a [routerLink]="['/users', 10, 'Anna']">Anna</a> // users/10/Anna
```

if we load anna through this, the object 'user' which is used to display in the view template is not changed so the name is not updated.
This is the default behaviour

To fix this we need to add params properties in the route 
params is an observable, that helps us to work with asynchronous tasks

user.component.ts
```ts
ngOnInit() {  
  this.user = {  
    id:this.route.snapshot.params['id'],  
    name: this.route.snapshot.params['name']  
  };  
  this.route.params.subscribe((params: Params) => {  
    this.user.id = params['id'];  
    this.user.name = params['name'];  
  })  
}
```
Now the changes are reflected as the path is updated

Note : 
- We can destroy subscription manually for our own observables
- Generally angular does it for you
```ts
export class UserComponent implements OnInit, OnDestroy {  
  user: {id: number, name: string};  
  paramSubscription: Subscription;  
  
  constructor(private route: ActivatedRoute) { }  
  
  ngOnInit() {  
    this.user = {  
      id:this.route.snapshot.params['id'],  
      name: this.route.snapshot.params['name']  
    };  
    this.paramSubscription = this.route.params.subscribe((params: Params) => {  
      this.user.id = params['id'];  
      this.user.name = params['name'];  
    })  
  }  
  
  ngOnDestroy() {  
    this.paramSubscription.unsubscribe();  
  }  
}
```


### Passing query parameters and Fragments
ex :  4200/users/10/anna?mode=editing#loading

new route in app.module.ts
```ts
{ path: 'servers/:id/edit', component: EditServerComponent },
```

when using a link in a template with `routerLink` directive to add query parameters we can add another attribute called `queryParams` with property binding which takes a js object as value where we can pass key value pairs for the query parameters.

server.component.html
```ts
<a  
  [routerLink]="['/servers', 5, 'edit']"  
  [queryParams]="{allowEdit: 1}"  
  href="#"  
  class="list-group-item"  
  *ngFor="let server of servers">  
  {{ server.name }}  
</a>
```

similarly we also have a fragment property
```html
<a  
  [routerLink]="['/servers', 5, 'edit']"  
  [queryParams]="{allowEdit: 1}"  
  fragment="loading"         // omitted the [] for manual value
  href="#"  
  class="list-group-item"  
  *ngFor="let server of servers">  
  {{ server.name }}  
</a>
```


### Passing Query Parameters and Fragments programmatically
html
```html
<button class="btn btn-primary" (click)="onLoadServer(1)">Load Server 1</button>
```
ts
```ts
onLoadServer(id: number){  
  // complex calculation  
  this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: '1'}, fragment: "loading"});  
}
```


### Retrieving Query Parameters and Fragments

we want to retrieve our query parameters and fragments in our edit-server component

There are two ways to retrieve ig : 
1. through snapshot 
```ts
ngOnInit() {  
  console.log(this.route.snapshot.queryParams);  
  console.log(this.route.snapshot.fragment);
```
- However if we have a chance of changing the queryparams or fragment from the page, it will not be updated in the snapshot
- Hence this approach is not recommended.

2. Using observables
```ts
ngOnInit() {  
  console.log(this.route.queryParams.subscribe());  
  console.log(this.route.fragment.subscribe());
```


## Child or Nested Routes

- For example : when we click a user/server, we want that server details to open next to the menu and not in a brand new page.
- Here we use Nested Routing

add another property `children` to the route to add an array of children to the route

```ts
{ path: 'servers', component: ServersComponent , children: [  
    { path: ':id', component: ServerComponent },  
    { path: ':id/edit', component: EditServerComponent },  
  ]},
```
/servers, /servers/:id, /servers/:id/edit  are available now

Now we have to locate where our child route components would be loaded.

we add another `<router-outlet>` element to the place in the parent component, where we want to display our child components.

```html
<div class="row">  
  <div class="col-xs-12 col-sm-4">  
    <div class="list-group">  
      <a  
        [routerLink]="['/servers', server.id]"  
        [queryParams]="{allowEdit: 1}"  
        fragment="loading"  
        href="#"  
        class="list-group-item"  
        *ngFor="let server of servers">  
        {{ server.name }}  
      </a>  
    </div>  
  </div>  
  <div class="col-xs-12 col-sm-4">  
   <router-outlet></router-outlet>                 // THIS
  </div>  
</div>
```


#### Configuring the Handling of Query Params

While navigating routes programmatically we can add another parameter to our .navigate method called "queryParamsHandling"
it takes a string value that can be 
- preserve : to keep the old query params for the next route as well
- merge : merge old query params with our new 
by default it drops the queryParams


```ts
// WITH PRESERVE
onEdit(){  
  this.router.navigate(['edit'], {relativeTo: this.route, queryParamsHandling: 'preserve'});  
}

// WITH MERGE and new parameters
onEdit() {
  this.router.navigate(['edit'], {
    relativeTo: this.route,
    queryParams: { mode: 'admin', section: 'info' }, // new params
    queryParamsHandling: 'merge' // merge with existing query params
  });
}
```


### Redirecting and Wildcard routes
handle 404 page not found error
redirect the user to a custom page

Redirecting to another path from the routes in app.module.ts
```ts
{path: 'not-found', component: PageNotFoundComponent },  
{path: '**something**', redirectTo: '/not-found'}
```
redirectTo is a alternative to component and redirects to the specified path

`"**"` is a wildcard route, it means if user hits any route then that component or redirecting is made
NOTE : make sure this path is the last path in the array of paths otherwise any other path below it would not be usable.


### Outsourcing our Route Configuration
- There can be many more routing and can take a lot of space in our app.module.ts 
- We can make a separate file to handle all the route paths 

Add all the routes to the file 
and also the imports which require the routes
also export the configured RouterModule and make it available in the main app.module.ts file 

app-routing.module.ts
```ts
import {NgModule} from "@angular/core";  
import {RouterModule, Routes} from "@angular/router";  
import {HomeComponent} from "./home/home.component";  
import {UsersComponent} from "./users/users.component";  
import {UserComponent} from "./users/user/user.component";  
import {ServersComponent} from "./servers/servers.component";  
import {ServerComponent} from "./servers/server/server.component";  
import {EditServerComponent} from "./servers/edit-server/edit-server.component";  
import {PageNotFoundComponent} from "./page-not-found/page-not-found.component";  
  
const appRoutes: Routes = [  
  { path: '', component: HomeComponent },  
  { path: 'users', component: UsersComponent, children: [  
      { path: ':id/:name', component: UserComponent }  
    ] },  
  { path: 'servers', component: ServersComponent , children: [  
      { path: ':id', component: ServerComponent },  
      { path: ':id/edit', component: EditServerComponent },  
    ]},  
  {path: 'not-found', component: PageNotFoundComponent },  
  {path: '**', redirectTo: '/not-found'}  
];  
  
@NgModule({  
  imports: [RouterModule.forRoot(appRoutes)],  
  exports: [RouterModule]  
})  
export class AppRoutingModule {  
  
}
```

app.module.ts
```ts
@NgModule({  
  declarations: [  
    AppComponent,  
    HomeComponent,  
    UsersComponent,  
    ServersComponent,  
    UserComponent,  
    EditServerComponent,  
    ServerComponent,  
    PageNotFoundComponent  
  ],  
  imports: [  
    BrowserModule,  
    FormsModule,  
    AppRoutingModule                    // THIS
  ],  
  exports:[RouterModule],  
  providers: [ServersService],  
  bootstrap: [AppComponent]  
})  
export class AppModule { }
```


## Guards
Guards in Angular are interfaces that control navigation to and from the routes.
They act as gatekeepers, determining whether a user can access a specific route or leave it.

to use a authentication guard we create a new service class "auth-guard.service.ts"
it implements the CanActivate interface 
the interface needs a canActivate() with the following variables

auth-guard.service.ts
```ts
export class AuthGuard implements CanActivate {  
  canActivate(route: ActivatedRouteSnapshot,  
              state: RouterStateSnapshot)Observable<boolean> | Promise<boolean> | boolean {  
  
  }}
```

In our case we are using a dummy authentication service class, which would be replaced by a database in real projects :
auth.service.ts
```ts
export class AuthService{  
  loggedIn = false;  
  
  isAuthenticated(){  
    const promise = new Promise(  
      (resolve, reject) => {  
        setTimeout(  
          () => {  
            resolve(this.loggedIn);  
          }, 800  
        );  
      }  
    );  
    return promise;
  }  
  
  login() {  
    this.loggedIn = true;  
  }  
  logout() {  
    this.loggedIn = false;  
  }  
}
```


SHIT I DIDN'T UNDERSTAND : AND IS DEPRECATED 
```ts
import {  
  CanActivate,  
  ActivatedRouteSnapshot,  
  RouterStateSnapshot,  
  Router,  
  CanActivateChild  
} from '@angular/router';  
import { Observable } from 'rxjs/Observable';  
import { Injectable } from '@angular/core';  
  
import { AuthService } from './auth.service';  
  
@Injectable()  
export class AuthGuard implements CanActivate, CanActivateChild {  
  constructor(private authService: AuthService, private router: Router) {}  
  
  canActivate(route: ActivatedRouteSnapshot,  
              state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {  
    return this.authService.isAuthenticated()  
      .then(  
        (authenticated: boolean) => {  
          if (authenticated) {  
            return true;  
          } else {  
            this.router.navigate(['/']);  
          }  
        }  
      );  
  }  
  
  canActivateChild(route: ActivatedRouteSnapshot,  
                   state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {  
    return this.canActivate(route, state);  
  }  
}
```


Then to use the guard we use canActivate property in the path in our routes array
```ts
{ path: 'servers',canActivate:[AuthGuard], component: ServersComponent , children: [  
    { path: ':id', component: ServerComponent },  
    { path: ':id/edit', component: EditServerComponent },  
  ]},
```

then add the new guard service we added to the app.module.ts in the providers array

however canActivate does not protect the child routes

CanActivateChild -> will protect all child routes as well

#### using canDeactivate : 
 did not make any notes


### Passing static data to a route
we can pass a data object in the route object, with values
ex : 
```ts
{path: 'not-found', component: PageNotFoundComponent, data: {message: 'Page not found'} },
```
now we can access the object within our ts component class

page-not-fount.component.ts
```ts
@Component({  
  selector: 'app-page-not-found',  
  templateUrl: './page-not-found.component.html',  
  styleUrl: './page-not-found.component.css'  
})  
export class PageNotFoundComponent implements OnInit {  
	errorMessage: string;
	constructor(private route: ActivatedRoute){}

	ngOnInit(){
		this.errorMessage = this.route.snapshot.data['message'];
	}
}
```

now we can use the errorMessage in our view template with string interpolation or property binding.

#### Passing dynamic data to a route
- did not make any notes


### Understanding Location Strategies
- The server has to be configured in a way that it serves the index.html instead of a 404 page in each case an unknown url is requested, so that Angular's router can handle the correct routing.
- [https://angular.io/guide/deployment#server-configuration](https://angular.io/guide/deployment#_blank)
- We are using fake urls here, unlike in a traditional website, where the urls usually reflect the real places of the subpages. In a SPA, when changing the url, we are not really moving to a different place but we only shift some content of the current view. Manipulating the urls in this way is only possible in modern browsers which support the History API. In older browsers you can mimic a similar behavior by using hashes for internal navigation on a page.
- This would be the same if you would write your app in plain JavaScript and not in Angular.

To Solve this issue we use another object with a property useHash in the RouterModule.forRoot in our routing module

```ts
@NgModule({  
  imports: [RouterModule.forRoot(appRoutes, {useHash: true})],  
  exports: [RouterModule]  
})
```

- By default usHash is false


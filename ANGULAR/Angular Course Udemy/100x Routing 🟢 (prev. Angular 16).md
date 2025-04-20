# Routing and Building Multi-page Single Page Applications


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
    RouterModule.forRoot(appRoutes)  
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
replacing href links with "/server", etc. works.
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


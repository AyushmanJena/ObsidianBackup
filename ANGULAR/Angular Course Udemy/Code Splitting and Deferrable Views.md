Improving Performance of our application
Specifically using Lazy Loading

## Lazy Loading
Lazy Loading means, certain pieces of application code are only loaded when it's needed.
Splitting the code into multiple chunks 
Resulting in smaller initial bundle size, application is up and running quicker.

Two Approaches provided by Angular : 
1. Route based Lazy Loading
2. Deferrable Views (angular 17 and higher)


# Routing Based Lazy Loading
For which routes lazy loading makes sense ?
- Lazy loading is useful for components/codes which are unlikely to be loaded at the start of the application.
- When you import a component you are loading it eagerly. 
- In lazy loading we call import as a function when required.

Adding lazy loading for TasksComponent 
- To perform lazy loading remove the "component" from the route object and add "loadComponent "property 
```ts
// BEFORE
{  
  path: 'tasks', // <your-domain>/users/<uid>/tasks  
  component: TasksComponent,  
  runGuardsAndResolvers: 'always',  
  resolve: {  
    userTasks: resolveUserTasks,  
  },

// AFTER
{  
  path: 'tasks', // <your-domain>/users/<uid>/tasks  
  loadComponent: () => import('../tasks/tasks.component').then(m => m.TasksComponent),  
  runGuardsAndResolvers: 'always',  
  resolve: {  
    userTasks: resolveUserTasks,  
  },
```
- we import the component only when we the route is activated
- the import returns a promise so call a then and handle the promise with a module value i.e. the file content we received, and hence we specify the component we want to access.

### Lazy Loading Entire Route Groups

```ts
// BEFORE  
{  
  path: 'users/:userId', // <your-domain>/users/<uid>  
  component: UserTasksComponent,  
  children: userRoutes,                         // this
  canMatch: [dummyCanMatch],  
  data: {  
    message: 'Hello!',  
  },  
  resolve: {  
    userName: resolveUserName,  
  },  
  title: resolveTitle,  
},

// AFTER 
{  
  path: 'users/:userId', // <your-domain>/users/<uid>  
  component: UserTasksComponent,  
  loadChildren: () => import('./users/users.routes').then(mod => mod.routes),  
  canMatch: [dummyCanMatch],  
  data: {  
    message: 'Hello!',  
  },  
  resolve: {  
    userName: resolveUserName,  
  },  
  title: resolveTitle,  
},
```
- Instead of implementing lazy loading on per route level for each component, we can mark group of routes to load lazily.

### Using Lazy Loading and Routing to Lazy Load Services
- Service code is eagerly loaded when application starts, 
- SInce we mention `@Injectable({ providedIn: 'root'})` , it is setup when the application starts

We can replace it with 
```ts
@Injectable({})  
export class TasksService {  
  private tasks = signal([  
    {
    ... 
```

in the routes we can add another route
```ts
 
export const routes: Routes = [  
	{
		path: '',
		providers: [
			TasksService
		], 
		children: [
		  {  
		    path: '',  
		    redirectTo: 'tasks',  
		    pathMatch: 'full',  
		  },  
		  {  
		    path: 'tasks', // <your-domain>/users/<uid>/tasks  
		    // loadComponent: () => import('../tasks/tasks.component').then(m => m.TasksComponent),    component: TaskComponent,  
		    runGuardsAndResolvers: 'always',  
		    resolve: {  
		      userTasks: resolveUserTasks,  
		    },  
		  },  
		  {  
		    path: 'tasks/new',  
		    component: NewTaskComponent,  
		    canDeactivate: [canLeaveEditPage]  
		  },  
		]
	},
  
];
```

add a wrapper route to the routes, with all the other routes inside as its children
add providers property to the route
- now all the nested routes and children mentioned will have access to the providers mentioned in the route.\
- Since the user routes are loaded lazily in app.routes.ts, therefore the providers mentioned will also be loaded lazily by angular.

#  Deferrable Views
- Used in newer versions of angular for lazy loading.

```html
@defer {
	<app-offer-preview/>
}
```
- use @defer and surround the component you want to load lazily within its curly braces

- you can use various triggers in parameters
- `on viewport` -> when viewable in the viewport, it needs a placeholder if it is not visible

```html
@defer(on viewport) {
	<app-offer-preview/>
} @placeholder {
	<p>We might have an offer...</p>
}
```

other triggers are : 
- on idle
- on interaction
- on hover
- on immediate
- on timer
check deferrable views documentation for details

### Prefetching Lazy loaded Code : 

- pre fetch the code in the background but dont display the component 
```html
@defer(on interaction; prefetch on hover) {
	<app-offer-preview/>
} @placeholder {
	<p>We might have an offer...</p>
}
```
- not noticeable on small code bases 
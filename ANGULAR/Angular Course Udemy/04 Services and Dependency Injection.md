- Revisiting Services
- Revisiting Dependency Injection DI
- Hierarchical Injectors and DI Resolution Process
- Injection Tokens and Services

### Services
Services allow you to share logic and data across multiple components in an application, can also be shared across directives or other classes as well.
- export class TasksService
- `@Injectable({providedIn:'root'})`

#### Creating a new service with angular CLI
`ng g s serviceName --skip-tests`

### Dependency Injection
You don't create service instances yourself - instead, you request them from Angular

 
```ts
	private tasksService = inject(TasksService);
```


- We want to share tasks from the service to the components using it, however we want it to be viewable outside and editable only inside the service class
- So we make the actual data private and make another property signal and make it public and readonly 
- and share the allTasks to other components 
- however while modifing in the actual service we edit the tasks
```ts
private tasks = signal<Task[]>([])
allTasks = this.tasks.asReadonly();
```


### Injectors available in Angular
Angular has multiple injectors :
1. NullInjector
2. Platform EnvironmentInjector
3. EnvironmentInjector (Application Root)
4. ModuleInjector (working with modules)
5. ElementInjector

Angular searches from buttom to top, if no injector is found then null injector is used


#### Injecting a service to entire application

main.ts
```ts
bootstrapApplication(AppComponent, {
	providers:[TasksService]
}).catch((err) => console.error(err));
```
Now you dont need to mark the service as Injectable but angular will detect it as a provider and is available throughout the application 

this approach does not optimise the code as is available throughout the code runtime, therefore not recommended


### Using ElementInjector to inject elements
- ElementInjector is closely tied to your DOM elements and directives
- In this case only the components or directives which inject the element or service have access to the service, other components do not have access to the service
- The child components also have access to that service

- remove Injectable from the service class
- add providers array to the @Component decorator config object and set up values that are injectable 
```ts
@Component({
	...
	providers:[TasksService]
})
export class TasksComponent{
	private tasksService = inject(TasksService);
	...
}
```

- However if you use the TasksComponent multiple times in its parent there will be multiple instances of the TasksService, therefore different data in each instance and changes will not be reflected in both uniformly.


NOTE : 
- You can use one service inside another service similar to using them in components, inject them and use as objects


### Using Custom Dependency Injection Tokens and Providers

Custom token for using Dependencies
By default the dependency token is the class name 
you can change it in the main.ts by using the ElementInjector method

Ex : 
```ts

export const TasksServiceToken = new InjectionToken<TasksService>('tasks-service-token');

bootstrapApplication(AppComponent, {
	providers;[{provide: TasksServiceToken, useClass: TasksService}],
}).catch((err) => console.error(err));
```

The actual dependency is `TasksService` class, however when using it in the component we need to specify the provider as `TasksServiceToken` now with the type of `TasksService` specified 

- tasks-service-token is a description and is useful for debugging and production purposes.


### Injecting Non-Class values 

data to inject : 
- also use Custom Dependency Injection token 
task.model.ts
```ts
export const TASK_STATUS_OPTIONS = new InjectionToken<TaskStatusOptions>('task-status-options');

type TaskStatusOptions = {
	value: string;
	taskStatus: TaskStatus;
	text: string;
}[];

export const TaskStatusOptions: TaskStatusOptions = [
	{
		value: 'open',
		taskStatus: 'OPEN',
		text: 'Open'
	},
	{
		value: 'in-progress',
		taskStatus: 'IN_PROGRESS',
		text: 'In Progress'
	},
	...
]
```

Injecting the data in a component

```ts
@Component({
	...
	providers: [{
		provide: TASK_STATUS_OPTIONS,
		useValue: TaskStatusOptions
	}]
})
export class TasksListComponent { 
	tasksStatusOptions = inject(TASK_STATUS_OPTIONS);
	...
}
```

- Since we are no longer using a class, by default useClass is not used anymore, therefore we need to specify useValue  for a particular value or useFactory for some value that is generated dynamically.
- useValue takes a simple value
- useFactory takes a function 

# Dependency Injection with Modules

app.module.ts
```ts
@NgModule({
	declarations: [
		...
	],
	imports: [...],
	bootstrap: [AppComponent],
	providers: [{
		provide: TasksServiceToken, useClass: TasksService
	}],
})
export class AppModule{}
```

this makes what you set in providers to the entire application and inject them as usual
We can also use custom tokens, shortcuts for injecting, etc as usual.



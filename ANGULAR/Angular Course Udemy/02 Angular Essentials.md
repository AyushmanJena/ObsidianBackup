# Project Directory Structure : 
### root
- All files found on the root levels are essentially configuration files.
1. tsconfig.app.json, tsconfig.json, tsconfig.spec.json : 
	- mostly default code for these should work.
2. package.json :
	- packages whichi are required by the project
3. Angular.json :
	- config file for json

### src : 
styles.css : stylesheet file
index.html : main html file
faviocn.ico : icon
main.ts : the first code file to be executed when the app loads up

assets folder: 
- contains assets like images and other files required

app folder: 
- where you will put all your angular components. 
- this is the component you spend most time working with.


### How Components work in Angular
`app.component.ts`
```ts
import  {Component} from '@angular/core';

@Component({
	selector: 'app-root',
	standalone: true,
	imports: [],
	templateUrl: './app.component.html',
	styleUrl: './ap.component.css',
})
export class AppComponent {}
```
- @Component () -> is a decorator and stores the metadata for the component
- The markup of the component is stored in the templateUrl and style in styleUrl
- this AppComponent is used in the code like this : 
`main.ts`
```ts
import {bootstrapApplication} from '@angular/platform-browser';
import {AppComponent} from './app/app.component';
bootstrapApplication(AppComponent).catch((err) => console.error(err));
```
note : for .ts files we do not specify the extension as above

### Type Aliases : 
Instead of creating objects we can create a layout for the object (something like class in java)

```ts
type User = {
	id : string;
	avatar: string;
	name: string;
}
```

or create a interface
```ts
interface User{
	id :string;
	avatar: string;
	name: string;
}
```

Now you can use the User instead of creating a object type for every variable in the file.

- With interface you can only define object types whereas with type you can create some other types as well.


### Using @For for loops
```html
@for (user of users; track user.id){
	<li>
		<app-user .../>
	</li>
}
```

track user.id -> unique identification for each element it is rendering which helps for more efficient rendering.

- Alternative : `*ngFor` structural Directive in Youtube notes

### Using @If for conditions
```html
@If(selectedUser){               // if selcted user is defined 
	<div></div>
} @else {
	<p>User not selected</p>
}
```
- You can also add @else if
- Alternative `*ngIf`, but for else you need reference variable  


### Creating Model files
note : outsource the models into separate files
user.model.ts : 

export interface User {
	id: string;
	avatar: string;
	name: string; 
}
- now you can import User into all the components that use it. 

### Dynamic CSS Styling with class binding
```html
<button [class.active]="selected" (click)="onSelectUser()">
	<div>...</div>
</button>
```

class -> property
active -> name of class 
selected -> a property in ts class which holds a boolean property
true means the active style is applied else it is not applied.

deleting task : 49 (might need a review)


### Two way data binding with Signals and ngModel
```ts
export class NewTaskComponent{
	enteredName = signal('');
}
```

```html
<input type="text" name="name" [(ngModel)]="enteredName">
```


### Pipes 
- Output transformers
- Things that transform output in templates.

`|` -> pipe symbol

```html
<time>{{task.dueDate | date}}</time>
```
- date -> angular built in pipe, import DatePipe from @angular/common
- It transforms the string date into human readable form.
- Input : 2025-08-24 -> Aug 24, 2025
- You can also specify the output format or configurations as well, check documentation of DatePipe

You can build your own pipes as well (later)


## Services
- Keeping components and their classes as simpler as possible by outsourcing the logic to other classes called services.
- Or same code logic is used by multiple components.

create a folder in the component which uses the service : 
tasks.service.ts
```ts
export class TasksService{
	private tasks = [
		{
			id: 't1', 
			userId: 't2',
			title: 'xyz',
			summary: 'xyz',
			dueDate: '2025-12-31'
		},
		...
	]

	getUserTasks(userId: string){
		return this.tasks.filter((task) => task.userId === userId);
	}

	addTask(taskData: TaskData){
		this.tasks.add()
		...;
	}
}
```

- make the data private and use methods to access the data 

### Dependency Injection
import the TasksService
We need to instantiate the class, however alternatively we can use Dependency Injection.
i.e. the object will be created by angular and managed by it as well.
-  Different components will use the same object in memory and therefore on the same data.

```ts
export class TasksComponent {
	private tasksService: TasksService;
	
	constructor(tasksService: TasksService){
		this.tasksService = tasksService;
	}

	get selectedUserTasks() {
		return this.tasksService.getUserTasks(this.userId);
	}
}
```

OR

```ts
export class TasksComponent {
	// using shortcut
	constructor(private tasksService: TasksService){}


	get selectedUserTasks() {
		return this.tasksService.getUserTasks(this.userId);
	}
}
```

OR 
- use inject()
```ts

export class TasksComponent {
	private tasksService = inject(TasksService);

	get selectedUserTasks() {
		return this.tasksService.getUserTasks(this.userId);
	}
}
```

Important : Angular automatically will not find the class you want to inject, therefore you need to mark the service class as something that can be injected with @Injectable with a configuration object

```ts
@Injectable({providedIn: 'root'})
export class TasksService{
	...
}
```


### Using LocalStorage for data storage
- The `localStorage.getItem()` method is used to retrieve the value associated with a specific key from the browser's local storage.

```ts
constructor() {
	const tasks = localStorage.getItem('tasks');

	if(tasks){
		this.tasks = JSON.parse(tasks);  // string to json and store it
	}

	private saveTasks(){
		localStorage.setItem('tasks', JSON.stringify(this.tasks)); // json to str
	}
}
```

now call saveTasks() after removing and adding tasks to save the snapshot to the browser localStorage.



MORE NOTE : 
- adding new data to signals need it to be updated as follows : 
```ts
tasks = signal<Task[]>([]);

...
const newTask: Task = {data: "xyz"}
tasks.update((oldTasks) =. [...oldTasks, newTask]);
```

or updating a signal

```ts
updateTaskStatus(taskId: string, newStatus: TaskStatus){
	this.tasks.update((oldTasks) => oldTasks.map((task) => task.id === taskId ? {...task, status: newStatus} : task));
}
```
- here we want to update the task status of a particular task based on id, 
- we loop over all the tasks and find the task whose id matches the clicked task then update it with the new task object with changed taskstatus value

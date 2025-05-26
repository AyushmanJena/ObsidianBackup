- Includes notes from the youtube tutorial 
- and more are here

### Adding assets
- Add images to the code 
```html
<img src="assets/image.png" alt="this"/>
```
- Make sure angular.json assets array looks like this : 
```json
"assets":[
	"src/assets",
	"src/favicon.ico"
]
```
- Now you can access all files in the assets throughout the project


- Note : You cannot access private members of a component.ts file in its view template.

### Property binding
- Using getters to get values from the component.ts file 
- Instead of using ts expressions within property binding, we can use getter methods to handle ts login within the class file and just access the property with computed value
- ex : selectedUser.avatar : user.png but we want to access the image like "/assets/users/user.png"
```ts
get imagePaht(){
	return '/assets/users/' + this.selectedUser.avatar;
}

// then use the value in our view template

<img [src]= "imagePath()"/>
```


### Listening to Events with Event Listeners
event name between parenthesis in the view template 
ctrl + space gives all the possible values within the parenthesis

```ts
export class UserComponent {  
  selectedUser = DUMMY_USERS[randomIndex];  
  
  get imagePath() {  
    return 'assets/users/' + this.selectedUser.avatar  
  }  
  
  onSelectUser() {  
    const randomIndex = Math.floor(Math.random() * DUMMY_USERS.length);  
    this.selectedUser = DUMMY_USERS[randomIndex];  
  }  
}
```

```html
<div>  
  <button (click)="onSelectUser()">  
    <img [src]="imagePath" [alt]="selectedUser.name"/>  
    <span>{{ selectedUser.name }}</span>  
  </button></div>
```
- This approach uses zone.js internally to detect changes
- works automatically, no special instructions required
- 
# SIGNALS
- Alternative to zone.js based method we can use Signals

A signal is an object that stores a value (any type of value, including nested objects)
- Angular manages subscriptions to the signal to get notified about value changes
- When a change occurs, Angular is then able to update the part of the UI that needs updating.

steps :
1. import signal from angular core
2. create a signal value and store it in a property value
	- you can pass a initial value to the signal
3. You can change the value of the signal by calling the set() on the signal object


```ts
export class UserComponent {  
  selectedUser = signal(DUMMY_USERS[randomIndex])                 // THIS
  
  get imagePath() {  
    return 'assets/users/' + this.selectedUser.avatar;  // this is incorrect tho
  }  
  
  onSelectUser() {  
    const randomIndex = Math.floor(Math.random() * DUMMY_USERS.length);  
    this.selectedUser.set(DUMMY_USERS[randomIndex]);                 // THIS
  }  
}
```

4. To use the signal value in the view template you cannot access them like normal properties, you have to call them through functions

```html
<div>  
  <button (click)="onSelectUser()">  
    <img [src]="imagePath" [alt]="selectedUser().name"/> // not selectedUser.name
    <span>{{ selectedUser().name }}</span>  
  </button></div>
```

- Now this part will be re-rendered every time the signal value changes 

computed() is used to **derive values** based on one or more signals, and it **automatically updates** when those signals change.

why?
- when using signal receives a new value only then angular will recompute imagePath only due to the computed method
```ts
imagePath = computed(() => 'assets/users/'+ this.selectedUser().avatar); 
// instead of the method imagePath
```
- just like signals you will need to access this through method
`imagePath()` and not like normal parameters



### Adding Component Inputs 
- Passing values to the components where they are called

- create properties in the ts class of the data you want to accept and give them decorator "@Input"
- now set the property value for the component using the property name attribute in the tag using property binding..

in parent where called
```html
<app-user [avatar]="users[0].avatar" [name]="users[0].name"/>
```
- make sure users is defined or imported in the parent to use its values

user.component.ts
```ts
export class UserComponent {
  @Input() avatar!: string;
  @Input() name!: string;

  get imagePath() {
    return 'assets/users/' + this.avatar;
  }

  onSelectUser() {

  }
}

```

user.component.html
```ts
<div>  
  <button (click)="onSelectUser()">  
    <img [src]="imagePath" [alt]="name"/>  
    <span>{{ name }}</span>  
  </button></div>
```


- now we dont have a selectedUser property, but just a name and avatar property

note : ! -> the value will be set later 
but the value can be left out while calling the component in parent
to solve this issue we pass a configuration within the @Input()

```ts
@Input({required: true}) avatar!: string;
```
similarly there are other configurations as well


### Using Signal Inputs 
- alternative to @Input

import input from angular/core 

```ts
avatar = input(''); //default value passed
or
avatar = input<String>(''); // stating the type of data to be received
```

user.component.ts
```ts
export class UserComponent {  
  avatar = input.required<string>();  
  user = input.required<string>();  
  
  get imagePath() {  
    return 'assets/users/' + this.avatar;  
  }  

  // also use computed() due to signal
  imagePath = computed(() => {  
	  return 'assets/users/' + this.avatar();  
	});

  onSelectUser() {  
  
  }  
}
```


since the values are now signals and not normal values we must use them as signals with method calls in view templates

```html
<div>  
  <button (click)="onSelectUser()">  
    <img [src]="imagePath()" [alt]="name()"/>  
    <span>{{ name() }}</span>  
  </button></div>
```


rest code stays same in the parent component call tag

you can use `input.required<string>()` -> to tell the value is required ( but initial value cannot be passed anymore)


NOTE : 
the signal values are READ ONLY, i.e. they react to changes outside the user component, but you cannot change the value within the user component with this approach.
you cannot call set() on input signals.


### @Output
- when a button in the user component (child) is clicked we want the app component (parent) to know about it and make changes 
using @Output decorator

// this event emitter will allow us to emit custom values through the select properteis to any parent component that wants the value

user.component.ts (child)
```ts
@Output() select= new EventEmitter();

onsSelectUser() { // the method called in child as event
	this.select.emit(this.id);
}
```

in the parent component we can listen to the event in the parent component
```ts
onSelectUser(id: string){
	console.log(id);
}
```
```html
<app-user [avatar]="users[0].avatar" [name]="users[0].name"
		(select)="onSelectUser($event)"/>
```
- $event is a variable provided by angular (more notes in yt notes)

alternative to @Output we can also use output() 
- does the same thing but simpler (not really)
```ts
// @Output() select= new EventEmitter();
select = output<string>();

onsSelectUser() { // the method called in child as event
	this.select.emit(this.id);
}
```
view template stays same 

### Accepting object as input

```ts
@Input({required: true}) user: {
	id: string;
	avatar: string;
	name: string;
};
```

now access through user : user.id, user.avatar, etc. in the code as well as view template 

in parent where it is called
```
<app-user [user]="users[0]"/>
```
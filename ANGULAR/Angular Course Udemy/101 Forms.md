Angular's job is to
- Display the form to the user
- Take input from the user
- Check if the form is valid 
- (Send the form data to a backend)

What angular does is it gives you a javascript representation of the form along with metadata making it easier to perform operations based on it.

### TWO APPROACHES TO HANDLING FORMS : 
1. Template Driven : Angular infers the Form Object from the DOM
2. Reactive : Form is created programmatically and synchronized with the DOM

[[#Template Driven Approach]]
[[#Reactive Forms]]
# Template Driven Approach
- Create the form 
- Import FormsModule in app.module.ts (if not already)

Angular will not detect  inputs for your form automatically
We need to make controls on our own (in our class)

Now we will tell angular what we want to do with the form 

ngModel is used : (ngModel : used for two way data binding)
- no parenthesis or square brackets 
- ngModel in the input tag is enough to tell angular that this is a control of my form

- For this to be recognized by our angular class we need to give it the `name` of the control (from the html attribute)

How to submit : 
since default submit will trigger default behavior of the HTML form, it will trigger a javascript submit event. 
**Angular gives us a directive we can add to form element `(ngSubmit)="function()"`,** 
**This will fire when the submit is clicked.** 

We can add a template reference variable to our form element and can access the form in our code.  

add value "ngForm" to the reference variable to tell angular to get access to the object created by angular


SUMMARY :
1. Add `ngModel` to input fields
2. Add `name="xyz"` to input fields
3. Add `(ngSubmit)="function()"` to form element
4. Add a template reference to form element with value `"ngForm"`
5. pass the reference through the function and accept the same in the code 

CODE : 
```html
<form (ngSubmit)="onSubmit(f)" #f="ngForm">  // THIS
  <div id="user-data"> 
    <div class="form-group">
      <label for="username">Username</label>
      <input type="text"
        id="username"
        class="form-control"
        ngModel                           // THIS
        name="username">                   // THIS
    </div>
    <button class="btn btn-default" type="button">Suggest an Username</button>
    <div class="form-group">
      <label for="email">Mail</label>
      <input type="email"
         id="email"
         class="form-control"
         ngModel
         name="email">
    </div>
  </div>
    <div class="form-group">
      <label for="secret">Secret Questions</label>
      <select id="secret" class="form-control"
          ngModel
          name="secret">
          <option value="pet">Your first Pet?</option>
          <option value="teacher">Your first teacher?</option>
      </select>
    </div>
      <button class="btn btn-primary" type="submit">Submit</button>
</form>

```

```ts
export class AppComponent {  
  suggestUserName() {  
    const suggestedName = 'Superuser';  
  }  
  
  onSubmit(form: ElementRef){   // if #f = "ngForm" not mentioned
    console.log(form);  // does not give access to js form object
  }  

onSubmit(form: NgForm){   // if #f = "ngForm"
  console.log(form);  
}

}
```

Now we can get the javascript object, from which we can access the value of the form data and its metadata as well. (with NgForm)

We have properties like : 
- touched
- dirty
- disabled
- invalid
- etc. 


### Accessing the form with @ViewChild
- You dont have to submit the form as above (not the only way)
- We can use @ViewChild as well

```html
<form (ngSubmit)="onSubmit()" #f="ngForm">
...
</form>
```

```ts
export class AppComponent {  
  
  @ViewChild('f') signupForm: NgForm;  
  
  onSubmit(){  
    console.log(this.signupForm);  
  }  
}
```

- This approach is useful when you want to access the form, not only after submitted but also earlier.


### Validation of Template Driven form
- With html templates like required and email also works
-  This way angular dynamically adds some classes to elements based on the metadata like ng-invalid, ng-dirty, ng-touched, etc.

Which Validators do ship with Angular? 

Check out the Validators class: [https://angular.io/api/forms/Validators](https://angular.io/api/forms/Validators) - these are all built-in validators, though that are the methods which actually get executed (and which you later can add when using the reactive approach).

For the template-driven approach, you need the directives. You can find out their names, by searching for "validator" in the official docs: [https://angular.io/api?type=directive](https://angular.io/api?type=directive) - everything marked with "D" is a directive and can be added to your template.

Additionally, you might also want to enable HTML5 validation (by default, Angular disables it). You can do so by adding the `ngNativeValidate`  to a control in your template.

Directly applying properties through html elements
ex: disabling the submit button if form is invalid
```html
<form (ngSubmit)="onSubmit()" #f="ngForm">
...
<button type="submit" [disabled]="!f.valid"></button>
</form>
```

adding styles
```css
input.ng-invalid.ng-touched {  
  border:1px solid red;  
}
```


### Accessing html elements locally quickly
Just like ngForm directive you added to the form element, you can create a reference for a single element in html with ngModel directive
- Quick and easy access
Ex : 
```html
<input type="email"  
       id="email"  
       class="form-control" 
       ngModel name="email" 
       required email  
#email="ngModel">                     // THIS
<span class="help-block" *ngIf="!email.valid && email.touched">Enter valid email</span>
```


#### Set Default values with ngModel property Binding
```html
<select id="secret"
class="form-control"  
ngModel
name="secret"
[ngModel]="defaultQuestion">                   // THIS
  <option value="pet">Your first Pet?</option>  
  <option value="teacher">Your first teacher?</option>  
</select>
```

ts class
```
defaultQuestion = "pet";
```

### Using ngModel with two way binding
- Untill now we get the form details after we click submit, then we check if the value is valid or other such operations.
- But we can also check as soon as data is entered or touched, etc.

```html
<textarea 
	name="questionAnswer"
	rows="3"
	[(ngModel)] = "answer"> </textarea>

<p>Repeated : {{answer}}</p>
```

ts 
```
answer=''; //default value
```
- using ngModel we get updated with each keystroke
- but if we submit it the final answer is submitted with the form


### Grouping Form Controls

- needs a div holding multiple form element like inputs
- using ngModelGroup 

```html
<div id ="..." ngModelGroup="userData" #userData="ngModelGroup">
...
...
</div>
```

- we can get metadata for  multiple elements through the group as a whole

### Handling Radio Buttons
```html
<form...>
...
<div class="radio" *ngFor="let gender of genders">
	<label>
		<input 
			type="radio"
			name="gender"
			ngModel
			[value]="gender">
			{{gender}}
	</label>
...
</form>
```

ts
```ts
genders=['male', 'female'];
```


### Setting and Patching Form Values

this.signupForm.setValue() here we need to pass a js object exactly representing our form
```ts
@ViewChild('f') signupForm: NgForm;
suggestUserName(){
	this.signupForm.setValue({
		userData : { // a group in the form with ngModelGroup
			username: "chickenNugget",
			email: '',
		},
		secret : 'pet',
		questionAnswer:'',
		gender: 'male'
	})
}
```
- This approach has a downside
- If we already have some data entered, it will be replaced 

alternative : 
using `patchValue()`
- To overwrite only specific controls
```ts
@ViewChild('f') signupForm: NgForm;
this.signupForm.form.patchValue({
	userData : {
		username: "chickenNugget";
		}
	})
```
- Only the username will be overwritten or modified
- Other controls will remain untouched

## Getting the username form and using it
- We want to create an object to hold the form data and also want to display the details in the webpage as well 

```ts
@ViewChild('f') signupForm: NgForm;

user = {
	username:'',
	email:'',
	secretQuestion:'',
	answer:'',
	gender:''
};
submitted = false;
...
onSubmit(){
	this.user.username = this.signupForm.value.userData.username;
	this.user.email = this.signupForm.value.userData.email;
	this.user.secretQuestion = this.signupForm.value.secret; // name value diff
	this.user.answer = this.signupForm.value.questionAnswer;
	this.user.gender = this.signupForm.value.gender;
	this.submitted= true;
}

```

displaying data
```html
<div class="row" *ngIf="submitted">
	<h3>Your Data </h3>
	<p>UserName : {{user.username}}</p>
	...
</div>
```

### Resetting the form
- Now that we have extracted and displayed the data in our page, we want to reset the form 
```ts
this.signupForm.reset(); // can be called after setting in onSubmit
```


# Reactive Forms 
- Form is created programmatically and synchronized with the DOM
- You dont need the FormsModule for Reactive forms approach
- Import **ReactiveFormsModule**

Creating a form object in our class of type FormGroup 
- **FormGroup** : A class made by angular for holding form data
Initialize the form in a method (preferably OnInit lifecycle hook) 
Add controls to the form object
- **Controls** : Controls are basically key value pairs in the object, that are passed to the overall form group.
- **FormControl** :  another class from forms package that tracks the value and validation status of an individual form input element.
- The FormControl constructor takes few parameters 
	1. Initial value 
	2. single or array of validators
	3. async validators

#### Syncing the form
Now we need to sync the ts class form with the html view template form
1. `[formGroup]` directive to the form element with the formGroup variable as value
2. *formControlName* directive to the elements of form to link the formControl variables with the tags (must match)

CODE : 
```html
 <form [formGroup]="signupForm">                   // THIS
   <div class="form-group">
     <label for="username">Username</label>
     <input
       type="text"
       id="username"
       formControlName="username"                  // THIS
       class="form-control">
   </div>
   <div class="form-group">
     <label for="email">email</label>
     <input
       type="text"
       id="email"
       formControlName="email"
       class="form-control">
   </div>
   <div class="radio" *ngFor="let gender of genders">
     <label>
       <input
         type="radio"
         formControlName="gender"
         [value]="gender">{{ gender }}
     </label>
   </div>
   <button class="btn btn-primary" type="submit">Submit</button>
 </form>
```

```ts
import {Component, OnInit} from '@angular/core';
import {FormControl, FormGroup} from "@angular/forms";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  genders = ['male', 'female'];
  signupForm: FormGroup;

  ngOnInit() {
    this.signupForm = new FormGroup({
      'username': new FormControl(null),       // default value passed
      'email': new FormControl(null),
      'gender': new FormControl('male'),
    })
  }
}

```

### Submitting the Form
ngSubmit directive with a method as value on the form element
But this time it is different from the template driven approach, this time we won't get any form anymore 

```ts
onSubmit(){  
  console.log(this.signupForm.value);  
}
```
```html
<form [formGroup]="signupForm" (ngSubmit)="onSubmit()">
...
</form>
```

### Adding Validation
- required and email like attributes do not work in the reactive form approach
- You configure the validators in the ts code

Use Validators class with method reference (no parenthesis) as second parameter
Ex : Validators.required 
For multiple validations pass an array of validator methods

```ts
ngOnInit() {  
  this.signupForm = new FormGroup({  
    'username': new FormControl(null, Validators.required),  
    'email': new FormControl(null, [Validators.required, Validators.email]),  
    'gender': new FormControl('male'),  
  })  
}
```

### Getting Access to Controls 
- In template driven approach we need to add a template reference for the element to maybe display a invalid message
- But its easier in Reactive approach

In Reactive form we can access the overall formgroup followed by a get method 
The get method allows us to get access to the controls of our form
Within the method we can specify the control name or the path to the control

```html
<span *ngIf="!signupForm.get('username').valid && signupForm.get('username').touched" class="help-block">Enter Valid Username</span>
```

### Grouping Controls 
Instead of FormControl we use FormGroup class inside the main FormGroup (actual complete form) and we can add our FormControls within it as usual

```ts
ngOnInit() {  
  this.signupForm = new FormGroup({  
    'userData': new FormGroup({  
      'username': new FormControl(null, Validators.required),  
      'email': new FormControl(null, [Validators.required, Validators.email])  
    }),  
    'gender': new FormControl('male'),  
  })  
}
```

to continue syncing with the html template
we can add the grouped control elements within another group with formGroupName directive

Note : The path of accessing the control also changes as per the groups separated by dot
```html
<span *ngIf="!signupForm.get('userData.email').valid && signupForm.get('userData.email').touched" class="help-block">Enter Valid Email</span>
```

### Arrays of Form Control (FormArray)

What we want : 
- A button 
- When user clicks the button we want to dynamically add a control to form (array of controls)

Here we use FormArray, another class similar to FormControl but to store array of controls.

Now similar to FormControlName directive for FormControl, we have formArrayName for FormArray

```ts
ngOnInit() {  
  this.signupForm = new FormGroup({  
    'userData': new FormGroup({  
      'username': new FormControl(null, Validators.required),  
      'email': new FormControl(null, [Validators.required, Validators.email])  
    }),  
    'gender': new FormControl('male'),  
    'hobbies': new FormArray([])  
  })  
}  
  
...
onAddHobby(){  
  const control = new FormControl(null, Validators.required);  
  (<FormArray>this.signupForm.get('hobbies')).push(control);  
}  
get controls(){  
  return (this.signupForm.get('hobbies') as FormArray).controls;  
}
```

```html
<div formArrayName="hobbies">  
  <h4>Your Hobbies</h4>  
  <button class="brn btn-default" type="button" (click)="onAddHobby()">Add Hobby</button>  
  <div class="form-group" *ngFor="let hobbyControl of controls; let i=index">  
    <input type="text" class="form-control" [formControlName]="i">  
  </div>  
</div>
```

### Adding Custom Validators

- for example add usernames not allowed

1. create an array of names not allowed
2. create a function in the ts class
3. it needs to accept a parameter a control (FormControl, etc.) and return a js object

```ts
nameIsForbidden=['ayush', 'naruto'];
forbiddenNames(control: FormControl): {[s:string]: boolean}{
	if(this.forbiddenUsernames.indexOf(control.value) !== -1){ // since -1 is true
		return {'nameIsForbidden': true};
	}
	return null; // if validation is successfull pass null IMPORTANT
}
```

now use it in the FormControl : 
```ts
'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]),
```

### Using Error Code
```html
<span *ngIf="signupForm.get('userData.user').errors['nameIsForbidden']">Invalid Name</span>
```

- This can be used for normal validator errors 
- To check names of the validation errors check the form object in console

### Creating a custom Async Validator
- You might need to react out to a web server to check validation

```ts
forbiddenEmails(control: FormControl): Promise<any> | Observable<any>{
	const promise  = new Promise<any>((resolve, reject) => {
		setTimeout(() => {
			if(control.value === 'test@test.com'){
				resolve({'emailIsForbidden': true});
			}else{
				resolve(null);
			}
		}, 1500);
	});
	return promise;
}
```

this is stored in 3rd parameter

```ts
'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails),
```

#### Reacting to Status or Value Change
We have two observables : 
1. statusChanges 
2. valueChanges

- Value Changes happens when we change anything about the form
```ts
ngOnInit() {  
  this.signupForm = new FormGroup({  
    'userData': new FormGroup({  
      'username': new FormControl(null, Validators.required),  
      'email': new FormControl(null, [Validators.required, Validators.email])  
    }),  
    'gender': new FormControl('male'),  
    'hobbies': new FormArray([])  
  });  
  this.signupForm.valueChanges.subscribe(  
    (value) => {console.log(value)}  
  );  
}
```

- You can also call these on individual or group form controls

- Status Changes is used exactly similarly
- It says if it is valid, invalid or pending
- Can be used for animations 


### Setting and Patching Values in Reactive forms
- To set and update values setValue() and patchValue() are used

```ts
this.signupForm.setValue({
	'userData': {
		'userName': 'Max',
		'email' : 'max@test.com'
	},
	'gender': 'male',
	'hobbies': []
});
```
- setValue() takes the whole js object
- It overwrites other values 

- patchValue() can take only part of the form and update it
```ts
this.signupForm.patchValue({
	'userData': {
		'userName': 'Moka',
	}
});
```

- Resetting the form
```ts
this.signupForm.reset();
```

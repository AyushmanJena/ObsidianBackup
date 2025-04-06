What is Angular ?
- Angular is a frontend JavaScript/Typescript framework. 
- It helps with building interactive, modern web user interfaces.
- It is also a collection of tools and features
- CLI, Debugging tools, IDE plugins ...
- Generally used for enterprise level web apps (same as spring boot)

Why use Angular ?
- Angular is not really useful for building simple web apps.
- However it simplifies the process of building complex, interactive web user interfaces.
- With angular you can write declarative code instead of imperative code. 

- Angular job is to figure out which steps should be taken to decide what changes to be made in the user interface. 
- You dont need to manually react out to the DOM elements. 

- Separation of concerns with Components.
- Components : Custom html elements (modular ui elements)

- some Object Oriented Programming concepts and principles
- dependency injection, classes, etc.

- Use of TypeScript 


### Angular CLI
- Makes project creation process easier
- Manages changes and optimizations during production
- Installin the Angular CLI : 
`npm install -g @angular/cli`

- To create a new angular project :
`ng new project-name`

Starting a local Development Server : 
`npm start`
or 
`ng serve`


# Project Directory Structure : 

note : In our angular project we can have multiple applications, and the default application is present in the app folder and one app component : `app.component`
- every angular application must have at least one component and one module.
### root
- All files found on the root levels are essentially configuration files.
1. tsconfig.app.json, tsconfig.json, tsconfig.spec.json : 
	- mostly default code for these should work.
2. package.json :
	- 3rd party packages which are required by the project
	- dependencies : dependencies of the angular project
	- devDependencies : dependencies of the project while being developed.
	- `npm install` to download and store the dependencies in node_modules
3. Angular.json :
	- config file for json
	- builder options, style pages, etc. 
4. package-lock.json : 
	- make sure the same version of the dependencies is installed 

### src : 
styles.css : stylesheet file
index.html : main html file
faviocn.ico : icon
main.ts : the first code file to be executed when the app loads up

assets folder: 
- contains assets like images, icons, text files and other files required
- everything in this folder will be public.

app folder: 
- where you will put all your angular components. 
- this is the component you spend most time working with.

node_modules : 
- Stores 3rd party library data that are needed in our application


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
- this AppComponent is used in the code like this : 
`main.ts`
```ts
import {bootstrapApplication} from '@angular/platform-browser';
import {AppComponent} from './app/app.component';
bootstrapApplication(AppComponent).catch((err) => console.error(err));
```
note : for .ts files we do not specify the extension as above


A component has mainly 3 files  : 
1. app.component.ts
2. app.component.html
3. app.component.css
4. app.component.spec.ts -> for testing

- @Component () -> is a decorator and stores the metadata for the component
- The markup of the component is stored in the templateUrl and style in styleUrl
- for a component the value of the selector can be used as an html element.

whenever we navigate around in the angular application, only the content of the html file will change, the content of the html file will not change. 
The single html page loaded into our browser is index.html.


## Bootstrapping Angular Application (The starting process)
- Bootstrapping is the process of initializing our angular app
- first index.html is loaded.
- ng serve : Angular CLI saves the compiled Angular application in the memory and directly starts it.
- if you make any changes to our app, angular CLI will recompile and restart it.
- Angular CLI uses WebPack to traverse through our angular app and it bundles JS and other files into one or more bundles.
- Then Angular CLI also injects the bundled JS and CSS files in the index.html

- When index.html is loaded, Angular core libraries and third party libraries are also loaded by that time.
- angular.json states the main entry point of our application 
	- `"main" : "src/main.ts";`

OVERALL : Angular Project -> index.html -> angular.json -> main.ts -> AppModule -> AppComponent -> View Template (the html component to be rendered)

# Components in Angular
- A component is a piece of user interface.
- Root component by convention is called App Component.
- The root component contains the entire application and other child components.
- Ab angular application is essentially a tree of components.
- Combining all these components together makes an Angular UI.

- We can split a webpage into multiple components and work on them separately.
- Ex. : Header, Nav bar, main component area, side bar, etc.
- All child components can have their own child components as well.

## Creating a component 
1. Create a typescript class and export it
2. Decorate the class with @Component decorator
3. Declare the class in main module file

- The child component should also go inside parent component folder
- In this case inside app

Create a folder "header" inside app folder
create a component file "header.component.ts" inside header
componentName.component.ts -> format
```ts
import { Component } from "@angular/core";

@Component({
    selector: "app-header",
    template: "<h3>eKart</h3>"
})
export class HeaderComponent{

}
```

Now we have to tell the angular application that there is a header component. And it should be loaded every time the app loads

in app.module.ts (add only if the app is non standalone)
```ts
...
import {HeaderComponent} from './header/header.component';

@NgModule({
	declarations :[
		AppComponent,
		HeaderComponent
	], ...
})
```

Then place the component in the html page : 
app.component.html 
```html
<app-header></app-header>
```

## View Template
- The view template of a component is a type of HTML that tells Angular how to render a component.
- When we have 2-3 lines of html using template property within @Component is okay.
- However when we have more complicated html 
- we use templateUrl and specify the path of the html.

header.component.ts
```ts
import { Component } from "@angular/core";

@Component({
    selector: "app-header",
    templateUrl: "./header.component.html"
})
export class HeaderComponent{

}
```

disadvantages of using template property : 
- It mixes the html and typescript code which makes the code less maintainable
- Since HTML is written as a string, if there is some error, we will nit know about it during compile time.
- If number of html lines is huge, it will be messy and not maintainable.

```ts
styles: ['a{text-decoration:none; margin:0 10px;}', ...],
or
styleUrls: ['./header.component.css', ...]
```
similar to templateUrl, styleUrls is preferred.

- In angular when you apply styles propeties to a component, that styles is applied only to that view template. And is not global. Not even the child components.
- Unless the other component is using the same styles file. 

#### Adding CSS styles Globally
- Using styles.css file in src

Font awesome icons : 
`@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.6.3/css/font-awesome.min.css');`


### Using Bootstrap for Styling
1.  install bootstrap : 
`npm install --save bootstrap` in terminal 
- Once you have installed bootstrap you need to rebuild your angular application
1. specify path of bootstrap file in angular.json you want to use
```
...
"styles": [
              "node_modules/bootstrap/dist/css/bootstrap.min.css"
              "src/styles.css"
            ],
...
```
or 
go to styles.css and another import statement
`@import "~../node_modules/bootstrap/dist/css/bootstrap.min.css"`


### Creating Components using Angular CLI 
command : 
`ng generate component component-name`
- It creates a component class decorated with @Component decorator
- Generates the view template and stylesheet for that component
- Registers the component class in the main module

### Types of Component Selector :
- we can use multiple types of selectors like :
1. HTML tag : `selector : 'app-nav'`
2. HTML attribute : `selector : '[app-nav]"'
3. CSS Class : `selector : '.app-nav'`
4. CSS ID : `selector : '#app-nav'`

Using a component as a html attribute : 
`<div app-nav></div>`

Using a component as a css class :
`<div class="app-nav"></div>`

Using a component as a css class :
`<div id="app-nav"></div>`

## Data Binding 
- Allows us to communicate a component class and its corresponding view template and vice versa
- Component class contains the UI Logic and the View Template contains the HTML rendered in the browser.

Component Class
```ts
export class MyComponent{
	title="edumy";
	message= "Online learning";
	display = false;

	onClick() {
		this.display = true;
	}
}
```

View Template 
```html
<div class="header">
    <div>{{title}}</div>
    <div>{{message}}</div>
    <button (click)="onClick()"></button>
    <div [hidden]="display">
        <p>Display This Message</p>
    </div>
</div>
```

`[hidden]` -> attribute/property
`(click)` -> event

Data binding is of two types : 
1. One way data binding - data can be accessed from component into its corresponding view or vise versa
2. Two way data binding - Binds data from component class to view template and view template to component class. It is a combination of property binding and event binding.

#### One way data binding : 
1. Component to View Template data binding
	Can be done in two ways : 
		1. String interpolation : `{{data}}`
		2. Property binding : `[property] = data`
2. View Template to Component Class data binding 
		Done with Event binding : `(data) = "expression"`

#### Two way data binding : 
- Can be done using `[ngModel]`


# One Way Data Binding
### String Interpolation :
- Add TS variables or properties to the Component class
```ts
export class ProductListComponent {
  name: string = "iphone";
  price : number = 999;
  color: string = "black";
}
```

- Then use the values of the properties in the view template within double curly brackets
```html 
<p>Name : {{name}}</p>
<p>Price : ${{price}}</p>
<p>Color : Matte {{color}}</p>
<p>Discounted Price : $799</p>
```


Creating an object instead of ts variables : 
```ts
export class ProductListComponent {
  product ={
    name: "iphone x",
    price : 789,
    color: "black",
    discount : 8.5,
    inStock : 0
  }

  getDiscountedPrice(){
    return this.product.price - (this.product.price * this.product.discount / 100)
  }
}
```

```html
<p>Name : {{product.name}}</p>
<p>Price : ${{product.price}}</p>
<p>Color : Matte {{product.color}}</p>
<p>Discounted Price : ${{getDiscountedPrice().toFixed(2)}}</p>
<p>{{product.inStock > 0 ? 'Only ' + product.inStock + ' items left' : 'Not in Stock'}}</p>
```

- you can also call functions through string interpolation 
- You can also write ts expressions within the brackets 

### Property Binding : 
- String interpolation is used to display a piece of data in HTML, such as displaying a title or a name.
- Property Binding lets us bind a property of a DOM object, for example the hidden property, to some data value. This can let us show or hide a DOM element, or manipulate the DOM in some other way.

Adding an image to our web page 
1. Paste the image in assets/images or public folder
2. in the object component.ts file add another property 
`pImage : "/iphoneImg.png"`
3. Use the property using property binding in view template
`<img [src]="product.pImage">`

applying property binding to a button : 
- check if the product is in stock and if it is not in stock disable buy now button 
```html
<button [disabled]="!(product.inStock > 0)">Buy Now</button>
```

Why use Property Binding when you can use String Interpolation : 
- We cannot use string interpolation for all types of html attributes
- `<button disabled={{!(product.inStock > 0)}}>Buy Now</button>` does not work
- for disabled, hidden, checked HTML attributes you have to use property binding syntax.

note : instead of square brackets you can also use bind-property : 
`bind-disabled ="..."`
but not recommended

### Attribute vs Property : 
Attribute : represents the initial value and does not change
	Ex : aria-label, aria-hidden, aria-control, data-name, 
Property : represents the current value and can change
	Ex : data-id, data-name, data-value, colspan

You cannot bind attributes with property binding, you need to bind them with attribute binding
Binding Attributes : 
`<input [value]="name" [attr.aria-hidden]="">`


### Event Binding #imp
- We use event binding to bind data from the view template to the component class

What we want : When user types something in the input field in the web page the data is stored in the name property

- when we change anything an input event would happen 
- This can be handled using event binding

- For event binding wrap the event name within parenthesis and assign it any ts expression 
```html
<input [value]="name" (input)="onNameChange()">
```

```ts
export class ProductListComponent {
  name :string = "John Doe";
  ...
  onNameChange() {
    this.name = 'Mark';
  }
}
```

- When an event occurs and event object is generated.
- When an input event occurs, input event object is generated.
- We can access that object using $event variable in the view template 
- Then we can access the event object properties / values in the ts file

```html
<input (input)="onNameChange($event)"><br>
```

```ts
onNameChange(event: any) { // can also use type besed on the event obj e.g. InputEvent here
	this.name = event.target.value;
    console.log(event.target.value);
}
```


# Two Way Data Binding
- Data flows from component to its view template and at the same time, from view template to component class
- It is a combination of Property binding and event binding
- Uses `[ngModel]`

Learn it with Search Text Box Functionality : 

```html
<div class="ekart--search--product">
    <input class="ekart-search-product-input" [value]="searchText" (input)="updateSearchText($event)">
    <button class="btn btn-search">Search</button>
</div>
<div class="ekart-search-result-for"><p><strong>Search result for: </strong> {{searchText}} </p></div>
```

```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-search',
  imports: [],
  templateUrl: './search.component.html',
  styleUrl: './search.component.css'
})
export class SearchComponent {
  searchText: string = "Mens watch";

  updateSearchText(event : any){
    this.searchText = event.target.value;
  }
}

```

- Here we are using both property binding and event binding.
- Property binding to send what value is stored in the searchText variable to display in webpage
- Event Binding to send what value has been typed in the input field to the searchText variable
- Together we can take the user input and display it in the webpage again.

INSTEAD OF USING both property binding and event binding we can use `[ngModel]` to achieve two way data binding.

```html
<input class="ekart-search-product-input" [value]="searchText" [(ngModel)]="searchText">
```
- combining both into ngModel
- However you need to import FormsModule in your ts file to use ngModel
updated component.ts file
```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';

@Component({
  selector: 'app-search',
  imports: [FormsModule],
  templateUrl: './search.component.html',
  styleUrl: './search.component.css'
})
export class SearchComponent {
  searchText: string = "Mens watch";

  updateSearchText(event : any){
    this.searchText = event.target.value;
  }
}

```


FOR OLDER OR NON STANDALONE PROJECTS : 

importing is slightly different
in app.module.ts
```ts
import {NGModel} from '@angular/forms';
...
imports:[
	...,
	NgModel
]
```

## Directives : 
- A Directive is an instruction to the DOM.
- Manipulate DOM, Change Behavior, Add/Remove DOM elements.
- Directives help you to extend HTML in a way.

- Directive can be of 3 types : 
	1. Component Directive : a simple angular component, with a template
	2. Attribute Directive : used to change the appearance or behavior of a DOM element. It does not have a template. It does not add or remove anything to the webpage.
		Ex : `<div changetoGreen>Some Content<div>`
		- We also have built in attribute directives like ngStyle and ngClass
		- The attributes can also be added conditionally
	3. Structural Directive : 
		- Structural Directive is used to add or remove a DOM element on the webpage.
		- It does not have any template as well.
		- YOU MUST USE A asterisk for structural directives.
		-  Some in built structural directives are ngIf, ngFor, ngSwitch
		- Ex : `<div *ngIf>Some Content<div>`

- We can use the selector of a directive like an html tag, css class or even id selector. However we mostly use directives like an HTML attribute.

- A Directive is also a typescript class, just like a component.
- It is decorated with @Directive

```ts
@Directive({
	selector: '[changeToGreen]'
})
export class ChangeToGreen{

}
```

```html
<div changeToGreen>Some Content</div>
```

- since the selector is within square brackets, we can use the directive like an html attribute.

#### ngFor Directive : 
- It is a structural directive
- The Angular ngFor directive iterates over a collection of data like an array, list, etc. and creates an HTML element for each of the items from an HTML template.
- It is used to repeat portion of HTML template once per each item for an itterable list.
- The ngFor directive is a structural directive. It manipulates the DOM by adding/removing elements from the DOM.

- Using ngFor for looping over items : 

```html
<div *ngFor="let item of [2, 3, 4, 5, 6, 8]">
    <p>Current element is : {{item}}</p>
</div>
```
- Need to add CommonModule to the component's @Component.imports

Looping over values from component.ts

```html
<div *ngFor="let item of listOfString">
        <p>Name : {{item}}</p>
    </div>
```
```ts
@Component({
  selector: 'product-list',
  imports: [CommonModule, SearchComponent],
  templateUrl: './product-list.component.html',
  styleUrl: './product-list.component.css'
})
export class ProductListComponent {
  listOfString: string[] = ["Mark", "Steve", "Marry", "John", "Sarah"];
}
```

Rendering menu items :
```html
<div class="ekart-menu">
    <a *ngFor="let menu of mainMenuItems" href="#">{{menu}}</a>
</div>

where  : 
  mainMenuItems: string[] = ["Home", "Products", "Sale", "New Arrival", "Contact"];
```

#### Rendering List of Complex Types : 
- Example : Array of Products

```html
<div class="ekart--products--container">
    <div class="ekart--product--item" *ngFor = "let prod of products">
        <div class="ekart-wishlist-sale-container" >
            <div class="ekart--add--to--wishlist">
              <i class="fa fa-heart-o" aria-hidden="true"></i>
            </div>
            <!-- <div class="ekart-on-sale-tag">75% OFF -->
        </div>
          <div class="ekart--product--image">
              <img [src]="prod.imageURL" class="ekart--product--image">
          </div>
          <div class="ekart--add--to--price">
              {{prod.price}}  
          </div>
          <div class="ekart--product--name">{{prod.name}}</div>
          <div class="ekart--product--category">{{prod.gender}} . {{prod.category}} . {{prod.brand}}</div>
        <div class="ekart--product--available-colors">{{prod.color.length}} Colors . Best Seller</div>
    </div>
</div>
```

```ts
import { CommonModule } from '@angular/common';
import { Component } from '@angular/core';

@Component({
  selector: 'product-list',
  imports: [CommonModule],
  templateUrl: './product-list.component.html',
  styleUrl: './product-list.component.css'
})
export class ProductListComponent {
  products = [
    {
      id: 1,
      name: "Nike React Infinity Run Flyknit",
      description: "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem .",
      brand: "NIKE",
      gender: "MEN",
      category: "RUNNING",
      size: [6, 7, 8, 9, 10],
      color: ["White", "Blue", "Black"],
      price: 160,
      discountPrice:140,
      is_in_inventory: true,
      items_left: 3,
      imageURL: "https://static.nike.com/a/images/c_limit,w_592,f_auto/t_product_v1/i1-665455a5-45de-40fb-945f-c1852b82400d/react-infinity-run-flyknit-mens-running-shoe-zX42Nc.jpg",
      slug: "nike-react-infinity-run-flyknit"
    }, ... 
    ];
}
```

#### ngIf Directive : 
- Also a structural directive `(*ngIf = "ts expression or condition")`
- Angular ngIf directive is a structural directive that is used to completely add or remove DOM element from the webpage based on a given condition.
- If conditon true -> add the element to webpage
- else -> remove the element
- Ex : if the search box is empty dont display "Search Results for"
```html
<div class="ekart--search--product">
    <input class="ekart-search-product-input" [value]="searchText" [(ngModel)]="searchText">
    <button class="btn btn-search">Search</button>
</div>
<div class="ekart-search-result-for"><p *ngIf="searchText != ''"><strong>Search result for: </strong> {{searchText}} </p></div>
```

```ts
@Component({
  selector: 'app-search',
  imports: [FormsModule, CommonModule],
  templateUrl: './search.component.html',
  styleUrl: './search.component.css'
})
export class SearchComponent {
  searchText: string = ""; // do not show anything when emtpy string

  updateSearchText(event : any){
    this.searchText = event.target.value;
  }
}
```


### ngStyle Directive
- It is an Attribute Directive (wrap within square brackets)
- It allows us to set many inline style of an HTML element using expression.
```html
<div class="ekart--product--in--stock" [ngStyle]="{fontWeight : 'bold'}">
            {{prod.is_in_inventory ? 'Available in Stock' : 'Not available in stock'}}
</div>
```
- note : in the example is_in_inventory returns a Boolean value
- Using ngStyle tag we can add a css style dynamically based on the given ts expression
- Ex : display in green if the product is available else show  in red
```html
<div class="ekart--product--in--stock" [ngStyle]="{fontWeight : 'bold', color: prod.is_in_inventory ? 'Green' : 'Red'}">
            {{prod.is_in_inventory ? 'Available in Stock' : 'Not available in stock'}}
</div>
```
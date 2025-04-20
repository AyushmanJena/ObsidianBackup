- A template reference variable is a variable which stores a reference to a DOM element, Component or Directive on which it is used.

## How to use Template Reference Variable on an HTML Element
usecase : 
as of now as soon as we type text in search box it shows the products with the same name
But we want to change it and search as soon as the user clicks the search button

-  We want to pass the value in the search box from the view template to the ts file class but we are not raising events when text is typed, but when the button is clicked. And the button cannot share what is inside the text field with $event. How do we do that?
- For that we use Template Reference Variable.

```html
<div class="ekart--search--product">  
    <input class="ekart-search-product-input" #searchInput>  
    <button class="btn btn-search" (click)="updateSearchText(searchInput)">Search</button>  
</div>  
<div class="ekart-search-result-for"><p *ngIf="searchText != ''"><strong>Search result for: </strong> {{searchText}} </p></div>
```

the `#searchInput` is a reference variable and stores a reference to the input element in the DOM.
Then pass it to the button with click event
which will be passed to the method as `HTMLInputElement` type
Now we can use this reference to manipulate the DOM

search.component.ts
```ts
  updateSearchText(inputEl : HTMLInputElement){
    // this.searchText = event.target.value;
    // console.log(inputEl.value);
    this.searchText = inputEl.value; // set the searchText value
    this.searchTextChanged.emit(this.searchText); // raise the event and emit the value 
  }
```

### How to use Template Reference Variable on a Component

usecase : 
whenever a user clicks on a product, we want to display the details of the product. 

steps : 
1. Create a Model Product.ts file: 
```ts
export class Product {
  id: number;
  name: string;
  description: string;
  brand: string;
  gender: string;
  category: string;
  size: number[];
  color: string[];
  price: number;
  discountPrice?: number;
  is_in_inventory: boolean;
  items_left: number;
  imageURL: string;
  slug: string;
}
```

2. In product-list.component 
- create a new variable selectedProduct of type Product
```
selectedProduct: Product;
```

3. In the html file of product-list bind a click event 
- to set the selectedProduct variable as the current product
product-list.component.html
```html
<div *ngFor="let prod of products" (click)="selectedProduct = prod">  
    <app-product [product]="prod" *ngIf="searchText === '' || prod.name.toLowerCase().includes(searchText)"></app-product>  
</div>
```

4. Create a new component  
product-detail component in the container folder
added html and css code

currently the product-detail component is displayed automatically with some dummy data. 
But we dont want that, we want such that the component is displayed only when a product is selected by the user.

in the parent component in which product-detail component is rendered we add a if condition to display the component
#### However 
we need to get if a product is selected from the product-list component and send the data to the product-detail component. Both are sibling components. Here we can use the old data binding with @Input and @Output 
We can also use Template Reference Variable on the product-list component
Then we can use the template reference variable to access the selectedProduct property.

container.component.html
```html
<div class="ekart--product--container">
    <app-search (searchTextChanged)="setSearchText($event)"></app-search>

    <product-list [searchText]="searchText" #productListComponent=></product-list>
  <product-detail *ngIf="productListComponent.selectedProduct"></product-detail>
</div>

```
The `#productListComponent` variable is storing a reference to the ProductListComponent class, in which we have the property selectedProduct

initially selectedProduct will be undefined, so the product-detail component won't be rendered, but when a product is selected, then it will be displayed. 
(But we still have dummy data for the component)


# @ViewChild 

@ViewChild decorator is used to query and get a reference of the DOM element in the component. 
It returns the first matching element

```html
<div class="ekart--search--product">
    <input class="ekart-search-product-input" #searchInput>
    <button class="btn btn-search" (click)="updateSearchText(searchInput)">Search</button>
</div>
```
previous code 
in the previous code when we click the button te searchInput is passed to the method, but instead what we want is 
as soon as the search component is loaded we want to access the input element

now we need a method to access the input element in our component class without the searchInput


steps: 
1. create a property `searchInputElement` where we will store a reference to our input Element
- for that we have to decorate the property with @ViewChild decorator : 
	- takes a selector as parameter, we can pass the template reference variable as a string 
1. Using the elementref in our code through the property
```ts
@ViewChild('searchInput')  
searchInputEl: ElementRef;

updateSearchText(){
    this.searchText = this.searchInputEl.nativeElement.value;
    this.searchTextChanged.emit(this.searchText);
}
```

as soon as the component loads, the value will be passed through the ViewChild parameter with the template reference variable "#searchInput" in the view template

- The view child decorator also takes a second optional parameter, 
	- read : Use it to read the different token from the queried elements.
	- static : determines when the query is resolved.
		- True : is when the view is initialized (before the first change detection) for the first time (default value)
		- False : is if you want it to be resolved after every change detection.
Ex :
```
@ViewChild('searchInput', {static: false})
```
since we want it to be re-initialized every time the value is changed.

- instead of the template reference variable (#searchInput) we can also use the component name (class) as the 1st parameter for @ViewChild
```
	@ViewChild(ProductListComponent)
```

ngOnInit() : to be discussed later
- it is a lifecycle method 
- this method will be called when all the properties of the component class are initialized.


## @ViewChildren decorator
- The @ViewChildren decorator is used to get a reference to the list of DOM element from the view template in the component class
- It returns all the matching element.

(Uses a new project)
we have a form in the app component 

add `#inputEl` to all input elements in the form

Using ViewChild
```html
<div class="input-container">  
  <input type="text" placeholder="First Name" #inputEl>  
  <br><br>  <input type="text" placeholder="Middle Name" #inputEl>  
  <br><br>  <input type="text" placeholder="Last Name" #inputEl>  
  <button (click)="show()">Show Full Name</button>  
</div>
```

```ts
export class AppComponent {  
  title = 'angular-view-children';  
  
  @ViewChild('inputEl')  
  inputElements: ElementRef;  
  
  show() {  
    console.log(this.inputElements.nativeElement.value);  
  }  
}
``` 
Using ViewChild we can access only the first element which matches the `#inputEl` reference variable
We want to get a reference to all the elements  in our app component class

Using @ViewChildren
- It returns a QueryList of type ElementRef
```ts
export class AppComponent {  
  title = 'angular-view-children';  
  
  @ViewChildren('inputEl')  
  inputElements: QueryList<ElementRef>;  
  
  show() {  
    this.inputElements.forEach((el) =>{  
      console.log(el.nativeElement.value);
    })  
  }  
}
```
- Now in the inputElements property we get a list of elements 

Getting the name and displaying in the browser : 
```html
<div class="input-container">  
  <input type="text" placeholder="First Name" #inputEl>  
  <br><br>  <input type="text" placeholder="Middle Name" #inputEl>  
  <br><br>  <input type="text" placeholder="Last Name" #inputEl>  
  <button (click)="show()">Show Full Name</button>  
  <h3>{{fullName}}</h3>  
</div>
```

```ts
export class AppComponent {
  title = 'angular-view-children';

  fullName: String = "";

  @ViewChildren('inputEl')
  inputElements: QueryList<ElementRef>;

  show() {
    let name = "";
    this.inputElements.forEach((el) =>{
      name += el.nativeElement.value + ' ';
    })
    this.fullName = name.trim();
  }
}
```


Note: ViewChildren is always resolved after the change detection cycle is run, not when the view is rendered 
i.e. in the 2nd parameter of @ViewChildren we cannot set the static property (boolean value)
i.e. ngOnInit() wont work with @ViewChildren properties like @ViewChild


# ng-template

- The ng-template is an Angular element which wraps an HTML snippet. 
- This HTML snippet acts and can be used like a template and can be rendered in the DOM.
```html
<ng-template>  
  <h3>This is a template</h3>  
  <p>This is an example paragraph to understand ng-template</p>  
</ng-template>
```
- when we create an element with ng-template, the element will not render itself on the DOM
- Using ng-template we are only creating a template

#### How to display the template 
1. using ngTemplateOutlet Directive
- ngTemplateOutlet is a structural directive, makes changes to the DOM
```html
<ng-template #myTemplate>  
  <h3>This is a template</h3>  
  <p>This is an example paragraph to understand ng-template</p>  
</ng-template>  
  
<div *ngTemplateOutlet="myTemplate"></div>
```

now instead of the div the ng-template element will be rendered

2. Using it as a ts expression : 
ex : 
```ts
<div class="ekart-product-detail-addtoCart">
    <button class="btn-add-to-cart" *ngIf="product.is_in_inventory; else notifyMe" >ADD TO CART</button>
    
    <ng-template #notifyMe>
      <button class="btn-add-to-cart">Notify me</button>
    </ng-template>
    
</div>
```



## ng-container
- The ng-container is a special angular element that can hold structural directives without adding new elements to the DOM.
- You cannot use multiple structural directives (*ngFor, *ngIf) together in the same tag, same with any event binding cause they all manipulate the DOM and might create some conflicts.
- Thats when you use ng-container, ng-container element is invisible to the HTML layout.
- ng-component allows us to use structural directives, without any extra elements making sure that the only DOM changes being applied are those being dictated by the directive itself.
```html
<ng-container *ngIf="toggle; else toggleOff">
<p>The toggle is ON</p>
</ng-container>
<ng-template #toggleOff>
  <p>The toggle is OFF</p>
</ng-template>
<button (click)=" onToggle()">Toggle</button>

```
```ts
export class AppComponent {
  title = 'angular-ng-container';

  toggle:boolean = true;

  onToggle() {
    this.toggle = !this.toggle;
  }
}

```
- Note since it is invisible to html, events cannot be used with it, use events in the child elements. 

### ng-content
- Specify some content within selector html tags
- By default nothing happens.
- We can pass data to the called(child) component's view template from the calling (parent) component with the help of `<ng-component>` tag

Ex : 
container.component.html
```html
<featured-brands>  
  <h3 class="title">New Arrival in Nike</h3>  
</featured-brands>
```
featured-brands.component.html
```html
<div class="ekart-featured-product-item">  
  <ng-content></ng-content>  
  <p class="description">  
    Lorem Ipsum is the best thing that has ever happened to the HTML Space.  
    This is how things used to work before Lorem Ipsum happened.  
  </p>  
  <button class="call-to-action">Learn More</button>  
</div>
```

Now instead of ng-content it would display the data passed within the featured-brands tag.
This is called CONTENT PROJECTION.
- We are projecting the content from the parent component to the child component.

Note :
- When we have multiple ng-content elements, the last ng-content element will be replaced with the content which we are projecting from the parent element.
- Other ng-content elements will be ignored.

Lets say we have multiple elements being projected and we want different ng-content elements to be replaced by each of them differently.

Example : 
```html
<featured-brands>  
    <h3 class="title">New Arrival in Upma</h3>  
    <button class="call-to-action">Show New Arrivals in Upma</button>  
  </featured-brands>  
```

```html
<div class="ekart-featured-product-item">  
  <ng-content select=".title"></ng-content>  
  <p class="description">  
    Lorem Ipsum is the best thing that has ever happened to the HTML Space.  
    This is how things used to work before Lorem Ipsum happened.  
  </p>  
  <ng-content select=".call-to-action"></ng-content>  
</div>
```

## @ContentChild()
- The @ContentChild decorator is used to access a reference of a DOM element or a component or directive from the projected content into child component class.  

We want to get a reference of a parent html element in the child component class
We can use the ContentChild decorator with Template Reference variable on the element 
Ex : 
parent.component.html
```html
<h2>Parent Component</h2>
<app-child>
	<h2>Some Heading</h2>
	<p #paragraph> This is a paragraph</p>
	<app-test></app-test>
</app-child>
```
child.component.ts
```ts
@Component({
	selector:'app-child',
	templateUrl:'./child.component.html',
	styleUrls: './child.component.css'
})
export class ChildComponent{
	@ContentChild('paragraph') paraEl: ElementRef;
}
```

- We are using the template reference variable as the selector
- It searches for the first matching element


- This cannot be done using ViewChild because we want to access the element in the Parent component within the class of child component.
- ViewChild can be used for the same component view template and class only.


### @ContentChildren
- The @ContentChildren decorator is used to access reference of all the DOM elements, components or directives from the projected content in the child component class based on a given selector. 

parent.component.html
```html
<div>
	<app-child>
		<p #para> para 1</para>
		<p #para> para 2</para>
		<p #para> para 3</para>
	</app-child>
</div>
``` 

child.component.ts
```ts
@ContentChildren('para') paraElements: QueryList<ElementRef>;

display(){
	this.testElements.forEach((el) => {console.log(el)});
}
```



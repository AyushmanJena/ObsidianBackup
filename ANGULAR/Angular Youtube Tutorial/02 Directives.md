## Directive
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

### ngClass Directive

- For example we want to make changes to the search button near search box based on if text is placed within the box.

- Specifying css styles within places which expect ts expressions : using curly braces within the double quotes

format : 
```html
<button [ngClass]="{'btn': true, 'btn-search' :true}" [disabled]="!searchText">Search</button>
```
- her btn and btn-search : css styles
- its value represents if they are to be applied or not

applying css style if text is there, and not a different style if it is disabled : 
```html
<div class="ekart--search--product">
    <input class="ekart-search-product-input" [value]="searchText" [(ngModel)]="searchText">
    <button [ngClass]="{'btn': true, 'btn-search' : searchText, 'btn-search-disabled': !searchText}" [disabled]="!searchText">Search</button>
</div>
<div class="ekart-search-result-for"><p *ngIf="searchText != ''"><strong>Search result for: </strong> {{searchText}} </p></div>
```

- We must provide a value to the css class a typescript expression that returns a boolean value, else if it does not return a boolean value, the value will be type casted into a boolean value. 
- Note :  false, '', null, undefined, 0 are the only falsy values 
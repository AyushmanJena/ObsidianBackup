## @Input and Custom Event Binding

When we want to communicate between two components and we have a relationship between the two components, 
In order to pass data between parent component to child component
We can use Custom Property Binding with @Input decorator

- We want to pass data from our parent component to its Child Component
- ex : product-list.component -> product.component

In the child component we create a property called "product" with @Input() decorator.
```ts
@Component({
  selector: 'app-product',
  imports: [CommonModule],
  templateUrl: './product.component.html',
  styleUrl: './product.component.css'
})
export class ProductComponent {
  @Input()
  product: {id: number, 
    name: string, 
    description: string,
     brand: string, 
     gender: string, 
     category: string, 
     size: number[],
     color: string[], 
     price: number,
     discountPrice?: number,
     is_in_inventory: boolean,
     items_left: number,
     imageURL: string,
     slug: string
    };
}
```

In the Parent component view template
we can use the variable product as a html attribute, wrapped around square brackets. This is custom property binding.
```html
<app-product *ngFor="let prod of products" [product]="prod"></app-product>
```
- to the product attribute we can assign any value from the parent component.

Now we can use the prod property in the child component, whose value will be obtained from the parent component : 
```html
<div class="ekart--product--item">
    <div class="ekart-wishlist-sale-container" >
        <div class="ekart--add--to--wishlist">
          <i class="fa fa-heart-o" aria-hidden="true"></i>
        </div>
        <div class="ekart-on-sale-tag" *ngIf="product.discountPrice">{{(100-(product.discountPrice / product.price * 100)).toFixed(0)}}% OFF</div>
    </div>
      <div class="ekart--product--image">
          <img [src]="product.imageURL" class="ekart--product--image">
      </div>
      <div class="ekart--add--to--price">
          {{product.price}}  
      </div>
      <div class="ekart--product--name">{{product.name}}</div>
      <div class="ekart--product--category">{{product.gender}} . {{product.category}} . {{product.brand}}</div>
    <div class="ekart--product--available-colors">{{product.color.length}} Colors . Best Seller</div>
    <div class="ekart--product--in--stock" [ngStyle]="{fontWeight : 'bold', color: product.is_in_inventory ? 'Green' : 'Red'}">
        {{product.is_in_inventory ? 'Available in Stock' : 'Not available in stock'}}
    </div>
</div>
```

In this way data can be passed from parent component to the child component.


## @Output decorator Custom Event Binding 

- We can create our custom events and bind them.
- When we want to pass data from Child Component to Parent Component, we can use Custom event Binding and @Output Decorator.

Usecase : 
- When the value of the filter changes, we want to inform about the change to the parent component.
- From filter-component to product-list-component

Steps : 
1. Create an event in the child class
filter.component.ts
```ts
@Component({
  selector: 'app-filter',
  imports: [FormsModule],
  templateUrl: './filter.component.html',
  styleUrl: './filter.component.css'
})
export class FilterComponent {
  @Input()
  all: number = 0;

  @Input()
  inStock: number = 0;
  
  @Input()
  outOfStock: number = 0;

  selectedFilterRadioButtonChanged: EventEmitter<String> = new EventEmitter<String>(); // custom event

  selectedFilterRadioButton: String = 'all'; // default radio button to be selected

  onSelectedFilterRadioButtonChanged(){ // method which raises the custom event and emits a value (step 3)
    console.log(this.selectedFilterRadioButton);
    this.selectedFilterRadioButtonChanged.emit(this.selectedFilterRadioButton);
  }
}

```
- We want to raise this event whenever the radiobutton selection happens.  A change event will happen, we want to handle it.
2. Create event binding with '()' in the child component view template. To call a method when the change happens.
filter.component.html
```html
<div clas="filter-container">
    <span>Filter: </span>

    <input type="radio" name="filter" value="all" 
    [(ngModel)]="selectedFilterRadioButton".................(1)
    (change)="onSelectedFilterRadioButtonChanged()"/> .................(2)
    <span>{{'All (' +all+ ')'}}</span>

    <input type="radio" name="filter" value="true" 
    [(ngModel)]="selectedFilterRadioButton" 
    (change)="onSelectedFilterRadioButtonChanged()"/>
    <span>{{'In Stock (' +inStock+ ')'}}</span>

    <input type="radio" name="filter" value="false" 
    [(ngModel)]="selectedFilterRadioButton" 
    (change)="onSelectedFilterRadioButtonChanged()"/>
    <span>{{'Out Of Stock (' +outOfStock+ ')'}}</span>
</div>
```
(1) :  handles two way data binding for the value selectedFilterRadioButton
(2) : handles the custom event and calls the required method.
2. The method which raises the custom event and we want to emit the data which the user has selected. 
	- If the user has selected all button emit all, if instock then emit true, etc. as per code
	- we are storing the data in selectedFilterRadioButton property
	- Since selectedFilterRadioButton is two way data binded its value will be tracked automatically.

As of now we are only tracking which radio button is selected and not making changes to the products-list cards 

1. bind the event in the parent component with a method 
product-list.component.html  : parent component
```html
<app-filter [all] = "totalProductCount" 
    [inStock] = "totalProductsInStock" 
    [outOfStock] = "totalProductsOutOfStock" 
    (selectedFilterRadioButtonChanged)="onFilterChanged()">
</app-filter>
<div class="ekart--products--container">
    <app-product *ngFor="let prod of products" [product]="prod"></app-product>
</div>
```
product-list.component.ts
```ts
export class ProductListComponent {
  products = [
    {...};
  onFilterChanged(){
    console.log(this.onFilterChanged);
  }
}
```



- the child component is raising the event but the parent component still does not know about the change. 
- Therefore we decorate the selectedFilterRadioButtonChanged property in the child component with @Output decorator
```ts
  @Output()
  selectedFilterRadioButtonChanged: EventEmitter<String> = new EventEmitter<String>();
```


Now the parent component can get to know when the event is raised, now we want to track the change and make changes accordingly.

create a property in the parent component which would track the changes made to the radio button in the child component.
```ts
selectedFilterRadioButton: string = 'all';
```

how will we track the change ?
when the event is raised, it emits some data.
we can access the data using $event variable.

in parent component
```html
<app-filter [all] = "totalProductCount" 
    [inStock] = "totalProductsInStock" 
    [outOfStock] = "totalProductsOutOfStock" 
    (selectedFilterRadioButtonChanged)="onFilterChanged($event)">
```
through the $event variable we can get which radio button is selected.

so we will accept the data in the method in parent component and make changes to the variable.
product-list.component : parent
```ts
  onFilterChanged(value: String){
    this.selectedFilterRadioButton =  value;
  }
```

Now we want to filter the products based on the value of selectedFilterChanged()
i.e. hide products  based on the selected radiobutton

using ngIf directive

NOTE : WE CANNOT HAVE TWO STRUCTURAL DIRECTIVES ON THE SAME ELEMENT e.g. ngFor and ngIf, we can solve it by having another outer div with one of the directives

product-list.component.html
```html
<div class="ekart--products--container">
    <div *ngFor="let prod of products">
        <app-product [product]="prod" *ngIf="selectedFilterRadioButton === 'all' || prod.is_in_inventory.toString() === selectedFilterRadioButton"></app-product>
    </div>
</div>
```


# Non-Related Component Communication
- How to pass data between sibling components or non-related components. 

usecase : when we type something inside the search box, we want to display the list of products based on the search text 
app-search component and product-list component are siblings (both child of container component)

- To do this we need to use a combination of custom custom event binding with @Output and custom event binding with @Input decorator
- First we pass data from search component to container component with @Output and then send the same data from container to product-list component with @Input
- For this we can also use Services (later)


In search Component (sender child component)
1. Create an event
2. create an input event in the view template text field
3. raise the event for every input event change and emit a value.
```html
<div class="ekart--search--product">
    <input class="ekart-search-product-input" [value]="searchText" [(ngModel)]="searchText" (input)="onSearchTextChanged()">
    <button [ngClass]="{'btn': true, 'btn-search' : searchText, 'btn-search-disabled': !searchText}" [disabled]="!searchText">Search</button>
</div>
<div class="ekart-search-result-for"><p *ngIf="searchText != ''"><strong>Search result for: </strong> {{searchText}} </p></div>
```

```ts
  @Output()
  searchTextChanged: EventEmitter<string> = new EventEmitter<string>();
  
  onSearchTextChanged(){
    this.searchTextChanged.emit(this.searchText);
  }
```

In the parent component : Container component 

1. Create a property to store the value sent by child component
2. use event binding to get the value from child in web view template using $event
3. create a method to set the value of the property to the returned string 

```html
<div class="ekart--product--container">
    <app-search (searchTextChanged)="setSearchText($event)"></app-search>
    <product-list></product-list>
</div>
```

```ts
@Component({
  selector: 'app-container',
  imports: [CommonModule, SearchComponent, ProductListComponent],
  templateUrl: './container.component.html',
  styleUrl: './container.component.css'
})
export class ContainerComponent {

  searchText: string = "";

  setSearchText(value: string){
    this.searchText = value;
  }
}

```

NOTE : IF multiple variables are going to store the same name in different components, you should name them the same variable across all components.

Now for the second step to send the data from the parent component to the product-list component (the second child component)

need to send data from from parent component (make changes )
```html
<div class="ekart--product--container">
    <app-search (searchTextChanged)="setSearchText($event)"></app-search> // to receive data from 1st child component 
    <product-list [searchText]="searchText"></product-list> // to send data to the 2nd child component
</div>
```

product-list.component accepting the data from the parent 
product-list.component.ts
```ts
export class ProductListComponent { 
	...
	@Input()
	searchText: string = ''; // initially empty
	...
}
```

Displaying the products based on the text received from the parent : 
product-list.component.html
```html
<app-filter [all] = "totalProductCount" 
    [inStock] = "totalProductsInStock" 
    [outOfStock] = "totalProductsOutOfStock" 
    (selectedFilterRadioButtonChanged)="onFilterChanged($event)">
</app-filter>
<div class="ekart--products--container">
    <div *ngFor="let prod of products">
        <app-product [product]="prod" *ngIf="searchText === '' || prod.name.toLowerCase().includes(searchText)"></app-product>
    </div>
</div> 

```
From New Angular Course

- Connecting Angular to a Backend and Database
- Fetching Data
- Sending Data
- Handling "Loading" and "Error" States


For a front end application
- You Don't directly talk to a database from your angular application
- Reason : Performance issues, security, etc.
- Hence you talk to a web api (your own or 3rd party)
- You connect to the backend and fetch database data through api calls
- The website visitor would not have direct access to the apis and data 
- Except for some specific routes 

You send HTTP requests from inside your angular app, 
The backend app responds with some response 

![[http01.png]]


# Directly fetching data from the component
- We would switch to service later 

### How do we send HTTP requests in Angular
using a special service called HttpClient 

in the component.ts file
```ts
private httpClient = inject(HttpClient);

or
// using constructor injection
constructor(private httpClient: HttpClient){} 
```
- Service provided by angular to manage HTTP requests and responses

Now we need to set a provider
We can add providers in the @Components decorator, however since we will need it across multiple components, it is better to add it to the `main.ts` file

add array of providers with a value of array 

main.ts
```ts
bootstrapApplication(AppComponent, {  
  providers: [provideHttpClient()]               // THIS
}).catch((err) => console.error(err));
```

or (in the` app.modules.ts`) if you are using modules
```ts
@NgModule({
	declarations: [
		AppComponent,
		PlacesComponent,
		...
	],
	imports: [BroswerModule, FormsModule],
	providers: [provideHttpClient()],
	bootstrap: [AppComponent],
})
export class AppModule { }
```


### Sending a GET request to fetch data 
- We want the data to be visible on the component as soon as the component starts, therefore we use ngOnInit

calling http method
here we use .get() and it takes the url you want to send the request

we can also specify the type of data we would get i.e. Place array (not necessary but recommended)

get returns an observable, so we need to call subscribe to trigger that request

```ts
  
@Component({  
  selector: 'app-available-places',  
  standalone: true,  
  templateUrl: './available-places.component.html',  
  styleUrl: './available-places.component.css',  
  imports: [PlacesComponent, PlacesContainerComponent],  
})  
export class AvailablePlacesComponent implements OnInit {  
  places = signal<Place[] | undefined>(undefined);  
  
  private httpClient = inject(HttpClient);  
  private destroyRef = inject(DestroyRef);  
  
  ngOnInit() {  
    const subscription = this.httpClient.get<{places: Place[]}>('http://localhost:3000/places').subscribe({  
      next: (resData) => {  // next -> handle next emitted value
        console.log(resData.places);  
      }  
    });  
  
    this.destroyRef.onDestroy(() => {  
      subscription.unsubscribe();  
    });  
  }  
}
```

- the httpClient typically produces only one value

- Also destroy the subscription we created for better memory management.

view template 
```html
<app-places-container title="Available Places">
  @if (places()) {
  <app-places [places]="places()!" />
  } @else if (places()?.length === 0) {
  <p class="fallback-text">Unfortunately, no places could be found.</p>
  }
</app-places-container>
```

#### Configuring the HTTP request 
We can configure the HTTP request by sending a second parameter i.e. an object to the get method.
Such that the function is triggered with different data than the response data

the response has a body
however it can be null 
so we need to check if it is null or not

```ts
ngOnInit() {  
  const subscription = this.httpClient.get<{places: Place[]}>('http://localhost:3000/places',  
    {  
      observe: 'response',  
    }  
  ).subscribe({  
    next: (response) => {  
	  console.log(response);
      console.log(response.body?.places);  
    }  
  });  
  
  this.destroyRef.onDestroy(() => {  
    subscription.unsubscribe();  
  });  
}
```

- We can replace the response with event, referring to actions performed during different time points during the request response life cycle.


#### Displaying the data in the view template

- we are using signals here i.e. places signal to share data between the component class and the view template.

```ts
ngOnInit() {  
  const subscription = this.httpClient.get<{places: Place[]}>('http://localhost:3000/places')  
    .pipe(  
      map((resData) => resData.places)  
    )  
    .subscribe({  
    next: (places) => {  
      this.places.set(places);                                // HERE
    }  
  });
```

### Showing a loading Fallback

- Adding a visual indication that the places is loading

1. create a variable isFetching to track if the process is going on
2. Set the value to false by default
3. set value to trye, when you start onInit method
4. then after the http request is complete set it to true within a parameter to the subscribe method called complete

```ts
export class AvailablePlacesComponent implements OnInit {  
  places = signal<Place[] | undefined>(undefined);  
  isFetching = signal(false);                               // THIS
  private httpClient = inject(HttpClient);  
  private destroyRef = inject(DestroyRef);  
  
  ngOnInit() {  
    this.isFetching = signal(true);  
    const subscription = this.httpClient.get<{places: Place[]}>('http://localhost:3000/places')  
      .pipe(  
        map((resData) => resData.places)  
      )  
      .subscribe({  
        next: (places) => {  
          this.places.set(places);  
        },  
        complete: () => {                                    // THIS
          this.isFetching = signal(false)  
        }  
    });  
  
    this.destroyRef.onDestroy(() => {  
      subscription.unsubscribe();  
    });  
  }  
}
```

changes to template
```html
<app-places-container title="Available Places">  
  @if(isFetching()){  
    <p class="fallback-text">Fetching available places....</p>   // THIS
  }  
  
  @if (places()) {  
  <app-places [places]="places()!" />  
  } @else if (places()?.length === 0) {  
  <p class="fallback-text">Unfortunately, no places could be found.</p>  
  }  
</app-places-container>
```
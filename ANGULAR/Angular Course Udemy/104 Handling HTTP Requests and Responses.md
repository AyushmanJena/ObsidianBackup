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

### Handling HTTP Errors

add an extra signal to the component fetching the data
`error = signal('');`

then add another error function to our observer to be called if the result is an error: 

then modify our view template with a check to show if error occured

available-places.component.html
```ts
export class AvailablePlacesComponent implements OnInit {  
  places = signal<Place[] | undefined>(undefined);  
  isFetching = signal(false);  
  error = signal('');                                          // THIS
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
        error: (error) =>{                                     // THIS
          console.log(error);   
          this.error.set("Something went wrong");  
        },  
        complete: () => {  
          this.isFetching = signal(false)  
        }  
    });  
  
    this.destroyRef.onDestroy(() => {  
      subscription.unsubscribe();  
    });  
  }  
}
```

view template : 
```html
@if(error()){  
  <p class="fallback-text">{{error()}}</p>  
}
```

or we can add an operator to keep the observer function simpler using catchError()

```ts
.pipe(  
    map((resData) => resData.places),
    catchError((error) => return throwError(() => new Error('Error Message')))  
)  
```


### Sending Data to the Backend

contains a request body


add a event listener to view template 
```html
<app-places [places]="places()!" (selectPlace)="onSelectPlace($event)" />
```

write the method in ts with the http call

parameters : (url, request body )
```ts
onSelectPlace(selectedPlace: Place){  
  this.httpClient.put('http://localhost:3000/user-places', {  
    placeId: selectedPlace.id  
  }).subscribe();  
}
```
- Angular will automatically convert the object into json 
- You need to subscribe to the put request to trigger it.


### Outsourcing HTTP Request Logic into a Service

- Using a service to make the component class smaller and simpler 

places.service.ts
```ts
@Injectable({  
  providedIn: 'root',  
})  
export class PlacesService {  
  private httpClient = inject(HttpClient);  
  private userPlaces = signal<Place[]>([]);  
  
  loadedUserPlaces = this.userPlaces.asReadonly();  
  
  loadAvailablePlaces() {  
    return this.fetchPlaces('http://localhost:3000/places', "Something went wrong");  
  }  
  
  loadUserPlaces() {  
    return this.fetchPlaces('http://localhost:3000/user-places', "Something went wrong");  
  }  
  
  addPlaceToUserPlaces(placeId: String) {  
    return this.httpClient.put('http://localhost:3000/user-places', {  
      placeId,  
    });  
  }  
  
  removeUserPlace(place: Place) {}  
  
  private fetchPlaces(url:string, errorMessage: string) {  
    return this.httpClient.get<{places: Place[]}>(url)  
      .pipe(  
        map((resData) => resData.places),  
        catchError((error) => {  
          console.log(error);  
          return throwError(() => new Error(errorMessage));  
        })  
      )  
  
  }  
}
```

then call the methods in other components like : 
available-places.component.ts
```ts
export class AvailablePlacesComponent implements OnInit {
  places = signal<Place[] | undefined>(undefined);
  isFetching = signal(false);
  error = signal('');
  private placesService = inject(PlacesService);
  private destroyRef = inject(DestroyRef);

  ngOnInit() {
    this.isFetching = signal(true);
    const subscription = this.placesService.loadAvailablePlaces()
      .subscribe({
        next: (places) => {
          this.places.set(places);
        },
        error: (error) =>{
          console.log(error);
          this.error.set("Something went wrong");
        },
        complete: () => {
          this.isFetching = signal(false)
        }
    });

    this.destroyRef.onDestroy(() => {
      subscription.unsubscribe();
    });
  }

  onSelectPlace(selectedPlace: Place){
    const subscription = this.placesService.addPlaceToUserPlaces(selectedPlace.id).subscribe({
      next: (resData) => console.log(resData),
    });

    this.destroyRef.onDestroy(() => {
      subscription.unsubscribe();
    });
  }
}

```

### Managing HTTP Loaded data via a Service

using tap operator : tap operator is used to execute some code as you subscribe, but without subscribing (ayein)

places.service.ts
```ts
loadUserPlaces() {  
  return this.fetchPlaces('http://localhost:3000/user-places', "Something went wrong")  
    .pipe(tap({  
      next: (userPlaces) => this.userPlaces.set(userPlaces)  
    }));  
}
```

now we can remove the next method from the ngOnInit in user-places.component as this will be used instead.



### Implementing Optimistic Updating  : 

Optimistic updating is a technique in application development where the user interface is updated immediately to reflect a change, before confirming that the change has been successfully applied on the server

- Updating the method to display the place when clicked only, and update the backend separately. 
- Adding a update method to our service which is called, this is updated automatically since we are using signals 

```ts
private userPlaces = signal<Place[]>([]);

addPlaceToUserPlaces(place: Place) {
    this.userPlaces.update(prevPlaces => [...prevPlaces, place]);
		// THIS above
    return this.httpClient.put('http://localhost:3000/user-places', {
      placeId: place.id,
    });
  }
```


HOWEVER this code can cause problems, like 
1. it would be displayed in user places but if something goes wrong in backend, when reloaded it is not displayed in user places
2. displaying the same place twice if something goes wrong.

to fix that we want to do 2 things : display an error message and roll-back the optimistic update to previous state

places.service.ts
```ts
addPlaceToUserPlaces(place: Place) {  
  const prevPlaces = this.userPlaces();  
  
  if(!prevPlaces.some((p) => p.id === place.id)) { // check duplicates  
    this.userPlaces.set([...prevPlaces, place]); // optimistic updating  
  }  
  
  return this.httpClient.put('http://localhost:3000/user-places', {  
    placeId: place.id,  
  }).pipe(  
    catchError(error => {  
      this.userPlaces.set(prevPlaces);  
      return throwError(() => new Error('SFailed to store selected place'))  
    })  
  );  
}
```


### Implementing App wide Error Management


### Deleting HTTP call

places.service.ts
```ts
removeUserPlace(place: Place) {  
  const prevPlaces = this.userPlaces();  
  
  if(prevPlaces.some(p => p.id === place.id)){  
    this.userPlaces.set(prevPlaces.filter(p => p.id !== place.id));  
  }  
  
  return this.httpClient.delete('http://localhost:3000/user-places/'+ place.id)  
    .pipe(  
      catchError(error => {  
        this.userPlaces.set(prevPlaces);  
        this.errorService.showError('Failed to remove selected place');  
        return throwError(() => new Error('SFailed to remove selected place'))  
      })  
    );  
}
```

```html
<app-places [places]="places()!" (selectPlace)="onRemovePlace($event)"/>
```

user-places.component.ts
```ts
onRemovePlace(place: Place){  
  const subscription = this.placesService.removeUserPlace(place).subscribe();  
  
  this.destroyRef.onDestroy(() => {  
    subscription.unsubscribe();  
  });  
}
```


### HTTP Interceptors
Interceptors are special functions that are called when a request is about to be sent or a request arrives.

go to the place where you provide the HTTP Client and add 
main.ts
```ts
function loggingInterceptor(request: HttpRequest<unknown>, next: HttpHandlerFn){  
  console.log('[Outgoing Request]');  
  console.log(request);  
  return next(request);  
}  
  
bootstrapApplication(AppComponent, {  
  providers: [provideHttpClient(  
    withInterceptors([loggingInterceptor])  
  )]  
}).catch((err) => console.error(err));
```

add with Interceptors method which takes an array of all the interceptor functions we want to add to our angular app

the interceptor function accepts two arguments
1. interceptor request object
2. a next function 

To allow intercepted requests to eventually continue we pass the next method as return value with the intercepted request as a parameter

#### Class Based Interceptors 
Besides defining HTTP interceptors as functions (which is the modern, recommended way of doing it), you can also define HTTP interceptors via classes.

For example, the `loggingInterceptor` from the previous lecture could be defined like this (when using this class-based approach):

```ts
 import {
	 HttpEvent,
	 HttpHandler,
	 HttpInterceptor,
	 HttpRequest,
 } from '@angular/common/http';
 import { Observable } from 'rxjs';

 @Injectable()
	 class LoggingInterceptor implements HttpInterceptor {
		 intercept(req: HttpRequest<unknown>, handler: HttpHandler): Observable<HttpEvent<any>> {
		 console.log('Request URL: ' + req.url);
		 return handler.handle(req);
	 }
 }
```

You now must use `withInterceptorsFromDi()` and set up a custom provider, like this:

```ts
providers: [
  provideHttpClient(
    withInterceptorsFromDi()
  ),
  { provide: HTTP_INTERCEPTORS, useClass: LoggingInterceptor, multi: true }
]
```

#### HTTP Response Interceptors
```ts
function loggingInterceptor(request: HttpRequest<unknown>, next: HttpHandlerFn){  
  console.log('[Outgoing Request]');  
  console.log(request);  
  return next(request).pipe(  
    tap({  
      next: event => {  
        if(event.type === HttpEventType.Response){  
          console.log("Incoming Response");  
          console.log(event.status);  
          console.log(event.body);  
        }  
      }  
    })  
  );  
}
```
### RxJS 
RxJS  (Reactive Extensions for Javascript) is a library for reactive programming using observables, to make it easier to compose asynchronous or callback based code

### Observables
- An Observable is an object that produces and controls a Stream of Data
- RxJS Observables emit values over time - you can set up subscriptions to handle them.
- Set up Subscriptions to listen for values (and use them)

- RxJS provides multiple functions e.g. `interval()` to create observables, (can find all in RxJS documentation)
### Creating and Using an Observable

you need a subscribe method to use an observable, because without any subscriber the data will not be emitted.
- The subscribe() can take upto 3 methods : 
1. next : executed with every listen
2. complete : executed after the data emitting is complete
3. error : if error occurred in the observable call

```ts
@Component({
	...
})
export class AppComponent implements OnInit{
	private destroyRef = inject(DestroyRef);

	ngOnInit() : void {
		const subscription = interval(1000).subscribe({
			next: (val) => console.log(val)
		});

		this.destroyRef.onDestroy(() => {
			subscription.unsubscribe();
		})
	}
}
```
- val is the value that is emitted by the observable
- It is a good habit to store the subscription in a variable and to clean it up when the component is removed from the DOM

### Working with RxJS Operators
- You can use operators to your called observables before the pipeline by adding the pipe method before you subscribe
- Then pipe allows you to add RxJS operators 
- You can find the RxJS operators available in the RxJS documentation as well

```ts
export class AppComponent implements OnInit{
	private destroyRef = inject(DestroyRef);

	ngOnInit() : void {
		const subscription = interval(1000).pipe(
			map((val) => val * 2)
		).subscribe({
			next: (val) => console.log(val)
		});

		this.destroyRef.onDestroy(() => {
			subscription.unsubscribe();
		})
	}
}
```

map function is an operator that converts the value emitted by the observable, 
here the map takes another function as an argument and that function will be executed on every value that is emitted by the observable.


### Working with Signals and Observables
- We might need to convert Signals to Observables and Vice versa in our application


```ts
export class AppComponent implements OnInit{
	clickCount = signal(0);
	private destroyRef = inject(DestroyRef);

	constructor(){
		effect(() => {
			console.log("Clicked Button ${this.clickCount()} times");
		});
	}

	ngOnInit() : void {
	}

	onClick() {
		this.clickCount.update(prevCount => prevCount + 1);
	}
}
```

effect()  -> function is called every time any signal of the component updates.


Observables : Great for managing events and streamed data
Signals : Great for managing application state


### Converting Signals to Observables
- using toObservable() 
```ts
export class AppComponent implements OnInit{
	clickCount = signal(0);
	clickCount$ = toObservable(this.clickCount);
	private destroyRef = inject(DestroyRef);

	constructor(){
		toObservable(this.clickCount)
	}

	ngOnInit() : void {
		const subscription = this.clickCount$.subscribe({
			next: (val) => console.log(${this.clickCount()}" times clicked"); 
		});

		this.destroyRef.onDestroy(() => {
			subscription.unsubscribe();
		})
	}

	onClick() {
		this.clickCount.update(prevCount => prevCount + 1);
	}
}
```


### Converting Observables to Signals
- using toSignal()
```ts
export class AppComponent implements OnInit{
	clickCount = signal(0);
	clickCount$ = toObservable(this.clickCount);
	private destroyRef = inject(DestroyRef);
	
	
	interval$ = interval(1000);                      // THIS
	intervalSignal = toSignal(this.interval$)

	constructor(){
		toObservable(this.clickCount)
	}

	ngOnInit() : void {
		const subscription = this.clickCount$.subscribe({
			next: (val) => console.log(${this.clickCount()}" times clicked"); 
		});

		this.destroyRef.onDestroy(() => {
			subscription.unsubscribe();
		})
	}

	onClick() {
		this.clickCount.update(prevCount => prevCount + 1);
	}
}
```

- Observers do not have some initial value, however signals have an initial value
- Initially for an observable stored in a signal, the data stored is undefined.
- However you can set the initial value manually as well
- `toSignal(this.interval$, {initialValue: 0})`

- You can also destroy the observable when the component is removed from memory by another similar property : 
- `toSignal(this.interval$, {initialValue: 0, manualCleanup: true})`

### Creating Custom Observable from Scratch
- using the Observable class

```ts
export class AppComponent implements OnInit{
	customInterval$ = new Observable((subscriber) => {
		setInterval(() => {
			subscriber.next();
		}, 2000);
	});
	...
	ngOnInit(){
		this.customInterval$.subscribe({
			next: (val) => console.log(val)
		});
		...
	}
}
```
- The constructor class of Observable takes a function as input that itself accepts subscriber as an argument
- The subscriber allows us to invoke methods like next, complete and error


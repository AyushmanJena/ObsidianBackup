To create a new page 
`ionic generate page location `
ex : 
`ionic generate page pages/loader`

in short : 
`ionic g p pages/loader`

`ionic generate page pages/loader --no-standalone` for non standalone components

Might need to add standalone: false to @Component if such issues arise

You can use pre built ionic components from :
https://ionicframework.com/docs/components

### Compnents
You can use components as you use in Angular (exactly similar)

### Testing
`ng test`
runs all spec.ts tests that are added 

### Routing
Same as angular
using Router
```java
// inject dependency
constructor(private router: Router){}

// use router to navigate to the endpoint
navigate(){
	this.router.navigate(['/servers']);  
}
// call navigate() on call or as required
```


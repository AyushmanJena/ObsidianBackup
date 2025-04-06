# Project Directory Structure : 
### root
- All files found on the root levels are essentially configuration files.
1. tsconfig.app.json, tsconfig.json, tsconfig.spec.json : 
	- mostly default code for these should work.
2. package.json :
	- packages whichi are required by the project
3. Angular.json :
	- config file for json

### src : 
styles.css : stylesheet file
index.html : main html file
faviocn.ico : icon
main.ts : the first code file to be executed when the app loads up

assets folder: 
- contains assets like images and other files required

app folder: 
- where you will put all your angular components. 
- this is the component you spend most time working with.


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
- @Component () -> is a decorator and stores the metadata for the component
- The markup of the component is stored in the templateUrl and style in styleUrl
- this AppComponent is used in the code like this : 
`main.ts`
```ts
import {bootstrapApplication} from '@angular/platform-browser';
import {AppComponent} from './app/app.component';
bootstrapApplication(AppComponent).catch((err) => console.error(err));
```
note : for .ts files we do not specify the extension as above




# Working with Modules
- Modules are used for grouping components together and structuring our application. Alternative to standalone components.
- Today standalone components are the recommended way of building components.
- But you can also still use Module based components with Angular modules.


Angular Modules make components (and more) available to each other 

App Module
	AppComponent
		- HeaderComponent
		- UserComponent
		- TasksComponent
			- TaskComponent

Where TasksComponent and TaskComponent are another module : TasksModule

- You dont need to specify on a per component level (in component template imports)
- The module combines all the components that need to work together

> [!note]
> With the release of **Angular 19, standalone components became the default** for new Angular components.
 That means that if you DON'T set `standalone: false` (e.g., if you don't set `standalone` at all) inside of `@Component()`, you'll get a standalone component

# To change a standalone app to support modules

app.module.ts is created
- a module is just a class that gets exported
class (AppModule) is decorated with @NgModule decorator 
##### NgModule
This decorator takes a configuration object where you will configure this module.
- a declarations array where you will declare and register all the components (directives as well)
- Add the classes you want in the module to the declarations
- bootstrap takes where it starts
app.module.ts
```ts
import {NgModule} from "@angular/core";  
import {AppComponent} from "./app.component";  
  
@NgModule({  
  declarations: [AppComponent],  
  bootstrap: [AppComponent],  
  imports : [BrowserModule, HeaderComponent, UserComponent, TasksComponent]  
})
export class AppModule {}
```
- declarations for non standalone components
- imports for standalone components
- BrowserModule includes some directives for non standalone apps
note : standalone components cannot have modules so remove imports 

change main.ts as well
```ts
import {platformBrowser} from "@angular/platform-browser";  
import {AppModule} from "./app/app.module";  
  
platformBrowser().bootstrapModule(AppModule);
```

Note : You can not only add modules to standalone components, but also standalone components to modules.

Importing other components and modules to app.module.ts
final : 
```ts
@NgModule({  
  declarations: [AppComponent, HeaderComponent, TasksComponent,  
    TaskComponent, NewTaskComponent, UserComponent, CardComponent],  
  bootstrap: [AppComponent],  
  imports : [BrowserModule, FormsModule],  
})  
export class AppModule {}
```

- Now all the imports and components are stored in one place, and easier to manage. 
- However hard to find which component uses which other modules and components, hence not used anymore.

### Creating and Using Shared Modules
Create a new module
shared.module.ts : just the name

- The structure of this module class will be similar to the app module
- However there will not be a bootstrap configuration.

- exports : will define what will be made available to any other module that imports this module

```ts
import {NgModule} from "@angular/core";  
import {CardComponent} from "./card/card.component";  
  
@NgModule({  
  declarations: [CardComponent],  
  exports: [CardComponent]  
})  
export class SharedModule { }
```
- also we can remove CardComponent from the app.module.ts


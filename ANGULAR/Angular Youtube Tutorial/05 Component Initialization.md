### Component Initialization
- How components and directives are created when a selector is encountered.
- What happens after that ?

When the selector `<app-demo></app-demo>` is encountered, it calls its class and instantiates the class i.e. calls its constructor (no parameter constructor by default)
first the main app component is called 
then the child components are called in order of selector if forms a tree of components.

- When a constructor is called, by that time, none of its input properties are updated and available for use.
- When a constructor is called, by that time, the child components of that component are not yet constructed.
- Projected contents are also not available by the time the constructor of a component is called. 
- Once the component is removed from the DOM, we can say the component is destroyed.
- Angular lets us know when these events happen using Angular lifecycle hooks.

The same points holds true for directives as well.

Angular life cycle hooks : Angular life cycle hooks are the methods that angular invokes on a directive or a component, as it creates, changes and destroys them.
These Life cycle hooks are
1. ngOnChanges
2. ngOnInit
3. ngDoCheck
4. ngAfterContentInit
5. ngAfterContentChecked
6. ngAfterViewInit
7. ngAfterViewChecked
8. ngDestroy 

COMPLETED TILL 38 START FROM 39
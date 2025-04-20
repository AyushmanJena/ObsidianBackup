- Includes notes from the youtube tutorial 
- and more are here

### Adding assets
- Add images to the code 
```html
<img src="assets/image.png" alt="this"/>
```
- Make sure angular.json assets array looks like this : 
```json
"assets":[
	"src/assets",
	"src/favicon.ico"
]
```
- Now you can access all files in the assets throughout the project


- Note : You cannot access private members of a component.ts file in its view template.

### Property binding
- Using getters to get values from the component.ts file 
- Instead of using ts expressions within property binding, we can use getter methods to handle ts login within the class file and just access the property with computed value
- ex : selectedUser.avatar : user.png but we want to access the image like "/assets/users/user.png"
```
get imagePaht(){
	return '/assets/users/' + this.selectedUser.avatar;
}

// then use the value in our view template

<img [src]= "imagePath()"/>
```



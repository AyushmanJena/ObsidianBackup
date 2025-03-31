
> [!ERROR]
> INCOMPLETE TUTORIAL 
## Creating a react app : 
1. Create React App
2. Vite : 
	1. `npm create vite@latest`
	2. then follow the instructions to create a react project in the current directory
- Vite : Vite is a javascript build tool that helps you to develop and build web applications faster.

## Files in the React project : 
- vite.config.js :  vite plugins, settings, etc. Not required for basic projects 
- package.json : Contains the metadata of our project
	it contains scripts : 
	"dev" script -> Starts the development server
	"build " -> creates a production build.
	You can run the scripts through the terminal : 
	`npm run  dev`
	But firstly you need to install the necessary dependencies using 
	 `npm install or npm i`
	note: IDE might do this for you
- index.html : the main html file within which the ap is loaded
- eslint.config.js : Config file used to define the settings for eslint (linting tool to fix problems, errors, styles, etc.)

Folders : 
- node_modules : all the installed dependencies of our project

- public : static items like images, icons, other files, etc.
	files in this folder can be accessed directly in the code ex.- /icon.png

- src : holds the main react components and javascript logic
	- app.jsx : main ui of the app is defined
	- main.jsx : entry point of the react app
	- assets folder : contains media assets 
	- app.css : styling file for app.jsx

 
# Components 
Two ways to define Components :
1. Class Components (old and not recommended)
```js
class ClassComponent extends React.Component {
	render(){
		return <h2>Class Component </h2>	
	}
}
```
2. Using Javascript functions : 
```js
const App = () => {  
    return (  
        <h2>Functional Array Component</h2>  
    )}  
  
export default App
```
or
```js
function App(){  
    return <h2>Hello World</h2>  
}
```

Using Components : 
- Create a Card Component and use it in through another Component : 
```js
const App = () => {
    return (
        <div>
            <h2>Functional Array Component</h2>
            <Card />
            <Card />
            <Card />
        </div>

    )
}

const Card = () => {
    return (
        <div>
            <h2>Card Component</h2>
        </div>
    )
}

export default App
```
- This way we can create one Component and use it multiple times 

For static values write them in html as usual, but for dynamic values write them within '{ }'
### Props
- Writing components is not enough 
- Sometimes we need to pass data from one component to another
-  Helpful to display something specific for each component and not repeating the same data
- This is done through PROPS
- PROPS -> Properties
- A way to pass data from one component to another

-  Typically done from a parent component to a child component
- Similar to arguments passed to functions

![[01.png]]
Example : 

```js
const App = () => {
    return (
        <div>
            <h2>Functional Array Component</h2>
            <Card title="Star Wars"/> // values are passed here
            <Card title="Avatar"/>
            <Card title="The Lion king"/> 
        </div>

    )
}

const Card = ({title}) => { // parameters are passed here 
    return (
        <div>
            <h2>{title}</h2> // using values 
        </div>
    )
}

export default App
```

- The prop can be any value like a number, boolean or a complex expression like object
```js
Card title="Star Wars" rating={5} isCool={true} actors={[{name:'Actors'}]}/>
```

### Styles
Ways to style : 
1. Inline styles
2. CSS
3. Tailwind CSS
4. Bootstrap
5. Material UI, ... 
6. The list never ends 

Index.css is imported in the main.jsx and the styles are applied

You can add id and class names to tags and implement css using index.css

Note : Inline styles have preference over all other ways of styling.

Inline styling : 
```js
const Card = ({title}) => {  
    return (  
        <div className = "card" style={{  
            border: '1px solid #4b5362',  
            padding: '20px',  
            margin: '10px',  
            backgroundColor: '#31363f',  
            borderRadius: '10px',  
            minHeight: '100px',  
        }}>  
            <h2>{title}</h2>  
        </div>  
    )}
```

Now tailwind css is the way to go. (most popular and recommended)

# State 
- State is like a react component's brain
- It holds information about components that can change overtime.

![[02.png]]

If  you use js variable instead of states and pass them as props to components, react won't know when the value has changed and would not render changes accordingly.
Because React rendering process relies on state and props to decide when and how to re-render components.

```js
const [var, setter] = useState() // syntax
const [hasLiked, setHasLiked] = useState(false) // example
```
var -> variable whose value is to be modified
setter -> setter function which would update the state
within parameters pass the default value (or initial state) of the state

note: in react everything that starts with "use" is a hook

```js
const Card = ({title}) => {  
    const [hasLiked, setHasLiked] = useState(false)  
    return (  
        <div className = "card" >  
            <h2>{title}</h2>  
            <button onClick={() => {setHasLiked(true)}}>  
                {  
                    hasLiked ? 'Liked' : 'Like'  
                }  
            </button>  
        </div>  
    )}
```
default value is false, so when the page is refreshed, the value returns back to 'Like'
`{setHasLiked(true)}` -> to invert the value of the state


## Hooks
- Hooks are special functions in react that let you tap into react features like state management.
- useState : for managing states
- useEffect : for handling side effects like data fetching
- useContext : for sharing data across components
- useCallback : for optimizing callback functions 
- more like useReducer, useContext, useTransition, useMemo, etc.

### useEffect hook 
- Lets you do stuff outside of just displaying stuff on the screen
- like fetching data from the server, doing cleanup after a component is removed from the screen.
- Generally used for logging purpose
- Ex : we want to log a message everytime a user likes a movie, for it we can use useEffect hook
```js
useEffect(() => {  
    console.log(`${title} has been ${hasLiked}`);  
});
```

> [!note]
> The effect runs twice when the component mounts.
When strict mode is on and in development, React runs setup and cleanup one extra time before the actual setup. (Not a problem)
And in production will run as intended


To go into non-strict mode, go to `main.jsx` and remove the `<StrictMode>` tag from the code

Adding another state to count the number of clicks : 
```jsx
const Card = ({title}) => {  
    const [set, setCount] = useState(0);  
    const [hasLiked, setHasLiked] = useState(false);  
  
    useEffect(() => {  
        console.log(`${title} has been liked ${hasLiked}`);  
    });  
  
    return (  
        <div className = "card" onClick={() => setCount((prevCount) => prevCount + 1)}>  // increasing the count of clicks
            <h2>{title} </br> {count} </h2>  
            <button onClick={() => {setHasLiked(!hasLiked)}}>  
                {hasLiked ? '❤️' : '🤍'}  
            </button>  
        </div>  
    )}
```

However the useEffect is called every time we click the tag, even if the state has not changed. and that is not what we want. 
To do that we use a dependency array 
passes as the second parameter to useEffect() 
for the value passed as the dependency array, react will check if its value has changed. And only if it has changed, useEffect effect method will be called. 

##### Creating an empty useEffect for a component : 
```jsx
useEffect(() => {  
    console.log('CARD RENDERED');  
}, [])
```
This useEffect runs only once on the mounting of that component. 

##### Conditional Rendering : 
we want to render count only if it is not zero
```jsx
<h2>{title} </br> {count ? count : null} </h2>
```

### Quickly Creating react components : 
Install Modern React Snippets plugin
shortcut : rafce -> react arrow function component elements


# MOVIE PROJECT 

### Installing Tailwindcss in the project
1. `npm install tailwindcss @tailwindcss/vite`
2. Configure the vite plugin  vite.config.js
```js
import tailwindcss from "@tailwindcss/vite";  
  
// https://vite.dev/config/  
export default defineConfig({  
  plugins: [react(), tailwindcss()],  
})
```
3. Import tailwindcss into our main index.css file
`@import "tailwindcss";`

Now you can use tailwind css in your project :)


Notes : 
- do not mutate props 
- Never mutate state, you only modify the state using the state setter function



Since api is not working anymore here are notes only

- In your react app you do not put your api key as a variable in the jsx files
- You put the key in environment variables. 

1. create a new file in the root of your folder : my-first-react-app 
2. .env.local
```.env.local
VITE_TMDB_API_KEY=yourapikey 
```
re run your app

- Use useEffect() react hook to fetch your movies. 
```jsx

const API_BASE_URL = 'https://api.themoviedb.org/3';

const API_KEY = import.meta.env.VITE_TMDB_API_KEY; // import from the env file

const API_OPTIONS = {
	method : 'GET', 
	headers: {
		accept: 'application/json',
		Authorization: 'Bearer ${API_KEY}'
	}
}

const App = () => {
	const[searchTerm, setSearchTerm] = useState('');

	// we can display any error message in the browser itself using useState hook
	const [errorMessage, setErrorMessage] = useState('');
	const [movieList, setMovieList] = useState([]);
	const [isLoading, setIsLoading] = useState(false);

	const fetchMovies = async() => {
		setIsLoading(true);
		setErrorMessage(';);
		try { // calling the api
			const endpoint = `{API_BASE_URL}/discover/movie?sort_by=popularity.desc`; // this is the complete endpoint
			const response = await fetch(endpoint, API_OPTIONS); // making api request using fetch()

		if(!response.ok){ // throw error if we do not get ok response
			throw new Error("Failed to fetch movies");
		}

		const data = await response.json(); // convert response into json

		console.log(data);

		// now we want to use the data we received and show it to the user
		if(data.Response === 'False'){
			setErrorMessage(data.Error || "Failed to fetch movies");
			setMovieList([]);
			return;
		}
		setMovieList(data.results || []);
			
		}catch(error) // if anything fails in the api call we catch it here
			console.error(`Error Fetching movies ${error}`);
			setErrorMessage('Error fetching movies try again later'); // setting the state of errorMessage  which would be displayed in the browser
		}finally {
			setIsLoading(false);
		}
	}

	useEffect(() => {
		fetchMovies();
	}, []);
}
```

accessing the error message in the website : 
```html
{errorMessage && <p clasName="text-red-500">{errorMesage}</p>}
```

showing the movies in the browser : 
```html
<section className = "all-movies">
	
</section>
```



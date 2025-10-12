 React makes it easy to manage and build complex frontend
 React is a Library
 React builds single page applications

Topics to learn
- Core of react (state or ui manipulation, JSX)
- Component Reusability
- Reusing of components (props)
- How to propagate change (hooks)

Additional Addons to React : 
- Router (React don't have routes)
- State management (React don't have state management) : Redux, Redux Toolkit, Context API, etc.
- Class Based Component : Legacy code :(
- BAAS Apps : Backend As Service e.g. Appwrite, superbase, etc.


# Create a New React Project
```
npx create-react-app 01basicreact
```
npx  : node package executor
create-react-app : utility
bulky utility so slow to setup and not recommended

Vite or Parcel are recommended
Vite is a bundler

Creating project using vite
```
npm create vite@latest

// then set project name
// select your framework : react
// select your language : javascript

npm install

npm run dev
```


# Folder Structure 

node_modules : Contains all dependencies installed via npm/yarn.

public : Static files that dont get processed. Ex: images, favicon, etc.

src :
App.jsx : Root react component (Entry point of your UI)
main.jsx : Application entry file, where ReactDOM renders `<App/>` into the root div in index.html
index.css : Global CSS Styles
assets : For images, icons and other static resources you want to import into components

other files :
gitignore → Lists files/folders Git should ignore (like node_modules).
eslint.config.js → Configuration for ESLint (code quality/linting rules).
index.html → The single HTML file. React injects the app into its `<div id="root">`
package.json → Project metadata, dependencies, and scripts (npm run dev, npm run build, etc.).
package-lock.json → Auto-generated lock file ensuring exact dependency versions.
README.md → Documentation about your project.
vite.config.js → Vite configuration file (plugins, server config, build options).


# Understanding How it works 
jsx : Javascript XML

`<App />` -> a function that returns html 

main.jsx
```jsx
createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)

```
- this code looks at index.html for the root element
- createRoot() : creates a react root to manage and render components
- render() : renders the react components inside the root div
- App : the root component

FOR VITE : 
- component file extension must be jsx
- and start with upper case 


You can export only one element. 
So you must enclose all the elements in your return in a parent tag

```jsx
function App() {
	return (
		<>
		<Chai/>
		<h1>Hello</h1>
		</>
	)
}

export default App;
```


In react, code is not segregated based on what type of code they use e.g. (html, css, js) are not different files. 
Code is segregated based on what they do e.g. each component is a different file and does a different task.
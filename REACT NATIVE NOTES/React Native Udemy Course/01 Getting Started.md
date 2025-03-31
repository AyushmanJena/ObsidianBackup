# React Native 
### React.js
- React.js is independent from React Native 
- React.js is a js library for building user interfaces.
- Typically used for web development
- Note : react-dom add web support for react js
- React itself is platform agnostic 
### React Native
- Collection of special React components
- Components are compiled to native UI elements
- Native platform APIs exposed to Javascript e.g. camera, etc.
- React native connects react to the specific platfoms

# A look under the hood
React + React Native App: 
```jsx
const App = props => {
	return(
		<View>
			<Text>Hello There!</Text>
		</View>	
	);
};
```
This component code will be compiled to real native app
`<div> -> <View>`
`<input> -> <TextInput>`
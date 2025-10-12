# Custom Hooks in React with Currency Project


For custom hooks create a js file that returns a value
e.g. : create a folder hooks in src, then a file useCurrencyInfo.js in it

we  want to make an api call in the hook : 

useCurrencyInfo.js : Hook

InputBox.jsx : Component


### Looping 
```jsx
{currencyOptions.map((currency)=> (
    <option key={currency} value={currency}>
	    {currency}
    </option>
)) }
```
When looping over jsx use `key` to make it optimized and improve the performance 


### Better Method to export components instead of exporting one by one : 
Create a file index.js in components
```js
import InputBox  from "./InputBox";
import SearchBox  from "./SearchBox";

export {InputBox, SearchBox}
```


##### useId hook
useId is a react hook for generating unique IDs that can be passed to accessibility attributes.

Each html element has accessibility attributes.

Create an hook with useId and bind it with the input field 


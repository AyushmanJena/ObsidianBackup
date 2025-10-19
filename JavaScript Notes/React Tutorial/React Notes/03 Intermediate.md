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

`const id = useId()`
It will generate an id for the component. 
And the id will be different for each time you render the component.

user the id value in html attributes 

Note : Do not call useId to generate keys in a field

App.jsx
```jsx
import { useState, useCallback, useEffect, useRef } from 'react'
import './App.css'
import InputBox from './components/InputBox';
import useCurrencyInfo from './hooks/useCurrencyInfo'
import { jsx } from 'react/jsx-runtime';


function App() {

    const [amount, setAmount] = useState(0);
    const [from, setFrom] = useState("usd");
    const [to, setTo] = useState("inr");
    const [convertedAmount, setConvertedAmount] = useState(0);

    const currencyInfo = useCurrencyInfo(from);

    const options = Object.keys(currencyInfo);

    const swap = () => {
        setFrom(to);
        setTo(from);
        setConvertedAmount(amount);
        setAmount(convertedAmount);
    }

    const convert = () => {
        setConvertedAmount(amount * currencyInfo[to]);
    }
  
    return (
        <div
            className="w-full h-screen flex flex-wrap justify-center items-center bg-cover bg-no-repeat"
            style={{
                backgroundImage: `url('${BackgroundImage}')`,
            }}
        >
            <div className="w-full">
                <div className="w-full max-w-md mx-auto border border-gray-60 rounded-lg p-5 backdrop-blur-sm bg-white/30">
                    <form
                        onSubmit={(e) => {
                            e.preventDefault();
                           convert()
                        }}
                    >
                        <div className="w-full mb-1">
                            <InputBox
                                label="From"
                                amount = {amount}
                                currencyOptions= {options}
                                onCurrencyChange={(currency) =>setFrom(amount)}
                                selectCurrency={from}
                                onAmountChange={(amount) => setAmount(amount)}
                            />
                        </div>
                        <div className="relative w-full h-0.5">
                            <button
                                type="button"
                                className="absolute left-1/2 -translate-x-1/2 -translate-y-1/2 border-2 border-white rounded-md bg-blue-600 text-white px-2 py-0.5"
                                onClick={swap}
                            >
                                swap
                            </button>
                        </div>
                        <div className="w-full mt-1 mb-4">
                            <InputBox
                                label="To"
                                amount = {convertedAmount}
                                currencyOptions= {options}
                                onCurrencyChange={(currency) => {setTo(currency)}}
                                selectCurrency={to}
                                
                            />
                        </div>
                        <button type="submit" className="w-full bg-blue-600 text-white px-4 py-3 rounded-lg">
                            Convert 
                        </button>
                    </form>
                </div>
            </div>
        </div>
    );

}

export default App;


```

useCurrencyInfo.js     : CUSTOM HOOK
```js
import { useEffect, useState } from "react";

function useCurrencyInfo(currency){

    const [data, setData] = useState({});

    useEffect(() => {
        fetch(`https://cdn.jsdelivr.net/gh/fawazahmed0/currency-api@1/latest/currencies/${currency}.json`)
        .then((res) => res.json())
        .then((res) => setData(res[currency]))
    }, [currency]);
    console.log(data);
    return data;
}

export default useCurrencyInfo;
```

InputBox.jsx : Custom Component
```jsx
import {React, useId } from 'react'

    
function InputBox({
    label,
    amount,
    onAmountChange,
    onCurrencyChange,
    currencyOptions = [],
    selectCurrency = "use",

    className = "",
}) {

    const amountInputId = useId();
   

    return (
        <div className={`bg-white p-3 rounded-lg text-sm flex ${className}`}>
            <div className="w-1/2">
                <label htmlFor={amountInputId} className="text-black/40 mb-2 inline-block">
                    {label}
                </label>
                <input
                    id={amountInputId}
                    className="outline-none w-full bg-transparent py-1.5"
                    type="number"
                    placeholder="Amount"
                    value={amount}
                    onChange={(e) => {onAmountChange && onAmountChange(Number(e.target.value))}} // if onAmountChange exists then perform
                />
            </div>
            <div className="w-1/2 flex flex-wrap justify-end text-right">
                <p className="text-black/40 mb-2 w-full">Currency Type</p>
                <select
                    className="rounded-lg px-1 py-1 bg-gray-100 cursor-pointer outline-none"
                    value={selectCurrency}
                    onChange={onCurrencyChange && onCurrencyChange(e.target.value)}
                >
                    
                        {currencyOptions.map((currency)=> (
                            <option key={currency} value={currency}>{currency}</option>
                        )) }
                
                </select>
            </div>
        </div>
    );
}

export default InputBox;
```



# Hydration
In frontend development, hydration is the process of transforming static, server-rendered HTML into a fully interactive and dynamic web page on the client side.


Hydration: This is where the "hydration" occurs. The client-side JavaScript, typically from a framework like React, Angular, or Vue, takes over the server-rendered HTML. Instead of re-rendering the entire page from scratch, it "hydrates" the existing HTML by:
- Attaching event listeners: Making elements interactive, allowing users to click buttons, fill out forms, and trigger other actions. 
- Initializing component state: Setting up the dynamic data and internal state that drives the application's behavior.
- Reconciling the DOM: Comparing the server-rendered HTML with the client-side component tree and making any necessary updates or adjustments without re-creating the entire DOM structure.


# React Router

 React Router is not a part of the React core. 
 It is a 3rd party library.
reactrouter.com

Installing React Router : 


6:25:25
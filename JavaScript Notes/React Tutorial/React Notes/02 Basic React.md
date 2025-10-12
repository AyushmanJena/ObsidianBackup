# Basic React 
Using variables in code: 
```jsx
function App(){
	const username = "Vulcan";
	return (
		<>
			<h1>Hello, {username}</h1>
		</>
	)
}
```

{xyz} -> Evaluated Expressions i.e. final outcome. 
Cannot add js expressions like if else, loops, etc.


## Hooks 

Project : Build a simple app, which shows a counter, 2 buttons. One button increases the counter and another button decreases the counter.

React won't automatically change values on view page based on change in variables in the code.
You need to specify them. 
This is done through hooks.

Hooks  : 
 React provides hooks to perform a specific task.
 e.g. useState : change the state throughout the UI / DOM

useState in the array takes two values 
1. variable : counter
2. function : setCounter
- you can set variable and function name any way, just follow the convention

useState takes default value in method
```jsx

 import React, { useState } from "react";

function App() {

  let [counter, setCounter] = useState(15) // default value passed

  function addValue(){
    console.log("value added");
    setCounter(counter+1)
  }

  function subtractValue(){
    console.log("value subtracted");
    setCounter(counter - 1);
  }

  return (
    <>
      <h2>Counter value: {counter}</h2>
      <br/>
      <button onClick={addValue}>Add Value</button>
      <br />
      <button onClick={subtractValue}>Subtract Value</button>
    </>
  )
}

export default App
 
```


# Virtual DOM, Fiber, Reconcillation
- Virtual DOM is not used in react anymore 

- When page is reloaded, only changed elements and values are changed in Virtual DOM.

 Not much important for projects, but might be needed for Interviews.

Article : https://github.com/acdlite/react-fiber-architecture

React Fiber is an implementation of React's core algorithm. It is the culmination of over two years of research by the React team.

The goal of **React Fiber** is to increase its suitability for areas like animation, layout, and gestures. Its headline feature is **incremental rendering**: the ability to split rendering work into chunks and spread it out over multiple frames.

Other key features include the ability to pause, abort, or reuse work as new updates come in; the ability to assign priority to different types of updates; and new concurrency primitives.

 We've established that a primary goal of Fiber is to enable React to take advantage of scheduling. Specifically, we need to be able to
- pause work and come back to it later.
- assign priority to different types of work.
- reuse previously completed work.
- abort work if it's no longer needed.

**Reconciliation** is the algorithm behind what is popularly understood as the "virtual DOM." A high-level description goes something like this: when you render a React application, a tree of nodes that describes the app is generated and saved in memory. This tree is then flushed to the rendering environment — for example, in the case of a browser application, it's translated to a set of DOM operations. When the app is updated (usually via `setState`), a new tree is generated. The new tree is diffed with the previous tree to compute which operations are needed to update the rendered app.


# TailWind in React
**Better follow the instructions in tailwind vite guide**

Installing tailwind : 
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p  
```

If this does not work use tailwind 3 : 
```
npm uninstall tailwindcss
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
```

in tailwind.config.js change the content : 
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./index.html",
    "./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}

```

inject tailwind inside index.css : 
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

In react to use classes we use `className` instead of `class`
```jsx
import React, { useState } from "react";

function App() {

  return (
    <>
      <h1 className="bg-green-400 text-white p-4 rounded">Tailwind Test</h1>
    </>
  )
}

export default App
 
```

Note: in react every img tag has to be a closing tag

# Components in React
Creating and reusing components in react 
Card.jsx
```jsx
import React from 'react'

function Card() {
  return (
    <div>
        <div className="*:border-2 *:rounded-xl *:p-4 *:m-4 *:bg-slate-500 *:text-white  flex flex-col items-center justify-center h-screen w-full text-3xl font-bold text-white bg-black
        ">Hello Users</div>
    </div>
  )
}

export default Card
```

App.jsx
```jsx
import React, { useState } from "react";
import Card from "./components/Card";

function App() {

  return (
    <>
      <Card/>
      <Card/>
    </>
  )
}

export default App
 
```

# Props 
Props -> Properties
Used for passing values that can be used for dynamic values with each component. 
That is passed with each Component call.

props is an object that is passed to the component

```jsx
import React, { useState } from "react";
import Card from "./components/Card";

function App() {

  let myObj = {
    username : "ayush",
    age : 21
  }

  return (
    <>
      <Card channel="chaiaurcode" someObject={myObj} />
      <Card/>
    </>
  )
}

export default App;
```



Using the props in the component : 
```jsx
import React from 'react'

function Card(props) {
  return (
    <div>
        <div className="xyz">
	        Hello {props.username}
        </div>
    </div>
  )
}

export default Card;
```


If no props is passed, it would be default value. 
You can overwrite the default value  by passing a default value in the function parameters 
```jsx
function Card(username= "user") {
  return (
    <div>
        <div className="xyz">
	        Hello {username}
        </div>
    </div>
  )
}
```


# Interview Question on Counter

```jsx
import React, { useState } from "react";

function App() {

  let [counter, setCounter] = useState(15);

  function addValue(){
    console.log("value added");
    setCounter(counter+1)                // this part
    setCounter(counter+1) 
    setCounter(counter+1) 
    setCounter(counter+1) 
  }

  function subtractValue(){
    console.log("value subtracted");
    setCounter(counter - 1);
  }

  return (
    <>
      <h2>Counter value: {counter}</h2>
      <br/>
      <button onClick={addValue}>Add Value</button>
      <br />
      <button onClick={subtractValue}>Subtract Value</button>
    </>
  )
}

export default App
```

What will happen if we call setCounter 4 times, 
It still increases value by 1 each time.

This is due to useState() 
Since it is the same task performed 4 times. The calls are batched and performed only once.

To fix that we can use a callback, and passing the last updatedState

```jsx
function addValue(){
	setCounter((prevCounter) => prevCounter + 1)
	setCounter((counter) => counter + 1) // callback so works with any var
	setCounter((prevCounter) => prevCounter + 1)
	setCounter((prevCounter) => prevCounter + 1)
}
```

Now it increases the value by 4 everytime the addValue() is called

# Building 1st React App
A webapp, where there are multiple buttons of different colors and clicking any color changes the background color to that color.


Using Dynamic Background styles in react : 
```jsx
<div className='w-full h-screen duration-200'
      style={{backgroundColor: color }} >hello</div>
```

Changing the color using onClick on buttons : 
```jsx
<button onClick={() => setColor("red")} className='outline-none px-4 rounded-full text-white shadow-sm'
style={{backgroundColor: "red"}}>Red</button>
```
- onClick by default expects a function, so we use a callback


```jsx
import { useState } from 'react'
import './App.css'

function App() {
  const [color, setColor] = useState("olive");

  return (
    <>
      <div className='w-full h-screen duration-200'
      style={{backgroundColor: color }} >
        <div className='fixed flex flex-wrap justify-center bottom-12 inset-x-0 px-2'>
          <div className='flex flex-wrap gap-3 shadow-lg bg-white px-3 py-2 rounded-3xl'>
            <button onClick={() => {setColor("red")}} className='outline-none px-4 rounded-full text-white shadow-sm'
            style={{backgroundColor: "red"}}>Red</button>
            <button onClick={() => {setColor("green")}} className='outline-none px-4 rounded-full text-white shadow-sm'
            style={{backgroundColor: "green"}}>Green</button>
            <button onClick={() => {setColor("blue")}} className='outline-none px-4 rounded-full text-white shadow-sm'
            style={{backgroundColor: "blue"}}>Blue</button>
          </div>
        </div>
      </div>
    </>
  )
}

export default App

```

# UseEffect, useRef and useCallback with a project

PROJECT GOAL : Random password generator by specifying length, if numbers, if special chars included.


useCallback : UseCallback is a react hook that lets you cache a function definition between re-renders.
`const cachedFn = useCallback(fn, dependencies)`
pass a function and an array of dependencies 


useEffect : useEffect is a react hook that lets you synchronize a component with an external system.
`useEffect(setup, dependencies)`
with the change in dependencies the setup will be re-run 
setup can be a callback function 
dependencies is an array of function, variables, hooks, etc.


useRef : 

when one element need to perform some change based on another element we use references.
to use useRef you need a variable 
pass the variable in every argument that 

note:  you  can use a normal function for copyPasswordToClipboard but for better performance we are using callback hook 

```jsx
import { useState, useCallback, useEffect, useRef } from 'react'
import './App.css'
import { jsx } from 'react/jsx-runtime';

function App() {
  const [length, setLength] = useState(8);
  const [numberAllowed, setNumberAllowed] = useState(false);
  const [charAllowed, setCharAllowed] = useState(false);

  const[password, setPassword] = useState("");

  // useRef hook
  const passwordRef = useRef(null)

  const passwordGenerator = useCallback(() => {
    let pass= ""
    let str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz"
    if(numberAllowed){
      str+= "0123456789"
    }
    if(charAllowed){
      str+= "!@#$%^&*()_+~`|}{[]:;?><,./-="
    }

    for(let i = 1; i<=length; i++){
      let char = Math.floor(Math.random() * str.length + 1);
      pass += str.charAt(char);
    }
    setPassword(pass);

  },  [length, numberAllowed, charAllowed, setPassword]);

  const copyPasswordToClipboard = useCallback(() => {
    // passwordRef is used to display the text is selected and print it 
    passwordRef.current?.select();
    console.log(passwordRef.current.value);
    // How copy to clipboard works with react (not core js)
    window.navigator.clipboard.writeText(password);
  }, [password]);

  useEffect(() =>{
    passwordGenerator();
  }, [length, numberAllowed, charAllowed, passwordGenerator]);

  return (
    
    <div className="w-full max-w-md mx-auto shadow-md rounded-lg px-4 py-3 my-8 bg-gray-800 text-orange-500">
      <h1 className='text-white text-center my-3'>Password generator</h1>
    <div className="flex shadow rounded-lg overflow-hidden mb-4">
        <input
            type="text"
            value={password}
            className="outline-none w-full py-1 px-3"
            placeholder="Password"
            readOnly
            ref={passwordRef}                        // note the reference
        />
        <button
        onClick={copyPasswordToClipboard}
        className='outline-none bg-blue-700 text-white px-3 py-0.5 shrink-0'
        >copy</button>
        
    </div>
    <div className='flex text-sm gap-x-2'>
      <div className='flex items-center gap-x-1'>
        <input 
        type="range"
        min={6}
        max={100}
        value={length}
         className='cursor-pointer'
         onChange={(e) => {setLength(e.target.value)}}
          />
          <label>Length: {length}</label>
      </div>
      <div className="flex items-center gap-x-1">
      <input
          type="checkbox"
          defaultChecked={numberAllowed}
          id="numberInput"
          onChange={() => {
              setNumberAllowed((prev) => !prev);
          }}
      />
      <label htmlFor="numberInput">Numbers</label>
      </div>
      <div className="flex items-center gap-x-1">
          <input
              type="checkbox"
              defaultChecked={charAllowed}
              id="characterInput"
              onChange={() => {
                  setCharAllowed((prev) => !prev )
              }}
          />
          <label htmlFor="characterInput">Characters</label>
      </div>
    </div>
</div>
    
  )
}

export default App;
```



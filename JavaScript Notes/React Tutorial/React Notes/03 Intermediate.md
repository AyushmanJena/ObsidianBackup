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
`npm install react-router-dom`

**Link** replaces `<a>` tag, in use of a tag the whole page is reloaded, but using Link only react dom manipulation takes place.

```jsx
<Link to="/" className="flex items-center">
                        <img src="https://alexharkness.com/wp-content/uploads/2020/06/logo-2.png" className="mr-3 h-12" alt="Logo" />
</Link>
```

NavLink provides additional features like : 
- isActive variable that tells us if the link is active

Note :  Using className with callback helps to change css/tailwind stylings easily, 
for example when we are in /home endpoint and want to show the header home highlighted, this becomes easier with callback functions.
```jsx
<NavLink  to="/"
    className={({isActive}) =>
        `block py-2 pr-4 pl-3 duration-200 ${isActive ? "text-orange-700" : "text-gray-700"} border-b border-gray-100 hover:bg-gray-50 lg:hover:bg-transparent lg:border-0 hover:text-orange-700 lg:p-0` }> Home 
        </NavLink>
```

Since we are using React Router, we do not render the App Component in the main.jsx
```jsx
const router = createBrowserRouter([
  {path: '/', element: <Layout/>,
    children: [
      {path: '/', element: <Home/>},
      {path: '/about', element: <About/>},
    ]
  },
])

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <RouterProvider router={router} />
  </StrictMode>,
)
```

You need to create the router variable with the method createBrowserRouter() and which takes an array of all the routes we need

Make another Layout Component that will store the overall layout of the page 
e.g. Header, Any component based on Routes, Footer
This could have been done in App.jsx, but we need to make it independent.

```jsx
function Layout() {
  return (
    <>
        <Header/>
        <Outlet/>
        <Footer/>
    </>
  )
}

export default Layout
```

Alternate to using the previous style of router : 
```jsx
const router = createBrowserRouter([
  createRoutesFromElements(
    <Route path="/" element={<Layout/>}>
      <Route path="" element={<Home/>}/>
      <Route path="about" element={<About/>}/>
      <Route path="contact" element={<Contact/>}/>
    </Route>
  )
]);
```


### Rendering Dynamic Urls 
like UserId, ProductId, etc
```jsx
{path: '/user/:userId', element: <User/>},
```
Then using the param in component : 
```jsx

function User() {
  const { userId } = useParams();
  return (
    <div>User : {userId}</div>
  )
}

export default User
```

Making api calls and performing some actions on it : 
```jsx
import React from 'react'
import { useState, useEffect } from 'react'

function Github() {

    const [data, setData] = useState([]);

    useEffect(() => {
        fetch('https://api.github.com/users/ayushmanjena')
        .then(response => response.json())
        .then(data => {
            console.log(data);
            setData(data);
    })
    }, [])

  return (
    <div className="text-center m-4 bg-gray-600 text-white p-4 text-3xl">
        Github Followers : {data.followers}
        <img src={data.avatar_url} alt="git image" width={300} />
    </div>
  )
}

export default Github
```

#### Loader
Making the api call or some other tasks even before the route is loaded. 
```jsx
const router = createBrowserRouter([
  {path: '/', element: <Layout/>,
    children: [
      {path: '/', element: <Home/>},
      {path: '/about', element: <About/>},
      {path: '/contact', element: <Contact/>},
      {path: '/user/:userId', element: <User/>},
      {path: '/github', element: <Github />, loader: async () => {
        const response = await fetch('https://api.github.com/users');
        const data = await response.json();
        return data;}
      },
    ]
  },
])
```

or add an async method in the component and call that in the loader in routes

useLoaderData Hook is used to use data passed through the loader in the component.

```jsx
{path: '/github', element: <Github />, loader: async () => {
        githubInfoLoader() // a function preferably in the same component/service
      }
      },
```

```jsx
export function Github() {

    const data = useLoaderData();

  return (
    <div className="text-center m-4 bg-gray-600 text-white p-4 text-3xl">
        Github Followers : {data?.followers}
        <img src={data?.avatar_url} alt="git image" width={300} />
    </div>
  )
}

export const githubInfoLoader = async() => {
    const response = await fetch('https://api.github.com/users/ayushmanjena')
    return response.json();
}
```


# Context APIs
Project : Theme Toggle 
In multiple nested components, if you want to pass a prop from grand-parent to child, you would have to pass it through the parent. It makes code redundant. 

This can be solved/optimised using Context APIs

Grand-parent passes to a global file and the child can access the same data from the same global file. This is called prop drilling.

Redux Library -> State Management 
react-redux (for react) also solves the same problem
Redux-toolkit (RTK)

Sustand -> Another state management library


Create a new file src/context/UserContext.js
Note: Not jsx

UserContext.js
```js
import React from 'react'

const UserContext = React.createContext()

export default UserContext;
```

UserContextProvider.jsx
```jsx
import React from 'react'
import UserContext from './UserContext'

const UserContextProvider = ({children}) => {

    const [user, setUser] = React.useState(null);

    return(
        <UserContext.Provider value={{user, setUser}}>
        {children}
        </UserContext.Provider>
    )
}

export default UserContextProvider;
```

This UserContext we created works as a provider, that provides data
And everytime we put any view layout inside `<UserContext> ... <UserContext />` then the layout inside the UserContext can access the provider and access its data
Context works as a global Variable

children -> what we provide inside the tags like divs, other components, etc.

Accessing the store in other components

App.jsx 
```jsx
import { useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from './assets/vite.svg'
import heroImg from './assets/hero.png'
import './App.css'
import UserContextProvider from './context/UserContextProvider.jsx'
import Login from './components/Login'
import Profile from './components/Profile'
import { useContext } from 'react'


function App() {
  

  return (
    <UserContextProvider>
      <h1>React with Chai</h1>
      <Login />
      <Profile />
    </UserContextProvider>
  )
}

export default App

```

make components one that will set the value and another that would use the value

Login.jsx
```jsx
import React from 'react'
import {useState, useContext} from 'react'
import {UserContext} from '../context/UserContext'

function Login() {

    const [username, setUsername] = useState('')
    const [password, setPassword] = useState('')
    const {setUser} = useContext(UserContext)

    const handleSubmit = (e) => {
        e.preventDefault()
        setUser({username, password});
    }


  return (
    <div>
        <h2>Login</h2>
        <input type="text" placeholder='username' 
            value={username} 
            onChange={(e) => setUsername(e.target.value)}/>
        <input type="password"
            value={password} 
            onChange={(e) => setPassword(e.target.value)}
            placeholder='password' />
        <button onClick={handleSubmit}>Login</button>
    </div>
  )
}

export default Login
```

Profile.jsx
```jsx
import React, {useContext} from 'react';
import {UserContext} from '../context/UserContext';

function Profile() {

    const {user} = useContext(UserContext)

    if(!user) return <div>Please Login</div>

  return (
    <div>
      <h2>Profile</h2>
      <p>Username: {user.username}</p>
      <p>Password: {user.password}</p>
    </div>
  )
}

export default Profile
```


# Theme Switcher Project for Context API p2

New way to create a context : 

theme.js
```js
import {createContext, useContext} from 'react';

export const ThemeContext = createContext({
    themeMode : 'light',
    darkTheme : () => {
    },
    lightTheme: () => {
    }
}); // default value CAN be passed

export const ThemeProvider = ThemeContext.Provider;

// a custom hook that can be used
export default function useTheme() {
    return useContext(ThemeContext);
}
```
you can pass both variables and methods inside the contexts

Now we do not use to import useContext and UserContext as well, we can only import useTheme and it will give use ThemeContext automatically.


App.jsx 
```jsx
import { useState } from 'react'
import './App.css'
import { ThemeProvider } from './contexts/theme'
import { useEffect } from 'react'
import ThemeBtn from './components/ThemeBtn'
import Card from './components/Card'

function App() {
  const [themeMode, setThemeMode] = useState('light');

  const lightTheme = () => {
    setThemeMode('light');
  }

  const darkTheme = () => {
    setThemeMode('dark');
  }

  // actual change in theme 
  useEffect(() => {
    document.querySelector('html').classList.remove("light", "dark");
    document.querySelector('html').classList.add(themeMode);
  }, [themeMode])


  return (
    <ThemeProvider value={themeMode, lightTheme, darkTheme}>
      <div className="flex flex-wrap min-h-screen items-center">
        <div className="w-full">
          <div className="w-full max-w-sm mx-auto flex justify-end mb-4">
            <ThemeBtn />
          </div>

          <div className="w-full max-w-sm mx-auto">
            <Card />
          </div>
        </div>
      </div>
    </ThemeProvider>


  )
}

export default App

```
the functionality is not defined in the Context, or we need to overwrite them
In such cases create methods with the same name i.e. `lightTheme()` or `darkTheme()` then, the functionality is automatically transferred into the methods defined.

Card.jsx
```jsx
import React from 'react'

export default function Card() {
    return (
        <div className="w-full bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700">
            <a href="/">
                <img className="p-8 rounded-t-lg" src="https://images.pexels.com/photos/18264716/pexels-photo-18264716/free-photo-of-man-people-laptop-internet.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2" alt="product_image1" />
            </a>
            <div className="px-5 pb-5">
                <a href="/">
                    <h5 className="text-xl font-semibold tracking-tight text-gray-900 dark:text-white">
                        Apple Watch Series 7 GPS, Aluminium Case, Starlight Sport
                    </h5>
                </a>
                <div className="flex items-center mt-2.5 mb-5">
                    <svg
                        className="w-4 h-4 text-yellow-300 mr-1"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="currentColor"
                        viewBox="0 0 22 20"
                    >
                        <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
                    </svg>
                    <svg
                        className="w-4 h-4 text-yellow-300 mr-1"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="currentColor"
                        viewBox="0 0 22 20"
                    >
                        <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
                    </svg>
                    <svg
                        className="w-4 h-4 text-yellow-300 mr-1"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="currentColor"
                        viewBox="0 0 22 20"
                    >
                        <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
                    </svg>
                    <svg
                        className="w-4 h-4 text-yellow-300 mr-1"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="currentColor"
                        viewBox="0 0 22 20"
                    >
                        <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
                    </svg>
                    <svg
                        className="w-4 h-4 text-gray-200 dark:text-gray-600"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="currentColor"
                        viewBox="0 0 22 20"
                    >
                        <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
                    </svg>
                    <span className="bg-blue-100 text-blue-800 text-xs font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-blue-200 dark:text-blue-800 ml-3">
                        4.0
                    </span>
                </div>
                <div className="flex items-center justify-between">
                    <span className="text-3xl font-bold text-gray-900 dark:text-white">$599</span>
                    <a
                        href="/"
                        className="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                    >
                        Add to cart
                    </a>
                </div>
            </div>
        </div>
    );
}

```

ThemeBtn.jsx
```jsx
import React from 'react'
import useTheme from '../contexts/theme'
import { useState } from 'react'


export default function ThemeBtn() {
    
    const {themeMode, lightTheme, darkTheme} = useTheme() 

    const onChangeBtn = (e) => {
        const darkModeStatus = e.currentTarget.checked;
        if(darkModeStatus) {
            darkTheme();
        } else {
            lightTheme();
        }
    }

    return (
        <label className="relative inline-flex items-center cursor-pointer">
            <input
                type="checkbox"
                value=""
                className="sr-only peer" 
                onChange={onChangeBtn}
                checked={themeMode === 'dark'}
            />
            <div className="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"></div>
            <span className="ml-3 text-sm font-medium text-gray-900">Toggle Theme</span>
        </label>
    );
}


```

### Backend Setup Code 

index.js
```js
import express from "express";

const app = express();

const port = process.env.PORT || 3000;

app.get("/api/products", (req, res) => {
    const products = [{
        id: 1,
        name: "table wooden", 
        price: 200,
        image: 'https://images.pexels.com/photos/164595/pexels-photo-164595.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940'
    }, {
        id: 2,
        name: "glass table", 
        price: 200,
        image: 'https://images.pexels.com/photos/164595/pexels-photo-164595.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940'
    }, {
        id: 3,
        name: "broken metal", 
        price: 200,
        image: 'https://images.pexels.com/photos/164595/pexels-photo-164595.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940'
    }, ];

    setTimeout(() => {
        res.send(products);
    }, 3000);

    // https://localhost:3000/api/products?search=metal

    if(req.query.search){
        const filterProducts  = products.filter(product => product.name.includes(req.query.search))
        res.send(filterProducts);
        return;
    }
})

app.listen(port, () => {
    console.log(`server running on port ${port}`);
});
```

`npm run start` to run backend

# Frontend Part

Then create a react project

Install Axios
`npm install axios`

Set proxy in vite.config.js to avoid CORS errors : 
```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  server: {
    proxy: {
      '/api': "http://localhost:3000"
    }
  },
  plugins: [react()],
})

```

App.jsx
```jsx
import { useState } from 'react'
import './App.css'
import axios from 'axios'
import {useEffect} from 'react'

function App() {
  const [products, setProducts] = useState([]);
  const[error, setError] = useState(false);

  useEffect(() => {
    (async () => {
      try{
        setError(false); // if fetch was unsuccessful earlier
        const response = await axios.get('/api/products')
        console.log(response.data);
        setProducts(response.data);
      }catch(error){
        // in case data fetch fails 
        setError(true);
      }
      
    })();

  } , [])

  if(error){
    return <h1>Something went wrong</h1>
  }

  return (
    <>
      <h1>Chai Aur API in react</h1>
    <h2>Number of Products are : {products.length}</h2>  
    </>
    
  )
}

export default App

```


()() -> IIF -> Immediately Invoked Functions
To avoid .then promise wait 

Note : First time the component is double mounted, (only in development, not in production)


#### Adding Loading fallback to display the data is being fetched
App.jsx
```jsx
import { useState } from 'react'
import './App.css'
import axios from 'axios'
import {useEffect} from 'react'

function App() {
  const [products, setProducts] = useState([]);
  const[error, setError] = useState(false);
  const[loading, setLoading] = useState(false);

  useEffect(() => {
    (async () => {
      try{
        setLoading(true);
        setError(false); // if fetch was unsuccessful earlier
        const response = await axios.get('/api/products')
        console.log(response.data);
        setProducts(response.data);
        setLoading(false);
      }catch(error){
        // in case data fetch fails 
        setError(true);
        setLoading(false);
      }
      
    })();

  } , [])

  if(error){
    return <h1>Something went wrong</h1>
  }

  if(loading){
    return <h1>Loading...</h1>
  }

  return (
    <>
      <h1>Chai Aur API in react</h1>
      <h2>Number of Products are : {products.length}</h2>  
    </>
  )
}

export default App

```


### Convert the data fetch, loading and error into a single hook

To avoid code duplication make a hook and reuse it multiple times :
Can make the function in the same file or different file 

``` jsx
import { useState } from 'react'
import './App.css'
import axios from 'axios'
import {useEffect} from 'react'

function App() {

  const [products, error, loading ] = customReactQuery("/api/products");

  if(error){
    return <h1>Something went wrong</h1>
  }

  if(loading){
    return <h1>Loading...</h1>
  }

  return (
    <>
      <h1>Chai Aur API in react</h1>
      <h2>Number of Products are : {products.length}</h2>  
    
    </>
    
  )
}

export default App

const customReactQuery = (urlPath) => {
  const [products, setProducts] = useState([]);
  const[error, setError] = useState(false);
  const[loading, setLoading] = useState(false);

  useEffect(() => {
    (async () => {
      try{
        setLoading(true);
        setError(false); // if fetch was unsuccessful earlier
        const response = await axios.get(urlPath)
        console.log(response.data);
        setProducts(response.data);
        setLoading(false);
      }catch(error){
        // in case data fetch fails 
        setError(true);
        setLoading(false);
      }
      
    })();

  } , [])

  // finally return all the states
  return [ products, error, loading ]
}
```


### Conditional Rendering react specific not axios
Instead of using multiple if statements and returning a template return but check conditions inside the only return statement
```jsx
return (
    <>
      <h1>Chai Aur API in react</h1>
      <input type="text" placeholder="text"></input>

      {loading && <h1>Loading...</h1>}

      {error && <h1>Error</h1>}

      <h2>Number of Products are : {products.length}</h2>  
    </>
  )
```


#### Search to make specific api calls
```jsx
import { useState } from 'react'
import './App.css'
import axios from 'axios'
import {useEffect} from 'react'

function App() {
  const [products, setProducts] = useState([]);
  const[error, setError] = useState(false);
  const[loading, setLoading] = useState(false);
  const [search, setSearch] = useState('');

  useEffect(() => {

    const controller = new AbortController();

    (async () => {
      try{
        setLoading(true);
        setError(false);



        const response = await axios.get('/api/products?search=' + search, {
          signal: controller.signal
        });



        console.log(response.data);
        setProducts(response.data);
        setLoading(false);
      }catch(error){
        
        if(axios.isCancel(error)){               // handling cancelled requests
          console.log("Request cancelled");
          return;
        }

        setError(true);
        setLoading(false);
      }
      
    })();


    return() => { // clean up method, clear data from 
      controller.abort();
    }
  } , [search])

  return (
    <>
      <h1>Chai Aur API in react</h1>
      <input type="text" placeholder="Search" 
      value={search} onChange={(e) => setSearch(e.target.value)} />

      {loading && <h1>Loading...</h1>}

      {error && <h1>Error</h1>}

      <h2>Number of Products are : {products.length}</h2>  
    </>
  )
}

export default App
```
Call useEffect when search values are changed but when everytime you type in a call is made (race condition) and multiple unnecessary calls are made
To avoid that we use AbortController, that makes sure the order of requests and responses received remains the same, so the user can see the updated UI.
Now you can send configurations in the api call itself

The signal cancels the older requests and sends them to the catch block, so we need to handle them properly in that block too.


# Learn Other method calls as well : POST, DELETE, ETC.


Other concepts to learn : 
1. Race Condition
2. Debouncing
3. De-throttling



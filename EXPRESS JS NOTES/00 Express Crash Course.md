Node js and express js tutorial

https://youtu.be/ha_leEpnT30?si=aSa4W8J3ShpxImtk

`npm init -y`
creates package.json

`npm install express`
creates node_modules

create new file `server.js` new file

server.js
```js
const express = require('express')
const app = express()

app.get('/', (req, res) => {
	res.send("Hell from express")
}) 

app.get('/about', (req, res) => {
	res.send("This is the about page")
})

// routes returning json
app.get('/products', (req, res)=> {
	res.json([
		{id: 1, name: "Laptop", price: 1299},
		{id: 2, name: "Mouse", price: 99}
	])
})

// route parameters
app.get('/products/:id', (req, res)=> {
	const id = Number(req.params.id)
	const products = [
		{id: 1, name: "Laptop", price: 1299},
		{id: 2, name: "Mouse", price: 99}
	]
	
	const requestedProduct = products.find((product) => product.id === id)
	
	res.json(requestedProduct)
})

app.listen(3000, () => {
	console.log("This server is running')
})
```

require() -> built in node.js function that loads other files or libraries into code.
When you call require('express'), node.js looks in node_modules and gives you access to its functions so you can build a web server.

Route -> Route is simply an instruction that says, if someone visits a specific url send back a specific response. 
In express, you create routes with methods llike app.get() or app.post(), and inside the route handler you decide which data to send back or which action to perform

JSON -> Javascript Object Notation 


Fixing CORS
CORS forces the browser to check if the frontend is allowed to talk to the backend. 

to fix CORS add this to the server.js
```js
const cors = require('cors')

app.use(cors({
	origin: ['https://localhost:5500', 'https://127.0.0.1.5500']
}))
// every domain added here will be allowed to talk to the backend
```

also do `npm install cors`

### POST Routes
forms, buttons, etc.

1. Enable JSON body parsing
```js
app.use(express.json())
```
this tells express : everytime a request comes in with json data, parse that json data and put that result into request.body
If you forget that line request.body will be undefined.

```js
app.post('/message', (req, res) => {
	const {name, message} = req.body
	// or const name = req.body.name; const message = req.body.message
	
	console.log(message)
	res.json({message : "Thank you for the message"}) // returning response
})
```


Similarly 
```
app.post('/products', (req, res) => {
// Create a new product
});

app.get('/products/:id', (req, res) => {
// Get one product by ID
});

app.put('/products/:id', (req, res) => {
// Update a product
});

app.delete('/products/:id', (req, res) => {
// Delete a product
});
```

# Express Router
It lets you split you r routes into smaller modules and group things that belong together. Making your code smaller, more readable and organised.

- Create a new file  : `products.js`
- Import express
- Create a router object
- Export that router

products.js
```js
const express = require('express')
const router = express.Router()

router.get('/', (req, res)=> {
	res.json([
		{id: 1, name: "Laptop", price: 1299},
		{id: 2, name: "Mouse", price: 99}
	])
})

// route parameters
router.get('/:id', (req, res)=> {
	const id = Number(req.params.id)
	const products = [
		{id: 1, name: "Laptop", price: 1299},
		{id: 2, name: "Mouse", price: 99}
	]
	
	const requestedProduct = products.find((product) => product.id === id)
	
	res.json(requestedProduct)
})

router.post('/', (req,res) => {
	const {name, price} = req.body
	const newProduct = {
		name,
		price
	}
	console.log(newProduct)
	res.json({message : "New Product Added", product : newProduct})
})

module.exports = router
```
instead of app.get we write router.get
We can also delete '/products' from every path

Update the server.js file
server.js
```js
const cors = require('cors')
const express = require('express')
const productsRouter = require('./products')               // IMP
const app = express()

app.use(express.json())

app.use('/products', productsRouter)                           // IMP

app.get('/', (req, res) => {
	res.send("Hell from express")
}) 

app.get('/about', (req, res) => {
	res.send("This is the about page")
})

app.use(cors({
	origin: ['https://localhost:5500', 'https://127.0.0.1.5500']
}))

app.listen(3000, () => {
	console.log("This server is running')
})
```

### Important stuff
Express Executes things from top to bottom 
cors -> express json -> routes -> app then routes and finally listen 

/special first then /:id
or else it will confuse special with a specific type dynamic parameter


### Middleware
A middleware is simply a function that runs between incoming request and the final route handler

```js
function exampleMiddleware (req, res, next) {
	const { username, email } = req.body
	const isAdmin = checkForAdmin(username, email)

	if(isAdmin){
		return res.send('Welcome Admin')
	}

	next() //run next middleware or route handler
}
```
All the app.use function calls we made are middleware 

Building a custom middleware 
```js
app.use((req, res, next) => {
	console.log(req.method, req.path)
	next()
})
```

Common use cases for middlewares : 
- Logging requests
- Checking authentication and permissions
- Validating data before it reaches the route handler
- handling file uploads
- error handling 
Initializing a new Node Project
```bash
// Create Project Folder
mkdir my-node-project
cd my-node-project

// Initializing the nodejs project : 
npm init -y

// install express package
npm i express

// Create entry file 
tough server.js 
or 
touch app.js

// Executing the project : 
node server.js

// run the project after any change automatically
npx nodemon server.js
```


server.js
```js
// import express
const express = require('express');

// create an instance of the server
const app = express()

app.get("/", (req, res) => {
	res.send("Hello World")
})

app.get("/about", (req, res) => {
	res.send("About Page")
})

// start the server
app.listen(3000)
```

Now we can access the endpoints 
`localhost:3000/` and `localhost:3000/about`


req -> request
res -> response

### API
Set of rules or protocols that allows different software programs to communicate and exchange data and functionality with each other.

Types of API : 
- REST (Representational State Transfer) APIs
- SOAP (Simple Object Access Protocol) APIs
- RPC (Remote Procedure Call) APIs
### REST API 
Representational State Transfer 
REST API is a set of rules and design guidelines that allows different software applications to communicate with each other over the internet. 
It relies on stateless, client-server communication over HTTP using standard methods and status codes.

HTTP Methods : 
**GET** -> to fetch data from the server
**POST** -> to send data to the server
**PUT** -> 
**PATCH** ->  to update data already on the server
**DELETE** -> to delete the data on the server


### FOLDER STRUCTURE
```
/node_modules
package-lock.json
package.json

server.js

/src 
	app.js
```

- app.js -> main task is to create the server
- server.js -> main task is to start the server


export the app in app.js
import the app in server.js and start the server

app.js
```js
// create the server
const express = require('express');
const app = express()

module.exports = app
```

server.js
```js
const app = require("./src/app") // import the server instance from app.js

app.listen(3000, () => { // this callback is executed when the server starts
  console.log("server is running on port 3000")
})
```


object structure for our note object
```
note = {
	title : "my first note",
	description : "this is my first note"
}

const notes = [
	{
		title : "my first note",
		description : "this is my first note"
	},
	{
		title : "my first note",
		description : "this is my first note"
	},
]
```

Creating a POST API
using which user can send a note object to the server

app.js
```js
// create the server

const express = require('express');

const app = express()

app.use(express.json())

const notes =[]

app.post('/notes', (req, res) => {
    notes.push(req.body)
    res.status(201).json({ // resource created status code 201
        message: "note created successfully"
    })
    console.log(req.body)
})

module.exports = app
```

we purposely put the input data in req.body i.e. request body
It can be accessed with `req.body`

We need to use a middleware to access the data 
express.json()

CREATING GET METHOD : 

```js
app.get('/notes', (req, res)=> {
    res.status(200).json({
        message : "notes fetched successfully",
        notes : notes
    })
})
```

CREATING DELETE METHOD : 
Deleting notes with param index number 
:index -> signifies this would be dynamic
```js
app.delete('/notes/:index', (req, res) => {
    const index = req.params.index;
    delete notes[index]
    res.status(200).json({
        message: "note deleted successfully"
    })
})

```
You can access the params values by `req.params.name`

PATCHING METHOD : 
Updating data already on the server
```js
app.patch('/notes/:index', (req, res) => {
    const index = req.params.index;
    const description = req.body.description
    notes[index].description = description;

    res.status(200).json({
        message: "note updated successfully"
    })
})

```


# DATABASE SETUP and IMPLEMENTATION
MongoDB would be used here

CLUSTER : A machine which we can configure, we can specify its ram, cpu, storage, etc.

The mongodb database exists in the server we create.


Types of server : 
- Web Servers : for hosting websites
- Mail Servers : for email
- File Servers : for data storage
- Database Servers : for data management 
- Application Servers : for running applications
- Proxy Servers : as intermediaries


### Two Access Layers
Network Access Layer
Database Access Layer 

**Network Access Layer** :
server runs on local machine
data to be stored in cluster located in some other region

first step is to connect our server and the mongodb server/cluster
We need to make sure only our server is connected and no other machine can access the cluster
This is **configuring the database** in Network Access Layer
We allow the ip address of our local machine in the database server

Since local machine ip changes frequently, while in development set it to allow access from anywhere. : `0.0.0.0/0`


**Database Access Layer :** 
Main operations on database : CRUD  

We need to configure the access of different types of users based on their roles.

Ex : Admin can perform all operations
Normal User can only read 
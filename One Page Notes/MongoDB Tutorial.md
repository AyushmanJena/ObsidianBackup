playlist : [https://www.youtube.com/playlist?list=PL4cUxeGkcC9h77dJ-QJlwGlZlTd4ecZOA](https://www.youtube.com/playlist?list=PLA3GkZPtsafZydhN4nP0h7hw7PQuLsBv1)

https://www.youtube.com/playlist?list=PLA3GkZPtsafZydhN4nP0h7hw7PQuLsBv1
## Introduction
NoSQL database server
Schemaless
Highly Scalable

Database -> Collection (Table) -> Documents (Rows)

collection -> employee
document -> individual employee information

different documents can have different structures
ADD EXAMPLE HERE

Behind the scenes data is stored in BSON : Binary JSON 
- BSON's binary structure encodes type and length information, which allows it to be traversed much more quickly compared to JSON.

### Creating in MongoDB :

Show all existing dbs :
```
show dbs 
```

Create db or select a db you want to use : 
```
use new_db
```
if it exists it is selected, else created.

Create collection in db 
db -> the db you are in 
students -> collection name 
```
db.students.insertOne({name:"Ayush", age:21})
```
if collection exists then document is inserted else the collection is created.

show data in collection
```
db.students.find()
```


### CRUD Operations in MongoDB

Create :
insertOne(data, options)
insertMany(data, options)
```

```

Read :
find(filter, options)
findOne(filter, options)
```
db.students.find()

// students with age = 11
db.students.find({age:11})

// only one student with age = 11 (the first one)
db.students.findOne({age:11})

```
find returns list whereas findOne returns object

Update :
updateOne(filter, data, options)
updateMany(filter, data, options)
replaceOne(filter, data, options)

Delete :
deleteOne(filter, options)
deleteMany(filter, options)
## MongoDB Intorduction

- DB engine tool that we can use to run NoSQL DB
- It is a server that allows us to run different DB on it
- It is build to store & use large data; built for large scale application, to quickly query data, store data & interact with data; mongo from work humongous
- Shop DB; Inside this we have multiple collections: Users, Orders; Inside collections we have multiple documents
- Documents are SCHEMALESS, there is no strict schema for every document inside collection
- ![Mongodb](image.png)
- Inside document, we use JS object to store data. MongoDb use JSON(JS object notation) to store data in collections
- To be precise mongodb uses BSON for binary json which it transforms BTS(from JSON to BSON) before storing it into files
- ![JSON format](image-1.png)
- We can store nested/embdedded data(address) inside document. This allows us complex relations between data & store them in one same document which make working & fetching it super efficient
- MongoDB characteristics
  - Schemaless
  - No/Few Realtions: Relational data needs to be merged manually
- Relations Options (![Relations options](image-2.png))
  - Nested/embedded document
    - If we have lot of data duplications, then we have to update it at all places
  - References
    - We will add reference of data in one table of another
    - We should not relate the documents too much as our queries become slow
- ![MongoDB ecosystem](image-3.png)
- In cmd : net stop MongoDB (stops the mongo db server); net start MongoDB (starts the server)
- After installing mongodb server & mongo shell
- In cmd -> mongosh : it will open mongo shell; show dbs : show the databses present in shell
- ![Working with MongoDB](image-4.png)
- ![with shell](image-5.png)
- ![Closer look to data](image-6.png)

## BASICS & CRUD OPERATIONS

# DB, Collection, Documents

- We can have one or more DB on our DB server
- Each DB can hold one or more collections (table in SQL)
- In collection there are multiple documents (data)
- These all are created implicitly

# Creating DB & Collections

- In cmd -> mongosh
- use: use flights : if this db not exist then it will create flights db & switch to it
- insertOne : db.flightData.insertOne({ "departureAirport": "MUC", "arrivalAirport": "SFO", "aircraft": "Airbus A380", "distance": 12000, "intercontinental": true }) : insert this one object to db as document (create flightData collection also); Mongodb auto generates unique \_id
- find : db.flightData.find().pretty() : show the documents inside flightData in pretty format
- We can also manually assign \_id to document : db.flightData.insertOne({ "departureAirport": "IXC", "arrivalAirport":"PNQ", \_id:"ixc-pnq-1" })
- We cannot add 2 documents with same \_id, else it will give error
- ![JSON BSON](image-7.png)

# CRUD

- We have MongoDB driver for each language, Eg: for nodejs, java etc
- ![CRUD](image-9.png)
- Create
  - insertOne(data, Options): allows to insert some data to collection
  - insertMany(data, Options)
- Read
  - find(filter, options): find the documents inside collection, find all the matching documents
  - findOne(filter, options): find 1st matching document
- Update
  - updateOne(filter, data, options): to change one piece of data
  - updateMany(filter, data, options)
  - replaceOne(filter, data, options): to replace the document entirely with new one
- Delete
  - deleteOne(filter, options)
  - deleteMany(filter, options)
- ![CRUD Eg](image-8.png)
- deletOne: db.flightData.deleteOne({departureAirport:"IXC"}): find the first document in collection with this, then it will delete that one
- updateOne: db.flightData.updateOne({distance:12000},{$set:{marker: "delete"}}): '$set' tell mongoDb to set this value for thr specified filter
- updateMany: db.flightData.updateMany({}, {$set:{marker:'toDelete'}}): updated all the documents; If marker already exists in document it will be replace & if not present then added to document
- deleteMany: db.flightData.deleteMany({marker:'toDelete'}) : delete documents with this filter
- insertMany: db.flightData.insertMany([ { "departureAirport": "MUC", "arrivalAirport": "SFO", "aircraft": "Airbus A380", "distance": 12000, "intercontinental": true }, { "departureAirport": "LHR", "arrivalAirport": "TXL", "aircraft": "Airbus A320", "distance": 950, "intercontinental": false }]): passed array of objects.
- find(): db.flightData.find({intercontinental:true}).pretty()
- find() with greater than filter: db.flightData.find({distance:{$gt:10000}}): '$gt' is greater than; '$lt' is less than, query gives all the document with filter
- findOne(filter): give 1st document that matches the filter
- if we paas {} as filter it means we want all the data in that case
- updateOne: db.flightData.updateOne({\_id:ObjectId('665d4d4bcf2442da28cdcdf8')}, {$set: {"delayed":true}}) : Update one element based on filter
- updateMany: db.flightData.updateMany({\_id:ObjectId('665d4d4bcf2442da28cdcdf8')}, {$set: {"delayed":false}}) : update for all
  updateMany:
- update is deprecated
- replaceOne: db.flightData.replaceOne({ \_id: ObjectId('665d4d4bcf2442da28cdcdf8') }, { departureAirport: 'MUC', arrivalAirport: 'SFO', aircraft: 'Airbus A380', distance: 20000, intercontinental: true, delayed: false }): replaces the entire document mathching the query
- If we insert many documents in passengers using insertMany
- find(): it gives back cursor object; we can use 'it' command in shell to see all the data as find does not show all data
- find does not return array of all docs in collection it gives only 20items as collection can be very big if it has 20M data then it will take very long time to get data & will send lot of data therefore it gives cursor object which is an object which have lot of meta data behind it that allows us to cycle through results
- toArray: db.passengers.find().toArrray() : it exhaust the cursor & fetch all docs from collection or we can use forEach as db
- passengers.find().forEach((passengerData)=>{printjson(passengerData)})
- forEach will fetch the next doc on every loop cycle so it is very efficient as it does not fetch all data in advance & load in memory instead it fetches data on demand not overusing our bandwidth & not loading too much in memory therefore pretty does not work on findOne as pretty method exists only on cursor
- Projection: If there is more data in database document but in application we only need few data of document, it is better to filter the data on mongodb server only by using Projection otherwise in application it will take more memory & bandwidth
- ![Projection](image-11.png)
- db.passengers.find({}, {name:1}).pretty(): projection of name is added it will return name & \_id , \_id is special field in data by default its always included, if we dont want \_id we have to explicilty mention it
- db.passengers.find({}, {name:1, \_id:0}).pretty() : will not return \_id in documents result
- Structured data : embedded docs, arrays
- Embedded documents or nested documents are those types of documents which contain a document inside another document. We can have up to 100 levels of nexting & max 16mb/document
- Arrays of embedded documents but arrays can hold ANY data, simply means list of data
- db.flightData.updateMany({}, {$set:{status:{description:"on-time", lastUpdated: "1 hour ago"}}}) : added nested data; now flightData will have embedded document
- db.flightData.updateMany({}, {$set:{status:{description:"on-time", lastUpdated: "1 hour ago", details:{responsible:"Reema"}}}}): more embedded doc now
- db.passengers.updateOne({name:'Reema'}, {$set:{hobbies:["sleeping", "eating", "cooking"]}}) : adding arrays
- Accessing Structured data
- db.passengers.findOne({name:'Reema'}).hobbies: to get array
- db.passengers.findOne({hobbies:'cooking'}): to get the document with specific array elt
- db.flightData.find({"status.details.responsible":"Reema"}): accessing embedded document
- ![Summary](image-10.png)
- https://docs.mongodb.com/ecosystem/drivers/
- https://docs.mongodb.com/manual/tutorial/getting-started/

## Schemas & Relations

# Reset DB

- use databaseName
- db.dropDatabase() : drop db
- db.myCollection.drop() : drop collection
- MongoDb enforeces no schemas! But that does not means that we can't use some kind of schema

# Data Types

- ![DataTypes](image-12.png)
- db companyData
- db.companies.insertOne({name:"Reema Inc", isStartup:true, employees:101, funding:1234568912345648789, details:{ceo:"Reeva"}, tags:[{title:'super'}, {title:'perfect'}], foundingDate:new Date(), insertedAt: new Timestamp()})
- db.stats(): give the detail of db
- db.numbers.insertOne({a: NumberInt(1)}): it takes less memeory in data size & more efficient
- typeof db.numbers.findOne().a : to check the data type
- MongoDb limits: https://docs.mongodb.com/manual/reference/limits
- https://docs.mongodb.com/manual/reference/bson-types/
- Normal integers (int32) can hold a maximum value of +-2,147,483,647
- Long integers (int64) can hold a maximum value of +-9,223,372,036,854,775,807
- Text can be as long as we want - the limit is the 16mb restriction for the overall document
- NumberInt creates a int32 value => NumberInt(55)
- NumberLong creates a int64 value => NumberLong(7489729384792)
- If we just use a number (e.g. insertOne({a: 1}), this will get added as a normal double into the database. The reason for this is that the shell is based on JS which only knows float/ double values and doesn't differ between integers and floats.
- NumberDecimal creates a high-precision double value => NumberDecimal("12.99") => This can be helpful for cases where we need (many) exact decimal places for calculations.
- https://mongodb.github.io/node-mongodb-native/3.1/api/Long.html
- db.collection('wealth').insert( {value: Long.fromString("121949898291")});

# Relations

- ![Options](image-13.png)
- ONE TO ONE RELATION (Embedded) : Patients <-> Disease Summary: 1 patient has 1 disease summary, a disease summary belongs to one patient
  - use hospital
  - db.patient.insertOne({name:'Reema', age:25, diseaseSummary:'summary-reema-1'})
  - db.diseaseSummaries.insertOne({\_id:'summary-reema-1', diseases: ['cold', 'pain']})
  - db.patient.findOne().diseaseSummary
  - var dsid=db.patient.findOne().diseaseSummary
  - db.diseaseSummaries.findOne({\_id:dsid}) : We can get the patient data like this, but its not optimal in 2 dbs, we can use embedded doc for this
  - db.patients.insertOne({name:'Reema', age:25, diseaseSummary:{ diseases: ['cold', 'pain']}}) : Embedded doc
- ONE TO ONE (References) : Person <-> Car : One person has one car, a car belongs to one person
  - If our application needs the average or some calculations on cars data then we don't need embedded doc of car & person, there we can have reference
  - use carData
  - db.persons.insertOne({name:'Reema', age:25, salary:100}) : generated \_id that is used in next query
  - db.cars.insertOne({model:'Ferarri', price:999999, owner: ObjectId('6661b386e319412802cdcdfd')})
- ONE TO MANY (Embedded) : Thread <-> Answers : One thread has many ans, one ans belong to one ques thread
  - use support
  - db.quesThreads.insertOne({creator:'Reema', ques:'How does that work', ans: [{ _id: 'q1a1', text: 'It work like that' },{ _id: 'q1a2', text: 'Thanks' }]})
- ONE TO MANY (References) : City <-> Citizens : One city has many citizens, one citizen belong to one city
  - If our app wants to fetch all the cities with their metadata but not all the data of citizens (if we use embedded for this then 16mb limit might be exhausted easily)
  - db.cities.insertOne({name:'Nalagarh', coordinates:{lat:25, lng:56}})
  - db.citizens.insertMany([{name:'Reema', cityId: ObjectId('6661c2e8e319412802cdce01')}, {name:'Reemaa', cityId: ObjectId('6661c2e8e319412802cdce01')}])
- MANY TO MANY (Embedded) : Customers <-> Products(Orders) : One customers has many products (via orders), a product belong to many customers
  - Often we model many to many relationships with reference
  - db.products.insertOne({title:'book', price:99})
  - db.customers.insertOne({name:'Reema', age:25})
  - db.customers.updateOne({}, {$set:{orders:[{title:'book', price:99}]}})
  - Disadv is data duplication, if ever the product data is changed then we have to update in product collection in orders collection also but if the order is placed then we will not be able to change the data for existing orders
- MANY TO MANY (References) : Books <-> Authors : One book has many authors, an author belongs to many books
  - For books & authors we need updated data of author in collection even if books were already ordered
  - db.authors.insertMany([{name:'reema', age:25,address:{street:'Chd'}}, {name:'ree', age:53, address:{street:'Chandigarh'}}])
  - db.books.insertOne({name:'My fav Book', authors:[ ObjectId('6667de60575f509448cdcdf7'), ObjectId('6667de60575f509448cdcdf8')]})
- ![alt text](image-14.png)

# lookup

- For merging reference relations
- A helpful tool that allows us to fetch 2 related docs merged together in one doc in one step instead of having to do 2 steps & this mitigates some of disadv of splitting our docs across collections
- ![alt text](image-15.png)
- db.books.aggregate([{$lookup:{from:'authors', localField:'authors', foreignField:'_id', as:'creators'}}])
- from collection name, local field is , in the connection we're running this on, where can the references to other collection to be found in, foreign field is which field are we relating to in our target collection, as is alias in which it is merged

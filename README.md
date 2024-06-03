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

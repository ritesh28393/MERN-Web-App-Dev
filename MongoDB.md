**Contents of the Mongo DB**
  1. [Introduction](#introduction)
  2. [Create Database](#create-database)
  3. [Create Collection](#create-collection)
  4. [Insert](#insert)
  5. [Find](#find)
  6. [Query](#query)
  7. [Sort](#sort)
  8. [Delete](#delete)
  9. [Drop](#drop)
  10. [Update](#update)
  11. [Limit](#limit)
  12. [Join](#join)
*** 
## Introduction
  * [Difference between SQL and NoSQL](https://www.thegeekstuff.com/2014/01/sql-vs-nosql-db)
## Create Database
  * MongoDB will create the database if it does not exist, and make a connection to it
  * >**Note:** In MongoDB, a database is not created until it gets content!. MongoDB waits until you have created a collection (table), with at least one document (record) before it actually creates the database (and collection).
  *
    ```js
    var MongoClient = require('mongodb').MongoClient;
    // connection URL with the ip address and the name of the database you want to create
    var url = "mongodb://localhost:27017/mydb";
    MongoClient.connect(url, function(err, db) {
      if (err) throw err;
      console.log("Database created!");
      db.close();
    });
    ```
## Create Collection
  * A collection in MongoDB is the same as a table in MySQL
  * >**Note:** In MongoDB, a collection is not created until it gets content!. MongoDB waits until you have inserted a document before it actually creates the collection.
  * `createCollection()`: To create a collection in MongoDB
    ```js
    var MongoClient = require('mongodb').MongoClient;
    var url = "mongodb://localhost:27017/";
    MongoClient.connect(url, function(err, db) {
      if (err) throw err;
      var dbo = db.db("mydb");
      dbo.createCollection("customers", function(err, res) {
        if (err) throw err;
        console.log("Collection created!");
        db.close();
      });
    });
    ```
## Insert 
  * A document in MongoDB is the same as a record in MySQL
  * >**Note:** If you try to insert documents in a collection that do not exist, MongoDB will create the collection automatically.
  * `insertOne()`: To insert a record/document into a collection
    ```js
    var MongoClient = require('mongodb').MongoClient;
    var url = "mongodb://localhost:27017/";
    MongoClient.connect(url, function(err, db) {
      if (err) throw err;
      var dbo = db.db("mydb");
      var myobj = { name: "Company Inc", address: "Highway 37" };
      dbo.collection("customers").insertOne(myobj, function(err, res) {  
        // result object contains information about how the insertion affected the database.
        if (err) throw err;
        console.log("1 document inserted");
        db.close();
      });
    });
    ```
  * `insertMany()`: To insert multiple documents into a collection in MongoDB. First parameter is the array of objects
  * **_id Field:** If you do not specify an _id field, then MongoDB will add one for you and assign a unique id for each document. If you do specify the _id field, the value must be unique for each document
## Find
  * `findOne()`:
    * Returns the first occurrence in the selection. 
    * The first parameter of the `findOne()` is a query object.
    * **Example:** Use an empty query object, which selects all documents in a collection (but returns only the first document)
      ```js
      dbo.collection("customers").findOne({}, function(err, result) {
        if (err) throw err;
        console.log(result.name);
        db.close();
      });
      ```
  * `find()`: returns all occurrences in the selection
  * **Projection**:
    * The second parameter of the find() method is the projection object that describes which fields to include in the result.
    * This parameter is optional, and if omitted, all fields will be included in the result.
    * You are not allowed to specify both 0 and 1 values in the same object (except if one of the fields is the _id field). If you specify a field with the value 0, all other fields get the value 1, and vice versa -
      ```js
      find({}, { projection: { _id: 0, name: 1, address: 1 } }).toArray(function(err, result) { }) // Excluding _id
      find({}, { projection: { address: 0 } }).toArray(function(err, result) { }) // Excluding address
      find({}, { projection: { name: 1, address: 0 } }) // MongoError: Projection cannot have a mix of inclusion and exclusion.
      ```
## Query
  * Used to filter the Result
  * The first argument of the find() method is a query object, and is used to limit the search
  * **Example:** Find documents with the address **Park Lane 38**
    ```js
    var query = { address: "Park Lane 38" };
    dbo.collection("customers").find(query).toArray(function(err, result) {
      if (err) throw err;
      console.log(result);
      db.close();
    });
  * **Filter With Regular Expressions:**
    * Regular expressions can only be used to query strings
    * To find only the documents where the "address" field starts with the letter **S**: `var query = { address: /^S/ };`
## Sort
  * `sort()`: use to sort the result in ascending or descending order. It takes one parameter, an object defining the sorting order.
  * **Example:** Sort the result alphabetically by name
    ```js
    var mysort = { name: 1 };
    dbo.collection("customers").find().sort(mysort).toArray(function(err, result) {
      if (err) throw err;
      console.log(result);
      db.close();
    });
  * **Sort Descending:** Use the value -1 in the sort object to sort descending
## Delete
  * `deleteOne()`
    * The first parameter is a query object defining which document to delete. 
    * If the query finds more than one document, only the first occurrence is deleted.
    * **Example:** Delete the document with the address **Mountain 21**
      ```js
      var myquery = { address: 'Mountain 21' };
      dbo.collection("customers").deleteOne(myquery, function(err, obj) {
        if (err) throw err;
        console.log("1 document deleted");
        db.close();
      });
      ```
  * `deleteMany()` - deletes all documents which matches the query.
    ```js
    var myquery = { address: /^O/ };  // Delete all documents were the address starts with the letter "O"
    dbo.collection("customers").deleteMany(myquery, function(err, obj) {
      if (err) throw err;
      console.log(obj.result.n + " document(s) deleted");
      db.close();
    });
## Drop
  * `drop()`
    * delete a collection(which is called table in MySQL)
    * It takes a callback function containing the error object and the result boolean parameter which returns true if the collection was dropped successfully, otherwise it returns false.
    * **Example:** Delete the **customers** table
      ```js
      dbo.collection("customers").drop(function(err, delOK) {
        if (err) throw err;
        if (delOK) console.log("Collection deleted");
        db.close();
      });
      ```
  * `dropCollection()` - Same as drop(), but the syntax is different. 
    ```js
    dbo.dropCollection("customers", function(err, delOK) {
      if (err) throw err;
      if (delOK) console.log("Collection deleted");
      db.close();
    });
    ```
## Update
  * `updateOne()`
    * The first parameter is a query object defining which document to update. 
    * The second parameter is an object defining the new values of the document.
    * If the query finds more than one record, only the first occurrence is updated.
    * **Example:** Update the document with the address **Valley 345** to name=**Mickey** and address=**Canyon 123**
      ```js
      var myquery = { address: "Valley 345" };
      var newvalues = { $set: {name: "Mickey", address: "Canyon 123" } };  // $set operator, only the specified fields are updated
      dbo.collection("customers").updateOne(myquery, newvalues, function(err, res) {
        if (err) throw err;
        console.log(res.result.nModified + " document(s) updated");
        db.close();
      });
      ```
  * `updateMany()` - To update all documents that meets the criteria of the query.
## Limit
  * Limit returns the subset of the result
  * It takes one parameter, a number defining how many documents to return
    ```js
    dbo.collection("customers").find().limit(5).toArray(function(err, result) {
      if (err) throw err;
      console.log(result);
      db.close();
    });
    ```
## Join
  * MongoDB is not a relational database, but you can perform a left outer join by using the `$lookup` stage.
  * The `$lookup` stage lets you specify which collection you want to join with the current collection, and which fields that should match.
  * **Example:** Join the matching **products** document(s) to the **orders** collection
    ```js
    dbo.collection('orders').aggregate([
      { $lookup:
        {
          from: 'products',
          localField: 'product_id',
          foreignField: '_id',
          as: 'orderdetails'
        }
      }
    ]).toArray(function(err, res) {
      if (err) throw err;
      console.log(JSON.stringify(res));  // Matching document(s) from the products collection is included as an array in the orders collection
      db.close();
    });
    ```

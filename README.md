# Introduction 
This is a repo created for storing any useful information about MongoDB.

<details>
<summary>Section - 1: Introduction</summary>

* MongoDB Data structure:  

![mongoDB](Section-1/intro-structure.jpg)

* MongoDB data format (document-oriented storage format):

![data-format](Section-1/2-data-format.jpg)

* BSON data-format and what is under the hood:

![BSON](Section-1/3-no-schema.jpg)

* MongoDB Ecosystem:

![Ecosystem](Section-1/4-ecosystem.jpg)

* Work with MongoDB:

![mongodb](Section-1/9-work-with-mongo.jpg)
![mongodb2](Section-1/10-inside.jpg)

* Implicit operations in Mongo:

![console output](Section-2/1-implicit.jpg)

## Start working with MongoDB

To add mongo command to your command line:  
<b> win - environment variables - advanced tab - environment variables</b>  
Add here a path to your mongoDB.
![image how to do that](Section-1/5-cmd-configuration.jpg)

[useful link](https://dangphongvanthanh.wordpress.com/2017/06/12/add-mongos-bin-folder-to-the-path-environment-variable/)

<b>BTW</b>, to continue working with course you have to stop MongoDB service and start db manually
using "mongo" command from console. Without it "mongo" command will open mongo service instead of real db.

to stop service - open CMD as admin and `net stop Mongo`

<b>Last step:</b>  
* To make default data storage location: create "data" folder in C: drive and put folder "db" within.
* Otherwise: create another directory (i.e. D:\mongodb-data\db) and put command in cmd:
`mongod --dbpath D:\mongodb-data\db`

<b>Pay attention:</b>
You have to leave your process running (cmd console should be opened) to work with mongoDB service.

* CMD - `mongo`

And now you are in the mongo shell where you can run your commands and queries.
</details>

<details>
<summary>Section - 1: Simple commands</summary>

* `show dbs` - will show existing dbs in selected repository (`--dbpath D:\mongodb-data\db`)
* `use Your_db_name` - will switch to db with selected name. If db does not exist - it will create it automatically.
* `db.products.insertOne({name: "A Book", price: 29.99})` - will create a table named products (it does not exist too) 
in db which we connected to and insert a document inside it.  
Pay attention on non-existing quotation mark in "keys" - you can use key naming without quotations, they will be added
under the hood.  

Here is a console output. InsertedId - generated uniqueId for this insert, acknowledged - verified that this data was inserted.
![console output](Section-1/7-console-output-after-insert.jpg)

* `db.products.find()` - retrieves you all data from collection (from table in SQL world).
* `db.products.find().pretty()` - show this data formatted.
![pretty](Section-1/8-find-pretty.jpg)
</details>

<details>
<summary>Section - 2: JSON-vs-BSON, Basics & CRUD operations</summary>

### Summary
![summary](Section-2/24-Summary.jpg)

### Json vs Bson:
![json-vs-bson](Section-2/2-json-vs-bson.jpg)

* You can set _id field manually and do not rely on autogenerated id.
BTW, you can't insert another document with the same _id.

![id](Section-2/3-_id-field.jpg)

## CRUD Operations:

![crud](Section-2/4-crud-1.jpg)
![crud](Section-2/5-crud-2.jpg)

### Read 
Simple filter: `db.flight.find({intercontinental: true}).pretty()`;
Greater than ($gt): `db.flight.find({distance: {$gt: 10000}}).pretty()`;
FindOne: `db.flight.findOne({distance: {$gt: 10000}})`

### InsertMany and Show results using find. Cursor  
Find does not show you all results, it shows you a cursor by default:  
![cursor](Section-2/11-insert-many.jpg)
![cursor](Section-2/12-find-cursor.jpg)

* Bare in mind that mongodb will increment Id to keep the proper element's order. First came element will contain minor identifier:

![crud](Section-2/6-insert-many.jpg)

### UpdateOne
`db.flight.updateOne({distance: 12000}, {$set: {marker: "new field delete"}})` - will update first document which contains distance: 12000.  
Pay attention on <b>$set</b> - all reserved words start from dollar sign. 
This operator means that you would like to update your document with new field.

### UpdateMany
`db.flight.updateMany({}, {$set: {marker: "to Delete!"}})` - empty curly braces `{}` mean all documents in collection.

### Update
`update` operation works like `updateMany`:  
![crud](Section-2/7-update.jpg)

As you may have noticed first modification using set to `delayed: true` has no modified results because our document already
has this value. When we changed the value to false - log shows us that our value has been changed.

The difference between them - you can use it without `$set` operator, update does accept this syntax.
But it works on another manner:
![crud](Section-2/8-update-2.jpg)

It will override all key-value pairs in document!
![crud](Section-2/9-update-3.jpg)

It works very close to `replaceOne`:
![crud](Section-2/10-replaceone.jpg)

### Delete
`db.flight.deleteOne({departureAirport: "TXL"})` - departureAirport: "TXL" will be used as filter to find what exactly
 we want to delete from collection. Only first found document with "TXL" will be deleted.

### forEach
It is possible to use .forEach operation after find() to do something with every element after filtering:
`db.passengers.find().forEach((passengerData) => {printjson(passengerData)})` - bare in mind forEach uses syntax according
your MongoDB driver. Shell uses Nodejs syntax.

Pay attention:
That's why you cant use `pretty()` after findOne() method - `pretty()` is a method of a Cursor, findOne does not return cursor,
(and `pretty()` does not exist for a single value), findOne returns a sole value.

</details>

<details>
<summary>Section - 2: Projection</summary>

### Projection
Projection means a mechanism to avoid overfetching from database.  
You can use it as a parameter in `find` method:
* `find({}, {name: 1})` - first parameter is a filter, the second is projection. 1 means - "include this data".

![projection](Section-2/13-projection-overfetching.jpg)
![projection](Section-2/14-projection-2.jpg)

By default it will send you objects with _id (because it is a default property) and "name".

* To exclude _id (or any other field) - `find({}, {_id: 0})` - 0 means exclude.
![projection](Section-2/15-projection-3.jpg)

To only name - `find({}, {name: 1, _id: 0})`/

</details>

<details>
<summary>Section - 2: Embedded Documents & Arrays</summary>

![embedded](Section-2/16-Embedded-doc.jpg)
![embedded](Section-2/17-Embedded-array.jpg)

## Array examples:
![arrays](Section-2/18-embedded-doc-example.jpg)
![arrays](Section-2/19-embedded-doc-example-2.jpg)
  
## Simple arrays with find method
![arrays](Section-2/20-arrays-of-string.jpg)
![arrays](Section-2/21-arrays-filter-by.jpg)

## Array of objects with find method
to use find in embedded document you have to use ".":
`find({"status.description": "your_value"})`

description is an embedded document inside status.  
Pay attention that you must use double quotation around `status.description`.

![arrays](Section-2/22-arrays-filter-by.jpg)
![arrays](Section-2/23-arrays-filter-by.jpg)

</details>

<details>
<summary>Section - 3: Schema Basis</summary>

![schema](Section-3/1-schema.jpg)
![schema](Section-3/2-schemaless-to-sqlworld.jpg)
![schema](Section-3/3-schemaless-to-sqlworld-2.jpg)

* SQL Approach (the same structure for all documents):  
You can assign null to your property. The value of such property will not be assign, but the property will be shown
in your data structure.
`db.products.insertOne({name: "Book", details: null})`

</details>

<details>
<summary>Section - 3: Data Schemas, Data Modelling</summary>

![data-modelling](Section-3/6-data-modelling.jpg)
</details>

<details>
<summary>Section - 3: Data Types, Limits and Statistic</summary>

[good link about how mongodb works inside](https://www.datadoghq.com/blog/monitoring-mongodb-performance-metrics-mmap/)

* Data Types:
![types](Section-3/4-Value-types.jpg)

* to get statistic from your database you have to use `stats()` command;
![stats](Section-3/5-stats.jpg)
To prove that it stores a number instead of float you can use `typeof db.numbers.findOne().a` command.

* MongoDB has a couple of hard limits - most importantly, a single document in a collection (including all embedded documents it might have) must be <= 16mb. Additionally, you may only have 100 levels of embedded documents.
[additional-info](https://docs.mongodb.com/manual/reference/bson-types/)

1. NumberDecimal creates a high-precision double value => NumberDecimal("12.99")
2. NumberInt creates a int32 value => NumberInt(55)
3. NumberLong creates a int64 value => NumberLong(7489729384792)

</details>

<details>
<summary>Section - 3: Relations</summary>

##One to One Relations
![relations](Section-3/7-relations-1.jpg)
![onetoone](Section-3/one-to-one/8-relations-one-to-one-1.jpg)
![onetoone](Section-3/one-to-one/9-relations-one-to-one-2.jpg)

* Example with one-to-one relations and call the data using two steps and variable:

![onetoone](Section-3/one-to-one/10-relations-one-to-one-3.jpg)

It's not the best option of storing data. In such case better to store data like embedded data inside patient document.
In most cases better to use embedded approach. 

* Another one-to-one examples, but using references. You still opt to use different collections: 
It could be possible useful if you try to analyze your data. And it's very good if your data stores in different
collections (for load balancing, for example. Or because we are interesting only in cars).

![onetoone](Section-3/one-to-one/11-relations-one-to-one-reference-4.jpg)
![onetoone](Section-3/one-to-one/12-relations-one-to-one-reference-5.jpg)

##One to Many Relations
![onetomany](Section-3/one-to-many/1-one-to-many-schema-1.jpg)
* And brief example of ref and embedded approaches:

![onetomany](Section-3/one-to-many/2-one-to-many-approaches.jpg)
* Additional example:

![onetomany](Section-3/one-to-many/3-additional-example.jpg)

##Many to Many Relations
![manytomany](Section-3/many-to-many/1-collection-relations.jpg)
* Sql World approach with 3 tables, one of them stores a joint data:

![manytomany](Section-3/many-to-many/2-sql-world-approach.jpg)

* MongoDB Approach:

![manytomany](Section-3/many-to-many/3-mongo-db-approach.jpg)

It allows us to use references within one of the data tables.
Advantages from sql and mongo worlds.
Also no reason to use fully embedded approach for some reasons (over-fetching, possible not up-to-date data and so on).

* Summary:
![summary](Section-3/many-to-many/4-summary.jpg)

## Merging And Joining with $lookup()
![aggregate](Section-3/8-merge-aggregate-lookup.jpg)
* Initial doc is:

![aggregate](Section-3/9-merge-aggregate-lookup2.jpg)
* And the result of aggregate + lookup operator:

![aggregate](Section-3/10-merge-aggregate-lookup3.jpg)

</details>

<details>
<summary>Section - 3: Schema Validation</summary>

![validation](Section-3/validation/1-validation-schema.jpg)
![validation](Section-3/validation/2-levels-and-actions.jpg)

* To declare validation for new collection you have to use explicit collection creation using `createCollection` method.
first  parameter is a new collection name.  
second parameter is its structure: validator + $jsonSchema = validate the schema.
1. Right now $jsonSchema is strongly recommended approach.
2. bsonType: "object" - every coming element should be object-like.
3. required: [] - array of required fields.
4. description - error message.
5. items - you can define nested elements validation.
6. validationAction: 'warn' - only warns you about errors in validation, but not blocks you to send a new document.
<pre>
db.createCollection("newNameOfCollection", {
validator: {
    $jsonSchema: {bsonType: "object", required: ["title", "text", "creator", "comments"], 
    properties: {
     title: { bsonTYpe: "string", description: "must be a string and is required" },
     text: { bsonType: "string", description: "must be a string and is required" },
     creator: { bsonTYpe: "objectId", description: "must be an object and is required" },
     comments: { 
        bsonTYpe: "array",
        description: "must be an array!"
        items: { 
            bsonType: "object",
            required: ["text", "authors"] 
            properties: {
                 text: {
                     bsonType: "string",
                     description: "text must be a string!!"
                 },
                 author: {
                     bsonType: "objectId",
                     description: "author must be an objectId"
                 }
               }
            }
        }
    }
}})
</pre>

* To add\modify validation to already existed collection you have to:
<pre>
db.runCommand({
    collMod: "posts",
    validator: { ...YOUR_VALIDATION_STRUCTURE_AS WE DID BEFORE }
    validationAction: 'warn'
    })
</pre>

this code will succeed. You could see a warning in the log file.(next lecture).

</details>

<details>
<summary>Section 4: MongoDB Settings (As Process and Service), Configuring DB, Log Path</summary>

#### mongod parameters

`--directoryperdb` - each db will be stored in a separate directory (under defined by --dbpath)
 Instead of collection of files - collection of nested folders.

* Linux:
`--fork` - fork process. Works only for Linux. 
[mongodb start vs mongod --fork](https://stackoverflow.com/questions/21329618/whats-the-difference-between-service-mongodb-start-and-mongod/48459859)
to kill MongoDB service process: `use admin` to switch to admin database. And `db.shutdownServer()`;

* Windows:
To run background MongoDB as background service: `net start MongoDB`.  
This command provides you ability to run Mongo as background service.
to kill MongoDB service process: `net stop MongoDB`.

### Save your configurations into configuration file and use it
[configuration file example](Section-4/mongod-configuration-example.cfg)  
To use config file with mongod:
`mongod --config C:/mongod-configuration-example.cfg`
or
`mongod --f C:/mongod-configuration-example.cfg`

It allows you to make a snapshot or blueprint of your mongod configurations.  
Another useful information could be found at mongodb documentation.

### Help
to use help just type help:  
`help admin` - administrative help  
`help connect` - connecting to a db help  
`help keys` - key shortcut
`help misc` - misc things to know
`help mr` - mapreduce.

</details>

<details>
<summary>Section 5: Using the MongoDB Compass to Explore Data Visually</summary>

The MongoDB Compass Docs:   
[https://docs.mongodb.com/compass/master/install/](https://docs.mongodb.com/compass/master/install/)  
Full free version of CompassDB is available free for community.

You can use compass to create databases, collections and documents.
![example](Section-5/1-compass-collection-creation-insert-document.jpg)
Additional features:
![features in compass](Section-5/2-tabs.jpg)

</details>

<details>
<summary>Section 6: CREATE operation, importing documents, additional information</summary>

Available methods:
![methods](Section-6/1-available-methods.jpg)

### Insert
* insert method also works, but not recommended.

* For example after using `db.persons.insert({name: "Phil", age: 25})` you do now await that this new person has an Id, but it has.  
Unlike the insertOne method insert does not show you its new "_id". It could be a real disadvantage, because in real create
operations you want to get an Id of just created object and then - immediately use it in your app (It's an extremely helpful in most cases).

The same story vs insertMany. Its output is not very helpful at all:
![insertmany](Section-6/2-insert.jpg)

### InsertMany
If you use insertMany and send elements with non-unique declared "_id" field - it will raise an error, 
but all elements until <b>first</b> error will be successfully added.  
 
* For example, this one will fail after trying to add "cooking" again:
![insertmany](Section-6/3-insert-many.jpg)
![insertmany](Section-6/4-insert-many.jpg)
* It does not rollback elements which succeeded in inserting.

* To change this behavior you have to use second argument in insertMany method, <b>ordered</b>:  
ordered option will specify how your insert works. If you set it to `ordered: false` all your elements except failed will be inserted.  
In other words, it will continue inserting after fail.
![insertmany-ordered-false](Section-6/5-insertmany-ordered-false.jpg)

* Tip: To Rollback your insert entirely you have to use transactions from transaction module.

### WriteConcern
![writeconcern](Section-6/6-w-j-parameters.jpg)

* What w:0 allows you - it allows you to get immediate response without waiting real data adding to any instance.
The response will be "acknowledge: false" - which means "we are not sure does your request reach the server or not".  
It is super fast, but obviously it does not let you know anything about operations.
![writeconcern](Section-6/7-acknowledge-false.jpg)

* Why storage engine does not store document on the disk first?
because this operation could be quite heavy (take care about indexes, for example), better to store the info into the 
memory first and after that - set it to the disk using "journal ("TODO")".

* Timeout option allows you control create operation time in situations with bad network connection, for example.
in milliseconds.

* Journal parameter (undefined or false is a default parameter):
![journal](Section-6/8-journal.jpg)

### Atomicity
![atomic](Section-6/9-atomic.jpg)

# Importing Data
to import data from json file you have to use mongoimport. This command is available globally (not from mongo terminal mode).
`-d` -database
`-c` -collection name (could be implicitly created)
`--jsonArray` - let your command to know that you send multiple objects, not only one
`--drop` - drop collection before adding (if the collection exist and not empty)
![import](Section-6/10-import-tool.jpg)

</details>

<details>
<summary>Section 7: READ Operations</summary>

## Structure:
![method-filter-operator](Section-7/0-method-filter-operator.jpg)
### Operators:
![operators](Section-7/1-operators.jpg)
![operators](Section-7/2-operator-examples.jpg)

#### Comparision Operators:
1. Examples how to work with top-level properties:
`db.movies.find({runtime: {$eq: 60}})` == `db.movies.find({runtime: 60})`
It's also possible to use not equal operator using `$ne`.
Others could be found in the official documentation.

2. How to work with non-top level properties (because we possibly have lots of embedded fields):
`db.movies.find({"rating.average": {$lt: 60}})` - average is lower than 60.

**Hint 1**: Is you have some genres for your movies in the array, and you try to find "Drama" movies with
`db.movies.find({genres: "Drama"}).pretty()` it also returns you movies with array of genres where "Drama" is included in.
![array-of-elements-and-filtering](Section-7/3-genres-array-and-filter-operator.jpg)

If you want to find exact films with only "Drama" in array you have to use next operator:
`db.movies.find({genres: ["Drama"]}).pretty()` - will return you only Drama in an array.

**Hint 2**: Pay attention on capital "D" in "Drama" equality. If you try to find any movie with "drama" genre - it will not return you anything.
Case sensitive searching.

#### Logical Operators (nor, or, not, and):
* OR (which means composition of 2 operators):  
`db.movies.find({"rating.average": {$or: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]}})` - returns all movies where average rating
is lower than 5 or greater than 9.3.

* NOR very similar with OR:
`db.movies.find({"rating.average": {$nor: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]}})` - returns you all movies
Where all conditions do not work (not higher than 9.3 and not lower than 5). Simply say it's the inverse of our previous check.

* AND:
`db.movies.find({"rating.average": {$and: [{genres: "Drama"}, {"rating.average": {$gt: 9.3}}]}})`

The alternative of that is:
`db.movies.find({"rating.average": {$gt: 9}, genres: "Drama"})` - it works the same because by default MongoDB has the concatenation mechanism.  
But what the point of having 2 different ways to get the same results?  
**Here the answer**:  
`db.movies.find({genres: "Horror", genres: "Drama"})` - works in command prompt, but prohibited in Javascript because you can't declare 2 object keys
with the same name.  
`db.movies.find({$and: [{genres: "Horror"}, {genres: "Drama"}]})` - but this one will work like a charm.  

**But pay attention.**  
`db.movies.find({genres: "Horror", genres: "Drama"})` option will return you results with movies which have
single genres "Drama" or "Horror". Why is that? Because it replaces previously declared "genres" with new value "Drama" (which was declared the last):  
How to check that?  
`db.movies.find({genres: "Horror", genres: "Drama"}).count()` - 23 elements.  
`db.movies.find({genres: "Drama"}).count()` - 23 elements.  
 
**Conclusion**: if you need to use and with one field - you have to use `$and` syntax.

* NOT:
Inverts the result of your filter:  
`db.movies.find({"runtime": {$not: {$eq: 60}})` - not equal to 60.
`db.movies.find({"runtime": {$ne: 60}})` - not equal to 60 too.

#### Element Operators:
Allows you to work with the data of different types - for example when phone number has integer type and string type.
Also, it allows you to check does this property exist and so on:

##### $exist:
`db.users.find({age: {$exists: true}}).pretty()` - shows you which documents have declared age field.  
Pay attention it will return you documents with defined age with "null" value as well.  
To avoid that let's do next:
`db.users.find({age: {$exists: true, $ne: null}}).pretty()` - will return you documents with defined age which is not null.

###### $type:
`db.users.find({phone: {$type: "double"}}).pretty()`
`db.users.find({phone: {$type: ["double", "string"]}}).pretty()` - works as well if you would like to check on multiple types.

###### $regex allow you to search with text (But be aware it has no super performance, especially with big texts, better to use indexes):  
`db.movies.find({summary: {$regex: /musical/}})` - for example it will look for "non-full equality".
But again, it's not the best way of doing that.  
Example:
1. Data:  
`{
               "_id" : ObjectId("5f0f2d0c461a206f6b3ab0a8"),
               "volume" : 100,
               "target" : 120
}
{
               "_id" : ObjectId("5f0f2d0c461a206f6b3ab0a9"),
               "volume" : 89,
               "target" : 80
}
{
               "_id" : ObjectId("5f0f2d0c461a206f6b3ab0aa"),
               "volume" : 200,
               "target" : 177
}`

###### $expr:
2. Find all elements where volume is higher than target:
`db.sales.find({$expr: {$gt: ["$volume", "$target"]} })`  
* $expr - expression
* "$volume" and "$target" - name of the fields 

2.1 It also could be more complex. For example find elements where volume is also higher than additional const value + target is higher than some const value:
`> db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 30]}, else: "$volume"}}, "$target" ]}})`
* $cond is related to expression $expr. Condition `$cond` allows you to describe complex conditions.
this condition must be `$gt` greater than `"$target"`.
* $subtract - subtract one value from another
* if then else condition
Result:
`{ "_id" : ObjectId("5f0f2d0c461a206f6b3ab0a9"), "volume" : 89, "target" : 80 }` - because only this documents suits to our condition

if we change our subtract from 30 to 10:
`> db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 10]}, else: "$volume"}}, "$target" ]}})`  
Result will be:  
`{ "_id" : ObjectId("5f0f2d0c461a206f6b3ab0a9"), "volume" : 89, "target" : 80 }
 { "_id" : ObjectId("5f0f2d0c461a206f6b3ab0aa"), "volume" : 200, "target" : 177 }` - two values instead of one in the result set.

##### Querying Arrays ($size, $all, $elemMatch):
to find something through complex objects in arrays, for example nested object  
`hobbies: [{title: "Sports", frequency: 3}, {title: "Cooking", frequency: 2}]`
Answer:  
`db.users.find({"hobbies.title": "Sports})` - you can use this operator for nested arrays. Will query all documents which include Sport in hobbies array.

* $size, $all  
Find all movies with genres action and thriller AND size or genre array should be 2. Will query only movies with exact genres.  
`db.movies.find({$and: [{genre: {$all: ["action", "thriller"]}}, {genre: {$size: 2}}]}).pretty()`

* $elemMatch  
Find all users who have hobby "Sports" with frequency 2. Data:  
`hobbies: [{title: "Sports", frequency: 3}, {title: "Cooking", frequency: 2}]`
Answer:  
`db.users.find({$and: [{"hobbies.title": "Sports"}, {"hobbies.frequency": 2}]})` - will not work. It will find all documents with independent values. For example if someone has any hobby with frequency 2 + Sports with frequency 3.  
Here we basically can use $elemMatch:  
`db.users.find({"hobbies": {$elemMatch: {title: "Sports", frequency: 2}}})` - will work.  
**Pay attention on "title" - i use it without writing parent node "hobbies.title" because it returns you the nested document**
 
* Additional example: to find all movies where rating array contains only values between 8 and 10 (using $all and $elemMatch):  
Data Structure:  
`{
         "_id" : ObjectId("5f0f3ddeb1feccd1e8f78a67"),
         "title" : "Supercharged Teaching",
         "meta" : {
                 "rating" : 9.3,
                 "aired" : 2016,
                 "runtime" : 60
         },
         "visitors" : 370000,
         "expectedVisitors" : 1000000,
         "genre" : [
                 "thriller",
                 "action"
         ],
         "ratings" : [
                 10,
                 9,
                 9
         ]
}`  
Answer:   
`> db.movies.find({ratings: {$all: [{$elemMatch: {$gt: 8}}, {$elemMatch: {$lt: 10}} ]}}).pretty()`

</details>

<details>
<summary>Section 7: Cursors behind the scene</summary>

![cursors](Section-7/4-Cursors.jpg)

#### Cursors. Base understandings and additional functions.

* `count()` - using count with a cursor you can find out how much documents you can get.
`db.movies.find().count()`
BTW, when we make such call - we make a call to memory, not to data which comes from file. It happens because after his inner command "run"
it place data into memory.

* `find()` - when you make a call with find() from console you can type "it" to get next batch of documents. The MongoDb driver has another command and mechanism to
make the same.

* `next()` - this method works in console and directly with mongoDb driver.  
`db.movies.find().next()`

1. If you call `next()` several times - it will return you the same element? Why? because it executes it from scratch.  
If you store somewhere your dataCursor and make the `next()` call from it - it will return you next element every time.

Example:  
`const dataCursor = db.movies.find() dataCursor.next()`
if you call dataCursor - it will return you 20 documents again.  

2. `forEach` - another operation which could be used with cursor.  
Example:  
`dataCursor.forEach(doc => {printjson(doc)})`. printjson - method provided by driver which prints your document on the screen.
**Pay attention. You will no longer able to use "it" to see more. After using forEach with the cursor it will return you all remain documents in db (obviously after prev next() calls)**

3. if you make a call to `next()` after `forEach` - it will show you an error because the cursor will be exhausted.  
You can check it using `hasNext()` function with cursor: `dataCursor.hasNext()`, With hasNext you can safety use `next()`.

#### Cursors. Sorting results.
You can sort alphabetically, or by number. You have to call it after `find()` and before `pretty()`.
`db.movies.find().sort({"rating.average": 1}).pretty()`  
1 - means ascending  
-1 - descending. Returns doc with the highest average rating first.

You can use several sorting fields:  
`db.movies.find().sort({"rating.average": 1, runtime: -1}).pretty()` - by average rating, then by runtime.

* Tip: For obvious reasons sort is available only for cursors after find, it is not available after findOne().

#### Cursors. Skipping and Limiting.
* `skip()`. You can use skipping for pagination on your website, for example.  
`db.movies.find().sort({"rating.average": 1, runtime: -1}).skip(10).pretty()` -- skip 10 documents.
We are also able to skip much higher than 20 (default cursor documents amount). It is possible to `skip(100)` for example.

* `limit()`. Limit allows you to limit amount of documents retrieving by the cursor.  
Interesting detail.
`db.movies.find().sort({"rating.average": 1, runtime: -1}).skip(10).limit(10).count()` - returns you total amount of documents in the collection.  
But if you try to see results by `db.movies.find().sort({"rating.average": 1, runtime: -1}).skip(10).limit(10).pretty()` - it will return you only 10 elements.
This method is also very useful for pagination.

**Pay attention. Orders of the methods matter! It matters when you use directly with a driver. But using with a cursor it doesn't matter, cursor place it in the right order for you under the hood!**

#### Cursors. Projection.
##### Projection for objects and embedded documents:  
* Using projection you can control which data returned. What does it mean? It means how we extract the data fields.  
To do that you have to pass second argument to `find`. First one is responsible for the filter criteria.  
`db.movies.find({}, {name: 1, genres: 1, runtime: 1, rating: 1, image: 0}).pretty()` -- all fields which are not mentioned here (or if they were mentioned explicitly with 0) - are not included.  
* There are one exception - _id field. It is included if you don't specify it with 1. You can exclude it  set to 0 explicitly:  
 `db.movies.find({}, {name: 1, genres: 1, runtime: 1, rating: 1, image: 0, _id: 0}).pretty()`
* you could also project for embedded documents:  
 `db.movies.find({}, {"schedule.time": 1}).pretty()` - will return you a schedule embedded document only with mentioned field. Other fields will not be included.

##### Projection for arrays:

1. `db.movies.find({"genres": "Drama"}, {"genres.$": 1})`  - This query means that we filtering every movie by genres which include "Drama" (genres is an array). But after that we tell mongo to fetch only first genre from the array.  
![cursors](Section-7/5-projection-for-arrays.jpg)
Another example: 
![cursors](Section-7/6-projection-for-arrays-2.jpg)
It looks a bit strange. Let me explain how it works. Technically "Horror" is a first matching element. Drama is lower. That's why after projection we see only him.

2. `$elemMatch`. Sometimes your want to pull some items which are not you queried for. In such situation you can use $elemmatch. It also available for you in projection.  
`db.movies.find({"genres": "Drama"}, {genres: {$elemMatch: {$eq: "Horror"}}})` 
![cursors](Section-7/7-projection-for-arrays-3.jpg)
You see empty field because "Horror" did not simply include into them.

##### Projection. $slice for arrays.
`$slice` allows you to pull specified amount of elements from array.
`db.movies.find({"genres": "Drama"}, {genres: {$slice: 2}, name: 1}})` - this example will return you series with name, id(added by default) and 2 genres.  
With slice you can use array form:
`db.movies.find({"genres": "Drama"}, {genres: {$slice: [1, 2], name: 1}})` - first parameter is amount of elements what you would like to skip. The second one - is the amount of element to pull.

</details>

<details>
<summary>Section 9: Delete Operation</summary>

1. `deleteOne` operator let you delete one with query selector:  
`db.persons.deleteOne({name: 'Chris'})`

2. `deleteMany` let you delete several records match your condition:  
`db.persons.deleteMany({age: {$gt: 30}, isSporty: true}})`
`db.persons.deleteMany({age: {$exist: false}, isSporty: true}})`

</details>

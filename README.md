# mongodb_notes

### Installing MongoDB
From terminal, enter the following commands :
* `brew install mongodb` - Install MongoDB using Homebrew.
* `mkdir -p /data/db` - Create a folder for your databases to go.
* `sudo chown -R `id -un` /data/db` - Gives write permissions to your new folder.

Now, to check and make sure everything is working OK, fire up MONGO:
* `mongod` in One Terminal
* `mongo` in another Terminal

The result, is that you should see a `>`

Control + C to close out.

### Robomongo
Robomongo is the visual tool we use to look at our Mongo Databases. Similar to Postico for SQL.

Download Robomongo here: https://robomongo.org/download

### What is Mongo DB
Open Source, NoSQL - Databases that generally not relational and don’t have a query language like SQL. 
Document Orientated - Great for unstructured data, especially when you have lots of it.

| SQL        |            | MongoDB  |
| ------------- |:-------------:| -----:|
| database      | -> | database |
| table     | ->     |   collection |
| row | ->      |    document |

The main difference? SQL is relational and MongoDB is document-oriented. 

Relational database management systems save data in rows within tables. MongoDB saves data as documents within collections. 

### Fruit Table

| potion_id        | name           | price  | vendor_id |
| ------------- |:-------------:| -----:| -----:|
| 1      | Apple | 1.49 | 2 |
| 2     | Peach     |   1.89 | 1 |
| 3 | Watermelon     |    3.99 | 1 |


### Vendors Table

| vendor_id        |    name        | 
| ------------- |:-------------:| 
| 1      | Welcome Basket | 
| 2     | Big Bowl     |   

All data is grouped within documents:
```javascript
{
	name: “Apple”,
	price: 1.49,
	vendor: “Welcome Basket”
}
```

Collections are simply groups of documents. Since documents exist independently, they can have different fields. 
Potions can have different key/value pairs but exist within the same collection:
```javascript
{
   name: “Apple”,
   price: 1.49,
   vendor: “Welcome Basket”
}
{
   name: “Peach”,
   price: 1.89
}
{
   name: “Watermelon”,
   price: 3.99,
   taste: “Great”
}
```

### Dynamic Schema: 
> Means that documents have different data but exist in the same collection.

### Where can write Mongo?
* In the ‘shell’
* To get there, after we install mongo, go to terminal and type ‘mongo’, this will bring you into the mongo shell.
* Commands go after the ">".

All installations of Mongo come with a command line program we can use to interact with our database using Javascript.

### We can write regular Javascript code. Example:
```javascript
> var fruit = {
	“name”: “Apple”,
	“vendor”: “Welcome Basket”
}
```
```javascript
> potion
console:
{
	“name”: “Apple”,
	“vendor”: “Welcome Basket”
}
```
> “Documents are just JSON-like Objects”

Some Shell commands:
* use reviews -> Will switch to a database called reviews (which does not exist yet! But will once we save to it!)
* db -> Shows us which DB we are using. Returns the current database name.
* show dbs -> Shows a list of all databases. Shows specifically name and size of DB.

'Documents need to be stored in Collections', or 'Documents are always stored in collections within a database.'
```javascript
> db.fruits.insert(
	{
		“name”: “Apple”,
		“vendor”: “Welcome Basket”
	{
)
```
Whats cool here is that with the ‘insert’ collection method, we save the document into a ‘potions’ collection which is dynamically created. Meaning, this collection does not exist yet, but it will be automagically created. 

We get back a console log:
```javascript
console:
WriteResult({ “nInserted” : 1 })
```

Whenever we write to the database, we’ll always be returned a WriteResult object that tells us if the operation was successful or not.

We can use the ‘find()’ collection method to retrieve the potion from the inventory collection.
```javascript
> db.fruits.find()
```

All collection methods must end with parentheses. 
In this case, find will return our one entry, but include a “_id”.
Mongo requires that a unique ID gets written to each document entry, so it automatically creates one.

```javascript
db.fruits.find({“name”: “Apple” });
```

This is a query of equality. Checks the database for a entry with a “name” (key) of “Apple” (value).
Returns the entire document.
Returns all documents with that value.
So if we looked for all documents of a particular vendor, it would give them all to us.

Documents are persisted in a format called “BSON” which is like JSON.
It can store:
* Strings
* Numbers
* Boolean
* Arrays
* Objects
* Null

BSON has some extras:
* ObjectID
* Date - ISODate

To learn more about BSON, head to:
http://go.codeschool.com/bson-spec

MongoDB will preserve the precision of both floats and integers.

Dates, Javascript uses “0” as the first month. So subtract 1 off of the normal month date.
To create a date:
> “tryDate”: new Date(2012, 8, 13) -> is “September 13th, 2012”

Dates get converted to an ISO format when saved to the database:
> “expiration”: ISODate(“2012-09-13T04:00:00Z”)

Can store any data type inside of an array:
> “colors”: [“green”, "red", “dark red”];

We embed documents simply by adding the document as a value for a given field:
```javascript
{
	…
	“ratings”: { “strength”: 2, “flavor”: 5 }
}
```
```javascript
db.fruits.find({ “colors”: “red” });
{
	“colors”: [“green”, “red”, “dark red”]
}
```

When performing a find on an array item, it will look through the entire array to find a value in there that matches to the find.

For searching for equality in an embedded document, we use dot notation.

```javascript
> db.fruit.find( { “ratings.flavor” : 5 } )
```

## Removing and Modifying Documents

Removing a document with a query of equality:
db.fruits.remove({“name”: “Apple”})

If our query matches multiple documents, then remove() will delete them all:
db.portions.remove({“vendor”: “Welcome Basket”})

We can use the update() collection method to modify existing documents:
db.fruits.update({“name”: “Apple”}, {“$set”: {“price” : 3.99} } )

UPDATE ONLY MATCHES TO THE FIRST MATCHING DOCUMENT!

Update operators always start with a “$” sign, like in $set. 

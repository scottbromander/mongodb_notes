# mongodb_notes

What is Mongo DB
Open Source, NoSQL - Databases that generally not relational and don’t have a query language like SQL. 
Document Orientated - Great for unstructured data, especially when you have lots of it.
10gen - Developed it back in 2007, 2013 Company renamed to MongoDB

| SQL        |            | MongoDB  |
| ------------- |:-------------:| -----:|
| database      | -> | database |
| table     | ->     |   collection |
| row | ->      |    document |

The main difference? SQL is relational and MongoDB is document-oriented. 

Relational database management systems save data in rows within tables. MongoDB saves data as documents within collections. 

Potion Table

| potion_id        | name           | price  | vendor_id |
| ------------- |:-------------:| -----:| -----:|
| 1      | Love | 3.99 | 2 |
| 2     | Invisibility     |   15.99 | 1 |
| 3 | Shrinking     |    9.99 | 1 |


Vendors Table

| vendor_id        |    name        | 
| ------------- |:-------------:| 
| 1      | Kettlecooked | 
| 2     | Brewers     |   
  
MONGODB

Potion Collections
All data is grouped within documents:
{
	name: “Love”,
	price: 3.99,
	vendor: “Brewers”
}

Collections are simply groups of documents. Since documents exist independently, they can have different fields. 

Potions can have different key/value pairs but exist within the same collection:
{
   name: “Love”,
   price: 3.99,
   vendor: “Brewer”
}
{
   name: “Sleeping”,
   price: 3.99
}
{
   name: “Luck”,
   price: 59.99,
   danger: “High”
}

Dynamic Schema: Means that documents have different data but exist in the same collection.

Where can write Mongo?
In the ‘shell’
To get there, after we install mongo, go to terminal and type ‘mongo’, this will bring you into the mongo shell.

Commands go after the >

All installations of Mongo come with a command line program we can use to interact with our database using Javascript.

We can write regular Javascript code. Example:
> var potion = {
	“name”: “Invisibility”,
	“vendor”: “Kettlecooked”
}

> potion

{
	“name”: “Invisibility”,
	“vendor”: “Kettlecooked”
}

“Documents are just JSON-like Objects”

Some Shell commands:
> use reviews -> Will switch to a database called reviews (which does not exist yet! But will once we save to it!)
> db -> Shows us which DB we are using. Returns the current database name.
> show dbs -> Shows a list of all databases. Shows specifically name and size of DB.

“Documents need to be stored in Collections”
Documents are always stored in collections within a database.

> db.potions.insert(
	{
		“name”: “Invisibility”,
		“vendor”: “Kettlecooked”
	{
)
Whats cool here is that with the ‘insert’ collection method, we save the document into a ‘potions’ collection which is dynamically created. Meaning, this collection does not exist yet, but it will be automagically created. 

We get back a console log:
WriteResult({ “nInserted” : 1 })

Whenever we write to the database, we’ll always be returned a WriteResult object that tells us if the operation was successful or not.

We can use the ‘find()’ collection method to retrieve the potion from the inventory collection.
> db.potions.find()

All collection methods must end with parentheses. 
In this case, find will return our one entry, but include a “_id”.
Mongo requires that a unique ID gets written to each document entry, so it automatically creates one.

db.potions.find({“name”: “Invisibility” });
This is a query of equality. Checks the database for a entry with a “name” (key) of “Invisibility” (value).
Returns the entire document.
Returns all documents with that value.
So if we looked for all documents of a particular vendor, it would give them all to us.

Documents are persisted in a format called “BSON” which is like JSON.
It can store:
Strings
Numbers
Boolean
Arrays
Objects
Null

BSON has some extras:
ObjectID
Date - ISODate

To learn more about BSON, head to:
http://go.codeschool.com/bson-spec

MongoDB will preserve the precision of both floats and integers.

Dates, Javascript uses “0” as the first month. So subtract 1 off of the normal month date.
To create a date:
“tryDate”: new Date(2012, 8, 13) -> is “September 13th, 2012”

Dates get converted to an ISO format when saved to the database:
“expiration”: ISODate(“2012-09-13T04:00:00Z”)

Can store any data type inside of an array:
“ingredients”: [“newt toes”, 42, “laughter”];

We embed documents simply by adding the document as a value for a given field:
{
	…
	“ratings”: { “strength”: 2, “flavor”: 5 }
}

db.potions.find({ “ingredients”: “laughter” });
{
	“ingredients”: [“newt toes”, “secret”, “laughter”]
}

When performing a find on an array item, it will look through the entire array to find a value in there that matches to the find.

For searching for equality in an embedded document, we use dot notation.

db.portions.find( { “ratings.flavor” : 5 } )

— REMOVING AND MODIFYING DOCUMENTS —

Removing a document with a query of equality:
db.portions.remove({“name”: “Love”})

If our query matches multiple documents, then remove() will delete them all:
db.portions.remove({“vendor”: “kettle cooked”})

We can use the update() collection method to modify existing documents:
db.potions.update({“name”: “Love”}, {“$set”: {“price” : 3.99} } )

UPDATE ONLY MATCHES TO THE FIRST MATCHING DOCUMENT!

Update operators always start with a “$” sign, like in $set. 

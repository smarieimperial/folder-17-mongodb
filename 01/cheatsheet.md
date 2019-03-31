--------------------------------------------
db
- shows database you are currently connected to

show dbs
	- show databases

use test;
- get into the test database
--------------------------------------------

MySQL --- MongoDB
both are called a database
MySQL has tables, MongoDB has collections
MySQL has rows, MongoDB has documents
MySQL has columns, MongoDB has fields

shows all the databases:
show dbs

gets you into a databaes:
	use dbName

inside a database show a list of all collections
show collections

CREATE a collection

	db.createCollection("players")

SELECT
	select * from tableName
		db.collectionName.find()

	show all from collection but in a pretty way
		db.collectionName.find().pretty() 

	FIND BY ID of the document
		> db.test.find({"_id" : ObjectId("4ecc05e55dd98a436ddcc47c")}) // explicit
		{ "_id" : ObjectId("4ecc05e55dd98a436ddcc47c"), "x" : 1 }

		> db.test.find(ObjectId("4ecc05e55dd98a436ddcc47c"))           // shortcut
		{ "_id" : ObjectId("4ecc05e55dd98a436ddcc47c"), "x" : 1 }

	find a particular element in an array of a document
		db.classroom.find({"hobbies": {$in: ["hobby1"]}})

	get a list of everyone in your row
		db.classroom.find({row:3})

	find all the documents with a field of name and value of Steve
		db.classroom.find({name:'Steve'})

	find all the users in your row with a Mac
		db.classroom.find({row: 3, os:'Mac'})

	limit the fields that come back
		You can remove the _id field from the results by specifying its exclusion in the projection, as in the following example:


		this only shows the item and qty fields AND NOT the _id field

			db.inventory.find( { type: 'food' }, { item: 1, qty: 1, _id:0 } )

	count
		db.collection.find(<query>).count()

      find all documents that have a specific element in an array of the document
            db.classroom.find({"hobbies": {$in: ["hobby1"]}})


CREATING
	insert many at the sametime
	   db.products.insertMany( [
	      { item: "card", qty: 15 },
	      { item: "envelope", qty: 20 },
	      { item: "stamps" , qty: 30 }
	   ] );


      insert document with array:
                   db.classroom.insert({name: 'Steve', row:3, os:'Mac', hobbies:['Coding', 'Reading', 'Running'] })


	insert one document:
		db.classroom.insert({name: 'Steve', row:3, os:'Mac', hobbies:['Coding', 'Reading', 'Running'] })

UPDATING
	The restaurant collection contains the following documents:

	{ "_id" : 1, "name" : "Central Perk Cafe", "Borough" : "Manhattan" },
	{ "_id" : 2, "name" : "Rock A Feller Bar and Grill", "Borough" : "Queens", "violations" : 2 },
	{ "_id" : 3, "name" : "Empire State Pub", "Borough" : "Brooklyn", "violations" : 0 }
	
	this updates the first document it finds

	   db.restaurant.updateOne(
	      { "name" : "Central Perk Cafe" },
	      { $set: { "violations" : 3 } }
	   );

	this updates all of the documents it finds
		db.books.update(
			{ stock: { $lte: 10 } },
			{ $set: { reorder: true } },
			{ multi: true }
		)

		or

		db.restaurant.updateMany(
		   { violations: { $gt: 4 } },
		   { $set: { "Review" : true } }
		);

DELETING

	drop the database
		db.dropDatabase()

	delete a collection
		db.students.drop()

	delete document by id

		db.orders.deleteOne( { "_id" : ObjectId("563237a41a4d68582c2509da") } );

	delete document by date
		db.orders.deleteOne( { "expiryts" : { $lt: ISODate("2015-11-01T12:40:15Z") } } );

	delete all documents that match
		db.orders.deleteMany( { "client" : "Crude Traders Inc." } );

		db.orders.deleteMany( { "stock" : "Brent Crude Futures", "limit" : { $gt : 48.88 } } );

	remove only one
		db.products.removeOne(...)

	remove all that match
		db.products.remove( { qty: { $gt: 20 } }, true )

	empty a collection
		db.bios.remove( { } )


all commands listed out
mongo Shell Quick Reference — MongoDB Manual 3.4

https://www.tutorialspoint.com/mongodb/mongodb_dro…
mongo Shell Quick Reference — MongoDB Manual 3.4

PAVAN K.Mar 21 2017 8:46 PM 
---------------------------------------------------------------------------

To check if a field exists for any document in the collection and will return those documents that have the field:
$exists — MongoDB Manual 3.4

General query syntax to use: 
db.collectionName.find({ "fieldName": { $exists: true } }).pretty()
---------------------------------------------------------------------------
$exists — MongoDB Manual 3.4

PAVAN K.Mar 28 2017 11:39 PM 
MongoDB
	only achievable through mongoose:

	books
		_id, title, author

	libraries
		_id, name, books : [
			<book _ids go here>
		]


	EXAMPLE:
		books
			_id, title, author

			dskjflksdj goosebumps - rl stein
			3qiur80h3uf animorphs - ka applegate

		libraries
			_id, name books
			adksfjewhu9h east brunswick library  [dskjflksdj, 3qiur80h3uf]

			fhibu3h2thg cranford library  [dskjflksdj, 3qiur80h3uf]


MySQL
	
	books
		id, title, author

		1 goosebumps - rl stein
		2 animorphs - ka applegate

	libraries
		id, name
		1 east brunswick library
		2 cranford library

	library_books
		book_id, library_id

			1		1
			1		2
			2		1
			2		2


	SELECT * 
	FROM libraries l
	LEFT JOIN library_books lb 
	ON lb.library_id = l.id
	LEFT JOIN books b
	ON b.id = lb.book_id;
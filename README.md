# SqliteDatabaseAndroid_CookBook
This is simple cookbook for implementing Sqlite Database in Android. 

## Introduction to Sqlite
					
SqLite is actually a library that helps us make android database's.
SqLite is openSource.
SqLite is integrated into the Android System by default ,that is why we are learning about it.
SqLite is a database library and sql is the language use by program sqlite3.

sql commands and sqLite program commands(dot commands)
.open <databaseName.db>
.help
.quit
.tables 
.mode ascii
.header on
.mode colomn
.schema <table-name> (shows the statement that was used to create the table)

sqlite3 pets.db
Storage class and DataTypes.

Storage classes:
NULL
INTEGER
REAL
TEXT
BLOB 

Their is no boolean type in sqlite.
CREATE TABLE headphones(_id INTEGER,name TEXT,price TEXT,style INTEGER,in_stock INTEGER,
			description TEXT);
PRAGMA TABLE_INFO(<table name);
DROP TABLE <table name>; (delete a table)
single line in multiple lines.
not all commands end with ;
dot commands are case sensitive.
sql commands are not case sensitive.

CRUD actions.

CREATE TABLE pets(_id INTEGER,name TEXT,breed 
	TEXT,gender INTEGER,weight INTEGER);

SELECT * FROM <table name>; (select all column and rows from table).
SELECT * FROM <table name> WHERE _id==1; (select a row from table where id=1).
SELECT * FROM <table name> WHERE weight>=18;
SELECT name,breed FROM <table-name>;
SELECT * FROM <table-name> ORDER BY name ASC; (display in assending order of name)
SELECT * FROM <table-name> WHERE breed!="unknown";

DELETE FROM pets;
DELETE FROM pets WHERE _id=<id-of-pet-to-delete>;
INSERT INTO pets (<column1 name>,<column2 name>,<column3 name>,
			<column4 name>,<column5 name>)
			VALUES("column1 value","column2 value","column3 value",
			"column4 value","column4 value");
INSERT example	:

INSERT INTO pets(_id, name, breed, gender, weight) 
		VALUES(2, "Garfield", "Tabby", 1, 8);
		
UPDATE pets SET breed="Pomeranian" WHERE breed=="Pomeranian Terrier";
UPDATE pets SET weight=0 ;

TABLE Constraints

PRIMARY KEY : can only be one primary key per table.
AUTO INCREMENT: automatically increments the id.
NOT NULL : make sure value cann't be null.
DEFAULT<value>: we can se default value.
//--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------			
## How to implement in Android
	implementing sqlite database in android involves few steps.
	some basic ones are.
	-identify Schema.
	-Make a contract class.
	-Extend an abstract class SqLiteOpenHelper and implement the methods.
	-use the extended class from SqLiteOpenHelper class to get the Readable or Writable database object.
	
 ### Identify Schema :
  this is fency way of saying "you need to plan out your database
					structure. to do so we need to answer 2 questions.
	What are the names of the tables in database.
	What are the names and datatypes of columns.

### Contract Class
We use contract classes in android for database.
reasons to use contract classes.
-all data related to Database at one place.
-no spelling errors.
-easy to update the database.
contract classes are final because we don't want to make a subclass of contract class.
-contract class contains inner class for each table in database .
-the inner classes have constants for table name,column names ,and some possible values for some columns eg for column gender GENDER_MALE=1 and GENDER_FEMALE=2.

### SqLiteOpenHelperClass
-SqLiteOpen helper class is an abstract class that we should extend.
-it can create a new database and open existing one .
-it has two abstract methods onCreate() and onUpgrade().
-onCreate() will be called when database first created.
-onUpgrade() will be called when we want to update the schema.(it drops the existing table and recreate it).
-SqLiteOpenHelperClass should have constants for data name and version.

### SqlDatabaseObject
-it is object of our database.
-it is used to execute queries on the database.
-getReadableDatabase returns a database object.
-getReadableDatabase checks if database exists ,if no make databaseObject using onCreate and return.
 else if database does exist than make a databaseObject and attach it with database
 an return it. 
 -thier is one other method called getWriteableDatabase Method
 
 ### ContentValues
 -is a class.
 -used to create Key Value pair ,that we will insert into database.
 -some methods of database object eg query() takes in contentValues as a parameter.
 
 ### .insert()
 method return row id, if unsuccessfull than returen -1;
 
### Projection 
 fency way of saying these are the columns we are intrested in getting back.
 -projection is a string Array that contains column names. eg String[] projection = {column1Name,column2Name};
 
 ### .query()
 -this method returns a cursor object.curson object contains rows.
 -this method has some 7 paramenters.
 -first paramenter is TableName.
 -rest of 6 paramenters are optional ,means we can pass in NULL.
 -second paramenter is projection if we pass projection as NULL all rows will be selected.
 -third paramenter is selection , its datatype is string it is basically a where clause eg StudentEntry.COLUMN_STUDENT_NAME + "=?" , //here ? is a place holder 
 -fourth parameter is selectionArgs ,its data type is array of strings ,and it has values that will replace ? placeholder in selection parameter. eg new String[]{nameString};
 
### Cursor Object
 -We get cursor object in return if we execute a query on a database. eg Select * From pets;
 -cursor object has methods to move through the rows for example moveToFirst();
 -methods like moveToFirst() returns boolean true or false to indicate if move is possible or not.
 -thier are other methods like cursor.getString(columnIndex) which returns the value at particular index.
 -always use cursor.close();
				
### -CRUD Actions
-getReadableDatabase() and getWriteableDatabase() returns a database object that represents underlying database file.
-this database object has methods like insert(),create(),update(),delete() we can use these methods to manipulate the underlying database file.


		

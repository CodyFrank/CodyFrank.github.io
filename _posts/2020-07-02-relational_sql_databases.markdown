---
layout: post
title:      "Relational / SQL databases"
date:       2020-07-02 12:02:03 -0400
permalink:  relational_sql_databases
---

![](https://cdn.sqlservertutorial.net/wp-content/uploads/SQL-Server-Sample-Database.png)


MySql, Microsoft SQL, PostgreSQL, SQL, SQLite, SQLbase, Oracle and many many more. These are all examples of relational databases or sometimes refered to as sql databases. 

## What is the goal here?
Relational databases are simple a way to store information or data. Twitter has to keep track of its users and tweets and spotify needs a way to keep all your songs. Databases are how its done. Think of it as a storage bin for data. Without a database our data is lost every time we terminate our program. 

## what types of Databases are there?
DataBases are broken up into two main categories. The First one being Relational or SQL databases.
Surprise thats what this blog post is about.

![](https://media.giphy.com/media/95ZYXmOCd9BBK/giphy.gif)

These are the traditional data storage systems. 

The second is non-relational / noSQL databases. These are no the topic of todays post but I will breifly explain. non-relational databases store data in documents similar to JSON notation or JavaScript objects.

similar to this 

```
{
  _id: "194jdh867439bskl4",
	firstname: "John",
	lastname: "Doe",
	address: {
	  street: "1 database way",
		city: "New York",
		state: "NY"
		}
	}
```

This is a very basic example but I think seeing a non-relational database will help to understand relational databases and why they are different.

## Well what does a relational database look like?
A relational Database is made up of a tables. They can be thought of much like a spread sheet. Each table has rows and coloums. The row represent another entry in out table. The columns represents another attribute of that enrty. 

For example

![](http://readme-pics.s3.amazonaws.com/sql-students.png)


Each row represents a different student and each column represents a different attribute that student has. 

## How do we create a table and make changes?
Relational databases use a structure query language (SQL) that allow us to tackle this task. Using SQL we can create tables, add and remove data, update data, and delete data.
the language looks like this. note im using sqlite3 syntax for simplicity however each language may differ or use a different name when creating a database. This is usually beacuse each database has slightly different capabilities.

create a new database (collection of tables)
```
sqlite3 pets_database.db
```

Create a new table
```
CREATE TABLE cats (
        id INTEGER PRIMARY KEY,
                name TEXT, 
                age INTEGER
            );
```

delete table
```
DROP TABLE cats;
```

altering tables
```
ALTER TABLE cats ADD COLUMN breed TEXT;
```

inserting data
```
INSERT INTO cats (name, age, breed) VALUES ('Maru', 3, 'Scottish Fold');
```

reading data 
all data
```
SELECT * FROM cats;
```
some select data
```
SELECT name FROM cats;
```
multipul parts of data
```
SELECT name, age FROM cats;
```
a whole row
```
SELECT * FROM cats WHERE name = "Maru";
```
please note you can narrow down the search criteria with WHERE

and updating data
```
UPDATE cats SET name = "Hana" WHERE name = "Hannah";
```

delete data
```
DELETE FROM cats WHERE id = 2;
```

So many caps!

![](https://media.giphy.com/media/1nR6zAvRESERIoA1T1/giphy.gif)

These are a few simple examples of how sql could be used to manage and quary a database. Its also worth noting that sql is able to do this very efficiently.

## I thought this was a relational database why isnt there any relations

In a single database there can be many tables both connected and not connected. This makes a large web of tables.

![](https://media.giphy.com/media/xT1R9MyOQLEB2WY6Oc/giphy.gif)

These connections are made by putting the id of a parent into a colum in the childs table. This is called a foreign key. It represents a relationship, or a parant could ** have many ** children and a child ** belongs too ** a parent. 

For example

![](https://cloud.githubusercontent.com/assets/18357112/17033515/7617ab2a-4f4c-11e6-8545-f179bdeeb500.JPG)
![](https://cloud.githubusercontent.com/assets/18357112/17033522/7d7ac122-4f4c-11e6-9116-2cfebe111f27.JPG)

You can think of the manager id as a pointer to the manager that each perticular employee reports too.

## What if you want to have many to many. 
#### The problem 
As programers we there is no sut number of relationships that one could have. For example a song could have one artist or it could have a collection of artist. Each song is different. Eminem might have 70 songs where Nirvana may only have 30. Additionally some of those songs may have multiple artists like Forever by Lil Wayne, Drake, Eminem, and Kanye West. 

#### The solution
To overcome this obstical we use a join table. This table is conventionally named after the tables it joins. For example a music database may have a artists table and songs table and and artists_songs table. this join table will have a foreign key for both the artist it points to and the song it points too. Now if a song has 4 artists there are 4 rows in this join table with the song forign key matching and different artist forign keys.

This has been a very basic explanation of SQL databases. It is very important to web development and many types of software engineering to know your way around databases. I hope this is a start to your database journey. 


---
layout: post
title:      "Non-Relational Databases / NoSQL Databases."
date:       2020-07-06 14:34:27 +0000
permalink:  non-relational_databases_nosql_databases
---

![](https://docs.mongodb.com/guides/_images/compass-insert-document-inventory.png)


Last week I made a blog post talking about Relational Databases also known as SQL databases. You can check it out [here](https://codyfrank.github.io/relational_sql_databases) if you want, however its not requires to learn about relational databases today.

Traditionally programmers used tabels to store data in a database. This ment you needed to create these tables and every new entry had to include the same info. This stratagy is effective and still used today, However its not alway the best fit for modern database needs.

![](https://media.giphy.com/media/1Qdp4trljSkY8/giphy.gif)

## What is Non-Relational
Non-Relational databases are still just a way to store information. The difference is how that data is stored. Non-Relational databases have abandoned the tabular design and replaces it with JSON data (JSON stands for JavaScript Object Notation and it looks much like a JavaScript object), XML (Extensible Markup Language, a document type designed to be both human and machine readable), Binary( Base-2 number system or Zero's and One's)

![](https://i.stack.imgur.com/S7mwu.png)

NoSQL is often used to refer to these databases. Be careful! NoSQL can be used because it referes to a database that does not use SQL or, more commenly, it can be used to refer to Not Only SQL.

## What are some big names?
MongoDB, Redis, CouchDB, RavenDB, and many many more. Each of these databases vary in their own ways however all of them share a key value structure of storing data. Some have their own language to retrieve data, some use JavaScript and everything inbetween. 

## Why use non-relational Databases?
* Scalable - Non-relational databases typically devide the storage into parts meaning if you need more space its usually possible to get more servers and connect them to your existing database. This is called Horizontal Scaling.

* Fast - In terms of how quickly and easily a NoSQL database can be created impemented and maintained. No need for creating tables and the complexity of managing them. No need for changing old tables and updating all the data when adding more info to the database. 

* Can store multiple Data Types - Most Non-Relational databases allow you to store data in multiple forms. This includes Hashes/Objects, arrays, strings, integers, ect

      1. Flexability 
      2. Heirarchy (parent child) structure in database 

* Simple and easy to learn

* No more need for SQL

If you have been keeping up with my blog you know I have been spending some time working on the MERN stack learning. Learning Node.js and Express.js. A big part of this stack is using MongoDB as a database and Mongoose as a ORM to map data to the database. This is a large part of the backend and it is critical to understand for this stack. This is one of the main reasons I wanted to work with the MERN stack and one of the reasons I enjoy it so much. I personally have found it much easier to understand and learn then the relational databases and SQL. I encourage everyone who has taken the time to read this to continue learning about relational database (but dont forget SQL).



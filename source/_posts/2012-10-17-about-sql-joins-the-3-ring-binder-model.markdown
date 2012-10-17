---
layout: post
title: "About SQL Joins: the 3 Ring Binder Model"
date: 2012-10-17 11:42
comments: true
categories: 
---

Any programmer worth their weight in bitcoins is going to need to know a bit about databases.  If you're not familiar with databases, you should take some time to read up on them, but hurry back, because for this post we're going to be talking about the different kinds of **JOINS**.  'Joins' are a method of linking multiple tables in a database together so we can access values from them with a single query.  Seems simple enough, but joins can be confusing.  Since tables in a database aren't tangible things, it can be difficult to visualize the links we create between them.  I for one find it difficult to really wrap my head around a concept until I can draw it, which made for some interesting moments in high school sex ed, but also led me to develop a method for visualizing the how the different kinds of joins work! Let's have a look:

## 3 RING BINDER

Picture the two tables in your database as two pieces of looseleaf.  The blue lines represent our rows, or records.  The dashed lines represent our columns.

{% img center /images/3ringbinder1.jpg 'database tables' %}

Now, using the SQL syntax for sqlite3, we'll start writing our join by stipulating which tables we'd like to link.

``` sql Writing a SELECT query using JOINS
SELECT * FROM persons
INNERJOIN orders
```

We'll go over INNERJOIN in a second, but for now just picture us putting our two pieces of looseleaf into a three ring binder. We're selecting from both persons *(left table)* and orders *(right table)*, and linking them with a join.

{% img center /images/3ringbinder2.jpg 'joined tables' %}

Now that our pages (tables) are in a binder (joined), we need to stipulate which column to link them on.  

``` sql Setting a join column
SELECT * FROM persons
INNERJOIN orders
ON persons.id = orders.pid
```

Here, we're saying that we should link the rows in persons and orders based on the value of the **'id'** column in the person table, and the **'pid'** column in our orders table.  When the values in those two columns match, we can assume a correlation between the two records.

{% img center /images/3ringbinder3.jpg 'join column' %}

Now we've joined our tables.  But there are a few different ways to join two tables, which will effect which rows are returned.  Let's have a look:

## Inner Join:

An inner join, or left inner join, will produce only the records for which there is a match from tablea *(persons)* and tableb *(orders)* (records produced are highlighted):

{% img center /images/3ringbinder4.jpg 'join column' %}

## Left Outer Join:

A left outer join will produce a complete set of records for tablea *(persons)*, with matching records from tableb *(orders)* **when they exist**.  If there's no match from tableb, the right table's join column value will be 'null'.



## Right Outer Join:

A right outer join will produce a complete set of records for tableb *(orders)*, with matching records from tablea *(persons)* when they exist.  If there's no match from tablea, the left table's join column value will be 'null'.



## Full Outer Join:

A full outer join will produce **the complete set of records from both tables**.  Wherever there is no match, the value will be 'null'.



## Conclusion:

Hopefully this visualization will help you wrap your head around database table joins, (and maybe conjure fond memories of decorating your Five Star™ binders...).  For more on databases and visualizations for joins, you can go [here](http://www.databasejournal.com/features/mysql/article.php/2248101/Referential-Integrity-in-MySQL.htm "Referential Integrity"), or [here](http://www.codinghorror.com/blog/2007/10/a-visual-explanation-of-sql-joins.html) for some great insights.      
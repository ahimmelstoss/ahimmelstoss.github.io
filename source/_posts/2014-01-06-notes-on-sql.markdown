---
layout: post
title: "Notes on SQL"
date: 2014-01-06 21:47
comments: true
categories: 
---

In part with my PHP learning, I am reading Larry Ullman's <a href="http://www.amazon.com/PHP-MySQL-Dynamic-Web-Sites/dp/0321784073">PHP and MySQL for Dynamic Websites</a>, which has been reintroducing me to raw SQL statements, after living in Railsland for the last two months. I had forgotten how much I love SQL, for its clean querying and data organizing. 

One thing that we were left to think about at Flatiron was the idea of code as a story. Code tells the story of your web application: there's the setting, characters, and information about the characters. Databases are the heart of that story, because they illustrate the connections between your code's characters. And SQL's `JOIN` query is at the heart of how these connections are told.

As a refresher, `JOIN` is a query that uses two or more tables, essentially creating a virtual table between them (you never really see this table, however). The most common join, and the focus of my post is the inner join. An inner join returns all of the database records from the named tables where a match is made on the specified criteria.

I'm going to be lazy about my examples here and adapt them from Larry Ullman's forum. A brief synopsis of the story about the forum. This database has messages table (a message has a subject, name, message, posted date, etc), a forum table (a forum has a subject), and users table (a user has a name, email, password, etc). On these tables, there are primary keys as integers. The relationship is that a message has one user and one forum (so there are foreign keys* for both on the messages table). 

Let's say I want to find all of the messages on a forum that have the subject "Cats". There are three ways to do this. 

The first does not actually utilize a `JOIN` statement:
```mysql
SELECT m.message_id, m.subject, f.subject 
FROM messages AS m, forums AS f 
WHERE m.forum_id = f.forum_id AND f.subject = "Cats"
```
The downside of running this query is that it overloads the `WHERE` clause, both limiting (where the forum_ids match) and describing (the forum subject as "Cats"). This isn't necessarily more expensive on the database, but it is not the clearest way to query.

Using a `JOIN` is cleaner and more descriptive:

```mysql
SELECT m.message_id, m.subject, f.subject 
FROM messages AS m JOIN forums AS f 
ON m.forum_id = f.forum_id 
WHERE f.subject = "Cats"
```
Here I'm using the `ON` clause to describe the relationship between the forum_id in the forum's table and the forum_id (as a foreign key) on the messages table.

For some syntactic sugar, there's an even nicer way to write this with a `USING` clause:

```mysql
SELECT m.message_id, m.subject, f.subject 
FROM messages AS m JOIN forums AS f 
USING (forum_id) 
WHERE f.subject = "Cats"
```
A `USING` clause is only possible if you name your primary key the same way in both tables. In this case, a clean way of doing that is <em>tablename</em>_id.

This is a very common type of query on a database, so writing it the way that seems cleanest is an important practice. I like the sound of the `USING` clause, so therefore prefer it.

*Foreign keys connect two tables in order to illustrate the relationship between them, and most importantly, to prevent bad data. A message's forum_id as a foreign key points back to a forum's id, signaling that that message belongs to that forum. So if you want to delete an entire forum, having the forum_id on the messages that belong to it prevents you from doing so without first deleting those messages. Without a foreign key in place, you can be left with bad data: messages from a deleted forum are left lying around.
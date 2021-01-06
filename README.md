# Sell me - Project for advanced design and architectural patterns

## Table of Contents
1. [Overview](#overview)
    1. [Functional Overview](#functional-overview)
    2. [Technical Overview](#technical-overview)
2. [Functionality](#functionality)
3. [Technicality](#technicality)
4. [Patterns](#patterns)

## Overview
### Functional Overview
This project is an example of business application that can be used as a way of exposing local business to more 
customers by using the internet. It supports below functionalities:
* creation of accounts
* wishlist
* search for products with matching criteria
* reservation

Functionalities are described in <b>[Functionality](#functionality)</b> section.

### Technical Overview
Application is created with:
* Java 11 and Maven
* PostgreSQL and Redis
* SpringBoot 2, Hibernate, FlywayDB, and Thymeleaf
* Mapstruct and Lombok
* Bootstrap

Technicalities are described in <b>[Technicality](#technicality)</b> section.

## Functionality
Application allows for creation of both customer and seller accounts. As one can expect seller account allows user for
creation of new products that will be available for customers to reserve. On top of that it allows for  managing 
already listed products by given seller. Customer account can add products to a whitelist and reserve an product if it 
is still available.
 
 Wishlist functionality allows for bookmarking some products that the customers are particularly interested in. It works
 for both users that are logged in as customers and for users that are not logged in. In the first case the wishlist is
 saved in database to allow customer to create long-lasting wishlist. In the second case wishlist is stored as long as 
 user's session is active and after the session is destroyed so is the wishlist.
 
 The application also supports search functionality that allows the user to filter all available products by several 
 attributes like name, description or price. 
 
 To finalize the transaction the application supports reservation functionality that sends the email to both the seller
 and the customer that've reserved the product to allow for the transaction to be finalized and it decreases the 
 quantity of the available product by one.

## Technicality
For connection with PostgreSQL database that stores all the data SpringBoot Data is used. It allows for easy change of
database in the future.

Redis is used as a way to store session data of users that are not logged into system and browse it anonymously.

Hibernate is used as a ORM (object-relational mapping) tool for mapping object-oriented domain to a relational database.

For database migration FlywayDB is used and for server-side rendering Thymeleaf is used, but in a future it is possible
that it will be replaced with client-side rendering with e.g. React.

For making the programming experience better both mapstruct and lombok are used to generate code and allow to focus on
the business side of programming.

For making the server-side rendered site pleasing to the eyes Bootstrap is used.

## Patterns
Patterns used in this projects include:
1) <b>Data Mapper</b> - used by Hibernate to perform bidirectional transfer of data between a persistent data storage 
(database) and in memory data representation (domain). It allows for keeping both of the representations independent of 
each other and the data mapper itself. The layer of abstraction that is data mapper is composed of multiple mappers that
perform the data transfer. In contrary to Active Record it allows to consider business domain and the database more 
independently. This pattern allows for greater flexibility between domain and database - the application architecture 
don't have to be identical to database representation. On top of that data mapper can make use of the database in more
efficient way than a naive active record implementation would allow.
2) <b>Data Transfer Object</b> - All calls to controllers receive and response with DTOs - because of that it's easier
to "protect" the domain and not leak sensitive information to customers by mistake. On top of that it reduces the amount
of data that needs to be sent across the wire in distributed applications. They also make great models in the MVC
pattern. Another use for DTOs is to encapsulate parameters for method calls. This can be useful if a method takes more 
than 4 or 5 parameters.
3) <b>Domain Model</b> -  "model" of the objects required for your business purposes. Said model is something that 
Martin Flower would call "anemic" domain model. But it isn't necessary a bad thing. Complexity grows naturally as code 
and requirements are added. Applications can start out very simple, as CRUD, but behavior/rules can come with time. The 
nice thing is we don't have to start out complex. We can start with the anemic domain model, that's just properties
(getters and setters), and with just standard refactoring techniques we can move towards a true domain model.
4) <b>Service Layer</b> - layer between the presentation layer and the data access layer. The presentation deals with 
HTTP requests and responses and presentation logic in general (e.g. workflow between pages) and delegates to the service
layer for the business, transactional logic that the application uses. The service layer then delegates to a data access
layer to access the database. It allows to comply with single responsibility rule to make code more readable and easier
to maintain.
5) <b>Metadata Mapping</b> - allows developers to define the mappings in a simple form, which can then be processed by 
generic code to carry out the details of reading, inserting, and updating the data. It allows to limit the number of 
lines of code needed for mapping domain to database.
6) <b>Query Object</b> - allows to hide SQL code to allow for more people (without SQL knowledge) to create meaningful
code. Furthermore, because of it the developer doesn't need  to know how exactly the database schema looks like. It's a 
structure of objects that can form itself into a SQL query, allowing for the developer to create query by referring to 
classes and fields rather than tables and columns. In this way those who write the queries can do so independently of 
the database schema and changes to the schema can be localized in a single place. For example Hibernate Query Language, 
or Search Criteria used in application.
7) <b>Repository</b> - allows all of the code to use objects without having to know how the objects are persisted. All 
of the knowledge of persistence, including mapping from tables to objects, is safely contained in the repository. Very 
often, it's possible to find SQL queries scattered in the codebase and when adding/changing column in a table it is
necessary to search code files to find usages of said table. The impact of the change is far-reaching. With the 
repository pattern, only change of one object and one repository is required. The impact is very small. It also makes 
testing easier, because of the ability to mock repository and not having separate database just for testing.
8) <b>MVC</b> - software architecture that separates business logic from the rest of the user interface. It does this by
separating the application into three parts: the model, the view, and the controller. This separation of 
responsibilities allows bigger flexibility while developing the application further. Being able to tease these 
components apart makes the code easier to re-use and independently test.
9) <b>Front Controller</b> - receives all request in one main controller and then dispatches them to different 
controllers. Spring Framework uses the Front Controller pattern to realize MVC. One of the main advantage of using 
front controller is to avoid duplication. Front controller handles all the requests, this implementation of centralized 
control that avoids using multiple controllers is desirable for enforcing application-wide policies such as users 
tracking and security.
10) <b>Template View</b> - generates HTML code from tags. It allows for separation of design and logic side of frontend.
11) <b>Server Session State</b> - techniques used to keep the user session state on the server, with the only requisite 
of a session id on the clients to distinguish between different users. It allows the wishlist functionality by saving
user's session into Redis (only when user is not logged in, otherwise the wishlist is saved to database).
12) <b>Identity Field</b> - in  order to write data back from domain to database tie from database to the in-memory 
object system is required. In order to do so the primary key of the relational database table is stored in the object's 
fields.
13) <b>Foreign Key Mapping</b> - maps an object reference to a foreign key in the database. Allows for one to many
mapping, used in domain model (Product)
14) <b>Association Table Mapping</b> - allows for many to many mapping by using new table with foreign keys to each of 
the tables that are associated.  
15) <b>Single Table Inheritance</b> - Relational databases don't support inheritance, so when mapping from objects to 
databases we have to consider how to represent our nice inheritance structures in relational tables. When mapping to a 
relational database, we try to minimize the joins that can quickly mount up when processing an inheritance structure in 
multiple tables. Single Table Inheritance maps all fields of all classes of an inheritance structure into a single 
table.
16) <b>Optimistic Offline Lock</b> - Once outside the confines of a single system transaction, it's not possible to 
depend on database manager alone to ensure that the business transaction will leave the record data in a consistent 
state. Data integrity is at risk once two sessions begin to work on the same records and lost updates are quite 
possible. Also, with one session editing data that another is reading an inconsistent read becomes likely. Optimistic 
Offline Lock solves this problem by validating that the changes about to be committed by one session don't conflict with 
the changes of another session. A successful pre-commit validation is, in a sense, obtaining a lock indicating it's 
okay to go ahead with the changes to the record data. So long as the validation and the updates occur within a single 
system transaction the business transaction will display consistency. Optimistic Offline Lock assumes that the chance of 
conflict is low. The expectation that session conflict isn't likely allows multiple users to work with the same data at 
the same time.
17) <b>Unit of Work</b> - keeps track of everything done during a business transaction that can affect the database. 
At the end, it figures out everything that needs to be done to alter the database as a result of work. Hibernate 
uses it's own Unit of Work implementation as Session object, because of that it's possible not to open new session for 
each operation.
18) <b>Lazy Load</b> - allows for delaying the loading of object until the point where it is needed. Because of that
hibernate won't load all the children elements from parent instantly and only when it has to perform some kind of
operation on them.

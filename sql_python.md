# What is SQL?

## Type

Relational database management

## Language

Structured query language

## Schema design

- primary and foreign keys
- Relationships between tables are established through the keys

## Scalability

- Difficult to scale up as its very structured.
- Scaling up gives it more resources for example increasing the ram.
- Scaling down gives it less recourses

Vertically scales - Scaling up/ down, adds or removes resources.

(scaling out, duplicates it)

## Structure

- DDL - data definition language. Used to define/ modify schema

- DML - data manipulation language. Used to manipulate data in tables

- DCL- data control language. Used to controll access and permissions

- TCL - Transaction control language, manage tractions.

# What is NoSQL?

Not only SQL, is a schema- less database, people use their own queries. It allows semi structured datasets.

## Language

Many different ways data can be stored and accessed. Syntax varies amongst databases.

- C++/ C/ C#
- Python
- PHP
- {Json}
- XML
- CSV
- Java
- Ruby
- Visual basic

## Schema design

NoSQL can query without SQL, their query driven design if modling is:

- One-to-few embedding
- One-to-many is referrencing
- One-to-infinity is parent referencing, document parent collection and they can have children.

Collections (in mongodb), they are **Not** tables but they are connected to each other in some way.

## Scalability

In SQL they have vertical scaling, in NOSQL we have horizontal scaling (which allows scaling out and duplicate versions of the same database)

**Why do we prefer Horizontal scaling?**

- Horizontal scaling is better because it gives high **availability**.

  -> If we have multiple servers and one or two of them stop working the others will still continue working.

- Good for sharding and adding more servers as a safety net

### Disadvantage to horizontal scaling

- Required high maintainance to connecting all the servers and manage them.

- Requires expertise in management of servers and operations

- How will these servers connect? Handling this can be difficult

## Structure

- Much more fluid, less structures. Alows semi-structure.

- More open to doing different things

- Do have the ability to query databases

- Almost SQL like systems but databases that dont use SQL

## Database types

### Key: Value

The most basic datatype is the key-value stores.

- Key must be unique. Value can be anything even another KVP (key value pair)

**Example**
{City: "New York City", Country: America}

#### MongoDB Example

**Document databases**
Extend the concept of key- value pairs represented in:

- Json
- XML
- BSON/ Binary JSon

Documents work similar to tables without the clear defined rules of relationships

### Column family database

Column families contain related information, it consists of multiple rows.

- Rows can have different number of columns

- A column will contain; name, value and time stamp.

- Group multiple columns together to form **Clusters**. When a c

- Grouped logically, similar to a table but more flexible.

Userprofile is a column containing all this information.
![Column with its contents](Nosql_columns.png)

#### Use cases

- Time series data
- Sensor data
- Write heavy systems

### Graph store

Nodes (entities) connected to others through relationships.

#### Edges

These relationships are known as **edges**.

There are different types of edges and the directions they face have meaning.

mini over view: Forming relationships (edges) through connecting entities (nodes).

#### Use cases

- Socail networks
- Knowledge graphs
- Fraud detection

### Overview

![sql_vs_nosql](sql_vs_nosql.png)

- SQL allows a structured format nosql allows semi structure

- SQL is structured quering language and noSQL has many different types of language depending on the database.

- SQL has a scheme, while NOSQL is schema-less

- NOSQL has different types of databases: Key - value pairs, Document oriented, Wide- column stores and Graph store databases.

# MongoDB

MongoDB is an open source NOSQL database. This means it is **Not Only SQL**.

The data is stored within documents which are Json- like objects.

Data is sent in as Json and stored as bson.

The max size of a mongoDB file is 16mb.

These documents are grouped together in collections. There is no real structure for a collection as they are schemaless.

A mongodb node is one server or machine in the database system.

## Architecure

Occurs in the background, it is designed to be flexible.

### Replica sets

Several copies of the same data is held in multiple nodes. A
primary node is the main source for all the writen operations. This is where all data modifications begin and are implemented initially.

Secondary node is a mirror of the primary node as it duplicates the data. This is used for dispersing the read workloads and load balancing.

![Replica]("mongodbreplicaset.png")

#### Advantages

- If any issues occur to a primary node the secondary node can take its place.

- Can write straight to the secondary node but not very common.

- Helps with redundancy.

#### Disadvantages

- Writing to the secondary node can cause data inconsistency.

### Sharding

Horizontal scaling is the core to sharding. Large datasets are divided into smaller pieces and distributed across multiple shards (servers).

Sharding helps with ensuring all the connections are still together through distributions across machines.

#### Advantages

- Improves performance
- Improves scalbaility in comparison to vetical scaling.

#### Disadvantages

- Increase complexity
- Operational overhead, backups, constant monitorisation and maintenance.

## MongoDB Use cases

### Real time analytics

Aggregation framework and ability to scale horizontally, it is perfect for applications that require quick insights from large datasets.

**Example:**
MetLife uses MongoDB for 'The Wall', a customer service app that consolidates data from 70+ legacy systems.

Business can track user behaviour and make customer experiences better.

### Gaming applications

MongoDBs flexible schema allows dynamic content and real time updates which allows devs to change in game behaviour constantly.

**Example**:

EA, uses MongoDB to track user data and create personalised gaming. It allows the storing and processing of data to be conducted quickly.

## Interact with MongoDB

In order to interact with our MongoDB database we use MongoDB shell, known as MONGOSH.

## Establish relationships

### Embedding

Embed subdocuments inside a larger document.

This is good for:

- One-to-one
- One-to-many

Good for denormalised data. Default method in MongoDB as it is good for read.

### Referencing

Best for normalised data, will use an ID.

Seperate documents will be referenced here, this helps with redundacy but affects the read function.

This is good for:

- Many-to-many

### Collection Validations

When going over the validations of our collections we have the options to change the level for security purposes.

- Moderate will implemenet the rules for new entries.

- Strict will implement rules all the time.

### Functions

- number long or number int will allow long numbers in MongoDB. Use doubles not floats.

- Show collections is a function which will show the collections.

- db.getCollectionNames() - will print the names of the the collections created within the database.

## Activities -Code 30/ 07/ 2025

Exercise: The data in this database (starwars) has been pulled from SWAPI Reborn - Star Wars APIs & Explorer.
Note: The old api is no longer supported, there may be some differences between the data we have been working with and the data in this api.

As well as 'people', the API has data on starships.
In Python, pull data on all available starships from the API.
The "pilots" key contains URLs pointing to the characters who pilot the starship.

Use these to replace 'pilots' with a list of ObjectIDs from our characters collection, then insert the starships into their own collection. Use functions at the very least! OOP preferred.

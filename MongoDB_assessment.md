# Assessment on MongoDB

24 questions, all multiple choice, 65% pass mark, 60 minutes.

Will be done Monday morning **04th August 2025**.

Topics include:

## NoSQL database types

A database in NoSQL is a storage container which holds semi/ no structured data. This is good for flexibility, scalability and diverse demands.

Often times the data stored in this format is within a data warehouse or a data lake.

![NoSQL databases](NoSQLDatabases.jpg)

### Document database

In a document database the data is stored as a Json, Bson or XML file. They do not require a fixed schema.

Documents can be stored and retreived in a form closer to the data objects within the applications. This in turn means less translation is required when using the data.

To access the data we use index values this allows faster querying.

Within document databases are collections, a method of grouping documents together with similar contents or purposes. However, the documents within a collection do not require a fixed schema so they may have different fields and structures.

#### Key features

- Flexible schema: Data within a document can have a different number of rows and different fields. Although some can have schema validation.

- Document model: Documents map to objects using programming languages, allows faster applications.

- Each document handled is an individual object

- Distributed: Data is stored across different notes rather than a single machine.

- Json storage: Supports the storage of different data. We can have all the information that you need to access together (embedding) in one document or take the liberty of creating separate documents and then linking them (referencing).

- No foreign keys: No dynamic relationships between documents, so documents can be independent.

An example is MongoDB, content management.

#### Crude Operations

Document databases use either an API or query language to allow CRUD (Create, read, update and delete).

- Create: Documents can be created inside the database and will be set a unique identifier.

- Read: Documents can be read from the database. The API/ Query language allows documents to be queried using their unique identifiers or field values.

- Update: Documents can be updated in whole or part.

- Delete: Can delete documents from the database.

### KEY: Value stores

This is the simplest form of a NoSQL database. Every element is stored in key - value pairs. Similar to documents the data can be retrieved using a unique key allocatted to each element in the database.

This is very similar to a relational database as it has two columns the key and the value assigned.

#### Key features:

- Simplicity and speed: Data retreival is fast due to direct key access

- Scalability: Designed for horizontal scaling and distributed storage

- Ideal for caching and real time applications

An example is Amazon DynamoDB, cloud based scalable applications.

### Column family database

Data is stored in columns rather than rows.

When we want to run a small number of columns we can read those columns directly without consuming memory with unwanted data.

Column databases are designed to read data more efficiently and retrive data quickly.

- Used to store large amounts of data.

#### Key features

- High scalability: Supports distributed data processing.

### Graph store

## Querying

## normalisation vs denormalisation

## Database scaling

## Graph databases (research this)

## Relationships in Mongodb

Mongosh commands
Mongodb architecture
Replica Sets
Sharding
.aggregate()
`_id:` field

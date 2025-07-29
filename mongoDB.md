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

![Sharding]("sharding.png")

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

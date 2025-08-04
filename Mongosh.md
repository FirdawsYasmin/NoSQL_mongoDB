# Mongosh

This section will discuss what we have already done within the training including the code written.

1. In Mongo compass click into your server.

2. There will be an option to get into mongosh which is mongoDBs terminal/ shell.

3. Once created you will have your server, inside will be a database and inside that will be a collection. We store documents inside a collection.

## Mongosh

### Create a database called sparta

```
test> use sparta  # use <name of database>
output:
switched to db sparta
>
```

This will not appear on the left until a collection has been created.

### Creat a collection

```
>db.createCollection("academy") # db.createCollection("<name of collection">)
output: { ok: 1 }
```

Refresh the server on the left and the new database and collection will appear.

### Add documents to a collection via monosh

#### insertOne document

```
# db.<collection name>.insertOne({key value pairs})
# only inserting one document called new document

sparta>db.academy.insertOne({name: "New document"})
output:
{
  acknowledged: true, # successful
  insertedId: ObjectId('688f24a8823b363a81e4eef0') # everything gets an objectid in mongoDB it is automatically generated
}
```

#### insertMany documents

```
db.academy.insertMany([{course: "Engineering", "length": 10}, {course: "Data Analysis", "length": 8}]) # When insertMany it is an array so add [] order is ([{}])
output:
{
  acknowledged: true, # successful
  insertedIds: {
    '0': ObjectId('688f2640823b363a81e4eef1'),
    '1': ObjectId('688f2640823b363a81e4eef2')
  }
}
# added two documents and their unique objectID was created
```

**IMPORTANT INFO**

- When inserting a document if the primary key is more than one word or requires a special symbol (e.g. -) it will need to be stored within " ".

Example:

```
db.academy.insertOne({"course name": "Engineering", "length": 10})
# here course name is two words and must be wrapped in " "
```

If you go into the academy collections there will now be 3 documents.

## Data modelling

Mongodb is flexible not much structure but we still want relationships between data. There are multiple ways to do this.

### Embedding

Create subdocuments inside a larger documents.
**This is for denormalised data**
Advantages:

- One-to-one relationships
- One-to-Many relationships
- Offers better performance on read and update operations as it will only find one document.

![Embedding](embedding.png)

```
db.academy.insertOne({
	name: "David",
  course: "Data engineering",
  trainer: {
    name: "Luke",
    expertise: "Data"
  }
})

output:
{
  acknowledged: true,
  insertedId: ObjectId('688f2d70823b363a81e4eef4')
}
```

![MongoDB](embeddingMongo.png)

### Referencing

**Normalisated data** as it requires an \_id.

It uses seperate documents and references the other documents, almost like using a primary key as a foreign key for another document.

- Must use for Many-to-Many

#### Advantages

- Avoiding data duplication
- Redundancy

#### Disadvantages

- Database is less efficient
- Effects read performance

![referencing](reference.png)

### Validation

A set of rules to prevent documents being added that dont follow the rules.

1. Create a new collection

```
db.createCollection("students")
{ ok: 1 }
```

2. Go into the collection and onto the validation tab

3. Click generate rules and add new rules

4. Change validation level

- Moderate will update rules for new entries, will not check in existing entries
- Strict will apply the rules for all.

5. Check for validation input updates in Mongosh

```
db.getCollectionInfos({name: "students"})

output:
[
  {
    name: 'students',
    type: 'collection',
    options: {
      validator: [Object],
      validationLevel: 'strict',
      validationAction: 'error'
    },
    info: {
      readOnly: false,
      uuid: UUID('d162335b-52db-44cf-978e-02bda8710a37')
    },
    idIndex: { v: 2, key: [Object], name: '_id_' }
  }
]
```

**Import data information**
Mongosh treats all numerical data as doubles (0.00) to enter a long number wrap it in numberlong OR numberint.

```
db.students.insertOne({
  name: "Mr S. Global",
  course: "Data",
	year: NumberInt(2020),
  score: 88.2,
  address: {
    city: "Birmingham"
  }
})
output:
{
  acknowledged: true,
  insertedId: ObjectId('688f34d8823b363a81e4eef7')
}
```

## Useful code

### Show collections

```
show collections is a mongosh command, it lists the collections in a reader friendly way

output:
academy
firdaws
insert
students
```

### db.getCollectionNames

getCollectionNames is a JavaScript method. It is a referrence to the function and prints out the function object.

```
db.getCollectionNames

output:
[Function: getCollectionNames] AsyncFunction {
  apiVersions: [ 1, Infinity ],
  returnsPromise: true,
  serverVersions: [ '0.0.0', '999.999.999' ],
  topologies: [ 'ReplSet', 'Sharded', 'LoadBalanced', 'Standalone' ],
  returnType: { type: 'unknown', attributes: {} },
  deprecated: false,
  platforms: [ 'Compass', 'Browser', 'CLI' ],
  isDirectShellCommand: false,
  acceptsRawInput: false,
  shellCommandCompleter: undefined,
  newShellCommandCompleter: undefined,
  help: [Function (anonymous)] Help
}
```

### db.getCollectionNames()

Also a javascript function, however this calls the function and prints an array of the collection names rather than the object.

```
db.getCollectionNames()
output:
[ 'academy', 'insert', 'students', 'firdaws' ]
```

## Get a specific document

```
db.students.find({})
output:
{
  _id: ObjectId('688f34d8823b363a81e4eef7'), # Each
  name: 'Mr S. Global',
  course: 'Data',
  year: 2020,
  score: 88.2,
  address: {
    city: 'Birmingham'
  }
}
```

Only have one document within the starta collection

## Find specific fields in a document

1. Create a new database called starwars

```
use starwars
```

2. Create a new collection

```
db.createCollection("characters")
```

3. Add data using a json file

4. Check for values

```
db.characters.find({name: "Chewbacca"})
output:
<all details relating to Chewbacca>
```

5. Specify fields

```
db.characters.find({name: "Chewbacca"}, {name: 1, eye_color: 1})
# name: 1 and eye_color: 1 indicates we want this field to show

output:
{
  _id: ObjectId('688fc0490f1f773466c77018'),
  name: 'Chewbacca',
  eye_color: 'blue'
}

# Object id will always show, to hide this we can do _id: 0 which is false

output:
{
  name: 'Chewbacca',
  eye_color: 'blue'
}
```

## Retrieve fields from embedded documents

```
db.characters.find({name: "Ackbar"}, {_id: 0, name: 1, "species.name": 1})
# For embedding when we want data inside subdocuments we need " " and to specify using .field
output:
{
  name: 'Ackbar',
  species: {
    name: 'Mon Calamari'
  }
}
```

## Find same field in multiple docs

```
# Find all humans in an embeded document
db.characters.find({
  "species.name": "Human"
},
{
  name: 1, #show name
  "homeworld.name": 1, # show the homeworld from embedded document
  _id: 0 # everything is set to false but _id
})
output:
<list of all humans in the database and what we specified above>
```

## When looking for different values

```
# where character has eye_color of yellow or orange
db.characters.find({
  eye_color: {
    $in: ["yellow", "orange"]
  }
},
{
	name: 1 # show only the name
})
output:
<All documents in the database where the character has the eye colour specified above>
```

## Use AND to find documents with both fields

```
# $and requires a []
db.characters.find({
  $and: [{"gender": "female"},
         {"eye_color":
    "blue"}]
  },
  {
    name: 1,
    gender:1
  }
)
output:
<list of all documents with females with blue eyes>
```

## Use OR to find documents with both fields

```
db.characters.find({
    $or: [{gender: "female",
        eye_color: "blue}]},
        {name: 1}
    )
output:
<list of all documents with females or documents with blue eyes as a value>
```

## Comparison operators

### Greater than

```
# $gt is greater than
db.characters.find({
  height:
  {
    $gt: "200"
  }
})
output:
<List of characters where height is greater than 200>
```

## Aggregation: $convert

```
# Update many where height is type string
db.characters.updateMany({
  height: {$type: "string"} #filter
},
[{
$set: {
  height: {
    $convert: {
      input: "$height", #converts where height is a string to an int
      to: "int",
        onError: "unknown", # if there are errors the value will be changed to unknown
        onNull: null # place if there are null values
    }
  }
}
}
])
output:
<check the column documents to see they have changed>
```

### Or we can check in mongosh

```
db.characters.find({
  height:{
    $gt: 200
  }
}, {name: 1, height: 1})
output:
<list of documents where height is above 200. Showing their names and heights>
```

**Key information**

- $convert is an aggregation function so [ ] are required
- In aggregation when we want to refer to a field we need "$". Without the $ mongo would treat this as a string rather than a field.

## Experiment with Operators

### $eq

Equals

```
db.characters.find({
  height:{
    $eq: 202
  }
}, {name: 1, height: 1})
output:
# all documents with a height equal to 202
{
  _id: ObjectId('688fc0490f1f773466c77019'),
  name: 'Darth Vader',
  height: 202
}
```

### gt

Greater than

```
db.characters.find({
  height:{
    $gt: 202
  }
}, {name: 1, height: 1})
output:
<lists all documents with height greater than 202>
```

### $gte

Greater than or equals to

```
db.characters.find({
  height:{
    $gte: 202
  }
}, {name: 1, height: 1})
output:
<lists all documents with height greater than or equals to 202>
```

### $lt

Less than.

```
db.characters.find({
  height:{
    $lt: 160
  }
}, {name:1, height: 1})
output:
<lists all documents with height less than or equals to 160>
```

### $lte

```
db.characters.find({
  height:{
    $lte: 160
  }
}, {name:1, height: 1})
output:
<lists all documents with height less than or equals to 160>
```

### $ne

Not equal to

```
db.characters.find({
  height:{
    $ne: 200
  }
}, {name:1, height:1})
output:
<lists all documents with height not equals to 200>
```

### $in

In is used when looking for something specific in a field. Hair color in brown or blond

```
db.characters.find({
  hair_color:{
    $in: [
      "brown", "blond"
    ]
  }
}, {name:1, hair_color: 1})
output:
<id, name and hair colour of documents with brown or blond hair>
```

### $nin

Not in
$nin selects the documents where:

- The specified field value is not in the specified array or
- The specified field does not exist.

```
db.characters.find({
  eye_color:{
    $nin:[
      "yellow", "orange", "blue"
    ]
  }
}, {name:1, eye_color: 1})
output:
<All documents where the eye colour is NOT yellow, orange or blue>
```

## .aggregation

```
db.characters.aggregate([
  {$match: {"species.name": "Human"}},
  {$group: {_id: null, total: {$sum: "$height"}}}
])
output:
{
  _id: null,
  total: 5476
}
# The total height of every human in the database currently.
```

### Another example of aggregation

Aggregation here contains two stages

```
#$match by species being human
db.characters.aggregate([
  {$match: {"species.name": "Human"}},
  #$group by total sum of each gender
  {$group: {_id: "$gender", total: {$sum: "$height"}}}
])
output:
{
  _id: 'male',
  total: 4194
}
{  _id: 'female',
  total: 1282
}
# Output shows total sum grouped by gender only in human species
```

### .aggregate with $group

```
db.characters.aggregate([{
    # $max returns the max value
  $group: {_id: "$homeworld.name", max: {$max: "$height"}}
}])
output:
<groups all documents using homeworld, for their max height>
```

## Activities

1. Insert data about yourself into sparta database

```
# First create a collection within the database
db.createCollection("firdaws")
{ ok: 1 }

# Second create a document to store information inside
db.firdaws.insertOne({"Full name": "Firdaws Yasmin", "Favourite movie": "Howls moving castle", City: "Sheffield"})

output:
{
  acknowledged: true,
  insertedId: ObjectId('688f2b05823b363a81e4eef3')
}
```

Go into the collection and click into import data to upload a json.

2. Create a collection to store information about your favourite films. Add appropraite validation rules, then insert at least 3 documents. Practice using both .insertOne() and .insertMany().

```
# Create a movies database
use movies

# Create a collection
db.createCollection("movieList")

# Insert one document
db.movieList.insertOne({"name": "Spirited Away", "director": "Ghibli", "year": 2001})

output:
{
  acknowledged: true,
  insertedId: ObjectId('688f77aaf257b55d2da81453')
}

# Insert many documents
db.movieList.insertMany([{"name": "Midsommar", "director": "Ari Aster", "year": 2019}, {"name": "Lights out", "director": "David F. Sandberg", "year": 2016}])

output:
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('688f7a3df257b55d2da81454'),
    '1': ObjectId('688f7a3df257b55d2da81455')
  }
}
```

3. Update one existing document using Mongosh

```
# Find the document we want to update
db.movieList.find({"name": "Spirited away"})
output:
{
  _id: ObjectId('688f77aaf257b55d2da81453'),
  name: 'Spirited Away',
  director: 'Ghibli',
  year: 2001
}

#update one of the documents
db.movieList.updateOne({name: "Spirited Away"}, {$set: { director: "Hayao Miyazaki"}})
output:
db.movieList.updateOne({name: "Spirited Away"}, {$set: { director: "Hayao Miyazaki"}})
```

4. Update many exisiting documents using Mongosh

```
db.movieList.updateMany({}, {$set: {rating: "5 Stars"}})
output:
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 3,
  modifiedCount: 3,
  upsertedCount: 0
}

# check to see if updated
db.movieList.find({})
output:
{
  _id: ObjectId('688f77aaf257b55d2da81453'),
  name: 'Spirited Away',
  director: 'Hayao Miyazaki',
  year: 2001,
  rating: '5 Stars'
}
{
  _id: ObjectId('688f7a3df257b55d2da81454'),
  name: 'Midsommar',
  director: 'Ari Aster',
  year: 2019,
  rating: '5 Stars'
}
{
  _id: ObjectId('688f7a3df257b55d2da81455'),
  name: 'Lights out',
  director: 'David F. Sandberg',
  year: 2016,
  rating: '5 Stars'
}
```

5. Delete a document using Mongosh

```
# use the deleteOne({}) method
db.movieList.deleteOne({name: "Lights out"})

output:
{
  acknowledged: true,
  deletedCount: 1
}

# check it has been deleted
db.movieList.find({})
{
  _id: ObjectId('688f77aaf257b55d2da81453'),
  name: 'Spirited Away',
  director: 'Hayao Miyazaki',
  year: 2001,
  rating: '5 Stars'
}
{
  _id: ObjectId('688f77aaf257b55d2da81453'),
  name: 'Spirited Away',
  director: 'Hayao Miyazaki',
  year: 2001,
  rating: '5 Stars'
}
# only two documents in our movieList collection
```

6. Use AND and OR to find Female characters with blue eyes

```
# $and requires a []
db.characters.find({
  $and: [{"gender": "female"},
         {"eye_color":
    "blue"}]
  },
  {
    name: 1,
    gender:1
  }
)
output:
<list of all documents with females with blue eyes>
```

7. Find male characters with yellow eyes

```
db.characters.find({
  $and[{gender: "male",
       eye_color: "yellow"}]
}, {name: 1})
output:
< All documents with men with yellow eyes>
```

### Aggregation Question

8. Convert height string to int

```
# Update many where height is type string
db.characters.updateMany({
  height: {$type: "string"} #filter
},
[{
$set: {
  height: {
    $convert: {
      input: "$height", #converts where height is a string to an int
      to: "int",
        onError: "unknown", # if there are errors the value will be changed to unknown
        onNull: null # place if there are null values
    }
  }
}
}
])
```

#### Key information

- $convert is an aggregation function so [ ] are required
- In aggregation when we want to refer to a field we need "$". Without the $ mongo would treat this as a string rather than a field.

9. Convert mass field into double

```
db.characters.updateMany({
  mass: {$type: "string"}
}, [{
  $set:{
    mass:{
      $convert: {
        input: "$mass",
        to: "double",
        onError: "unknown",
        onNull: null
      }
    }
  }
}], {name:1, mass:1})

#check if its worked
db.characters.find({
  mass:{
    $lt: 180
  }
}, {name:1, mass: 1})
output:
<list documents where mass is less than 180 so this is now a numerical data type>
```

# Introduction to the MongoDB shell
Once we have a running `mongod` instance, we can connect to the MongoDB shell running the `mongo` command:

```
$ mongo
MongoDB shell version: 3.0.0
connecting to: test
>
```

Here we'll see some basic commands to get familiar with the mongo shell. Don't forget to check the [Getting Started Guide][2], especially the [MongoDB Shell(mongo)][3] section. After that, nothing better than the [manual][4].

## Importing a dataset
Could be a good idea to import an existing dataset to get practice with the mongo shell. The [MongoDB][1] guys have made available a dataset for practice at [github][5], just download it and rename it as `primer-dataset.json`. Now we can import it running:

```bash
$ mongoimport --db test --collection restaurants --drop --file primer-dataset.json
```

Let's explain the options we've used:

* `--db` for designate a database (It doesn't have to exist).
* `--collection` to save as a collection named `restaurants`. (It shouldn't exist).
* `--drop`, if there's already a collection named `restaurants`, drop it.
* `--file` to point the file we want to import.

The `mongoimport` command connects to a `mongod` instance running on localhost on port number `27017`. To import data into a `mongod` instance running on a different host or port, specify the hostname or port by including the `--host` and the `--port` options in your `mongoimport` command.

## Commands to manage databases
Once we start a shell session, there are some basic operations we'll want to be familiar with, such as:

* What database are we working on.
* List all the databases we have on our system.
* How to start using an existing database.
* How to create and delete a database.

### What is the current database
Since the [MongoDB][1] shell prompt is so minimalistic, at some point we may want to know what database we are connected to. Just run `db`:

```
> db
test
```

Once we start a shell session, by default we are always connected to the `test` database. We can specify a given database when calling the `mongo` command:

```bash
$ mongo mydb
```

It doesn't matter if the database doesn't exist, it will be created for us.

### Listing databases
To **list** all the databases we have in our system, we run:
```
> show dbs
local  0.078GB
test   0.078GB
```

> `show databases` is an alias.

### Creating or switching to a database
The `use` command takes one argument, a database name. Depending on the existence of the database, `use` can perform 2 actions:

1. Select the database, if the name we are passing as an argument corresponds to an existing database.
2. Otherwise it will create a new database. But this database is temporary, we'll have to insert some document to make it permanent.

For example, to **switch** to an existing database we run:
```
> use demo
switched to db demo
```

And to create a new one:
```
> use mydb
switched to db mydb
```

### Deleting a database
To **delete** the database in use run `db.dropDatabase()`. Note that this command doesn't take arguments. Be careful, because it will delete the whole database, with all of its contents.

## Commands to manage documents and collections

### Creating a document
Once we have selected a database, we can create a document using the `insert()` method. For example:

```
> db.employes.insert({
...   "name": "Bob",
...   "age": 40,
...   "address": {
...     "street": "1st Street",
...     "city": "L.A.",
...     "state": "California",
...     "zip": 12345
...   }
... })
```
The above operation inserts a document into a collection named `things`. The operation will create the collection if it does not currently exist. The method returns a `WriteResult` object with the status of the operation:
```
WriteResult({ "nInserted" : 1 })
```
### Querying a collection
You can use the `find()` method to issue a query to retrieve data from a collection. All queries in MongoDB have the scope of a single collection.

* To return **all documents** in a collection, call the `find()` method without a criteria document. For example:

```
> db.employees.find()
{
  "_id": ObjectId("56684b7b23578a81d1bf9b32"),
  "name": "Bob",
  "age": 40,
  "address": {
    "street": "1st Street",
    "city": "L.A.",
    "state": "California",
    "zip": 12345
  }
}
```

Notice that when we created the document before, we didn't passed to the `insert()` method the `_id` field; the mongo shell automatically adds this field to the document and sets its value to a generated `ObjectId`.

* We can also specify equality conditions as an argument to `find()`, for example:

```
> db.employees.find({ "name": "Bob" })
```

If the searched field is a **top-level field** and not a field in an embedded document or an array, you can either enclose the field name in quotes or omit the quotes. So we can write the above method as:

```
> db.employees.find({ name: "Bob" })
```

* If the searched field is in an embedded document or an array, use **dot notation** to access the field. With dot notation, you must enclose the dotted name in quotes. For example:

```
> db.employees.find({ "address.city": "Bob" })
```

### Updating a document
We can use the `update()` method to update documents of a collection. By default, the `update()` method updates a single document, but we can pass a `multi` option to update all documents that match the criteria. We cannot update the `_id` field.

### Listing Collections
To list the collections in the database in use we run `show collections`. An equivalent command is `show tables`. Another one is `db.getCollectionNames()`, but this returns our collections in array literal form.

### Removing Collections
To remove a collection from the database we use `db.collection.drop()`. This method also removes any indexes associated with the dropped collection. For example:

```
> db.employees.drop()
true
```
If the command is successful the return value is `true`.

---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: configuration.md
[next]: #

<!-- links -->
[1]: https://www.mongodb.org/
[2]: https://docs.mongodb.org/getting-started/shell/
[3]: https://docs.mongodb.org/getting-started/shell/client/
[4]: https://docs.mongodb.org/manual/
[5]: https://raw.githubusercontent.com/mongodb/docs-assets/primer-dataset/dataset.json

# Introduction to MongoDB
[MongoDB][1] is an open source, document database management system. The keyword here being **document**, what means that in [MongoDB][1] there are **no tables** related to each other, and henceforth we can't use [SQL][2] to manage the database. That's why [MongoDB][1] is also known as a [NoSQL][3] database.

## Documents
[MongoDB][1] stores its records in documents. A document is a data structure composed of field and value pairs. The values of these fields may include other documents, arrays, and arrays of documents.

[MongoDB][1] documents use a format named [BSON][5], which stands for **Binary JSON**. The [BSON][5] format is a bin­ary-en­coded seri­al­iz­a­tion of [JSON][4]-like doc­u­ments.

## Collections
[MongoDB][1] groups **documents** in collections. **Collections** are analogous to tables in relational databases. Unlike a table, however, a collection does not require its documents to have the same schema, that's why [MongoDB][1] is characterized as being schema-less.

In MongoDB, **documents** stored in a collection must have a unique `_id` field that acts as a primary key. The `_id` field is usually an **ObjectId**, which is a special 12-byte BSON data type that guarantees uniqueness within the collection.



---
[:arrow_backward:][back] ║ [:house:][home] ║ [:arrow_forward:][next]

<!-- navigation -->
[home]: ../README.md
[back]: ../README.md
[next]: installing.md

<!-- links -->
[1]: https://www.mongodb.org/
[2]: https://en.wikipedia.org/wiki/SQL
[3]: https://en.wikipedia.org/wiki/NoSQL
[4]: http://json.org/
[5]: http://bsonspec.org/

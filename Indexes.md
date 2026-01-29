📘 MongoDB Indexes – Complete In‑Depth Guide

This document is created to be part of your MongoDB repository. It explains MongoDB Indexes from basics to advanced level, including internal working, types of indexes, syntax, examples, performance considerations, and best practices.


🔷 1. What is an Index in MongoDB?

An index in MongoDB is a special data structure that stores a small portion of a collection’s data in an ordered, easy‑to‑search form.
Indexes help MongoDB find documents quickly without scanning the entire collection.
Without index → Full collection scan (slow) With index → Index scan (fast)
MongoDB mainly uses B‑tree based indexes which keep data sorted and allow efficient searching, sorting, and range queries.


🔷 2. Why Indexes are Important

Indexes significantly improve:
Query performance
Sorting speed
Filtering operations
Join performance ($lookup)
Uniqueness enforcement
However, indexes also:
Take extra storage
Slow down insert/update/delete operations
So, indexes should be designed carefully based on application queries.


🔷 3. Default Index (_id)

Every MongoDB collection automatically contains an index on the _id field.

Properties:
Unique
Cannot be deleted
Ensures fast document lookup

Example:

db.users.find({ _id: ObjectId("...") })


🔷 4. How Indexes Work Internally

MongoDB stores index data in a B‑tree structure.

This allows:
O(log n) search complexity
Efficient range queries
Fast sorting
Each index stores:
Indexed field value
Pointer to the actual document


🔷 5. Creating Indexes
▶ Single Field Index
db.users.createIndex({ name: 1 })

1 → ascending order -1 → descending order

▶ Compound Index

Index on multiple fields:

db.users.createIndex({ age: 1, city: 1 })

Important rule: Prefix Rule

This index supports:

age

age + city

But NOT:

city alone



🔷 6. Types of Indexes in MongoDB

✅ 6.1 Single Field Index

Index on one field.

Use when filtering frequently on one field.

✅ 6.2 Compound Index

Index on multiple fields.

Used when queries include multiple conditions.

Example:

db.orders.createIndex({ userId: 1, createdAt: -1 })

✅ 6.3 Multikey Index

Automatically created when indexing an array field.

Example:

db.students.createIndex({ skills: 1 })

Supports searching inside arrays.

✅ 6.4 Text Index

Used for text searching.

db.posts.createIndex({ title: "text", content: "text" })

Search query:

db.posts.find({ $text: { $search: "mongodb index" } })

✅ 6.5 Hashed Index

Used for hash‑based equality lookups.

db.users.createIndex({ email: "hashed" })

Mostly used in sharding.

✅ 6.6 Geospatial Index

Used for location‑based queries.

db.places.createIndex({ location: "2dsphere" })

Supports:

$near

$geoWithin

$geoIntersects

✅ 6.7 Wildcard Index

Indexes unknown or dynamic fields.

db.logs.createIndex({ "data.$**": 1 })

Used in flexible schemas.
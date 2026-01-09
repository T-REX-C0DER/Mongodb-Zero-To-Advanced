# 📘 MongoDB CRUD Operations – Complete Guide

CRUD stands for **Create, Read, Update, Delete** — the four fundamental operations used to work with data in any database system.

In MongoDB, CRUD operations are performed on **collections** and deal with **documents (BSON objects)**.

This guide explains CRUD operations in full detail, including syntax, variations, options, real examples, and best practices.

# 📌 Prerequisites

Make sure you have:
- MongoDB installed  
- Mongo Shell (`mongosh`) or MongoDB Compass  
- A running MongoDB server  

Start MongoDB shell:

```bash
mongosh
Select / create a database:
use societyDB
Create or switch to a collection:
db.residents


🟢 CREATE – Insert Operations
Used to add new documents into a collection.

✅ insertOne()
Inserts one document.

db.residents.insertOne({
  name: "Amit Sharma",
  age: 22,
  flatNo: "A-203",
  phone: "9876543210",
  active: true
})

✅ insertMany()
Inserts multiple documents at once.

db.residents.insertMany([
  { name: "Neha", age: 21, flatNo: "B-101" },
  { name: "Sanjay", age: 24, flatNo: "C-202" },
  { name: "Priya", age: 23, flatNo: "A-105" }
])

✅ Ordered vs Unordered Inserts
db.residents.insertMany([...], { ordered: false })

ordered: true → stops on first error
ordered: false → continues even if one fails


🔵 READ – Find Operations
Used to retrieve documents.

✅ find()
db.residents.find()

Pretty format:
db.residents.find().pretty()

✅ findOne()
db.residents.findOne({ name: "Amit Sharma" })

✅ Query Filters
db.residents.find({ age: 22 })
db.residents.find({ age: { $gt: 21 } })
db.residents.find({ age: { $lt: 25 } })
db.residents.find({ age: { $gte: 22 } })
db.residents.find({ age: { $ne: 20 } })

✅ Logical Operators
db.residents.find({
  $and: [{ age: { $gt: 20 } }, { active: true }]
})

db.residents.find({
  $or: [{ flatNo: "A-203" }, { flatNo: "B-101" }]
})

✅ Projection (Select Fields)
db.residents.find({}, { name: 1, flatNo: 1, _id: 0 })

✅ Sorting
db.residents.find().sort({ age: 1 })
db.residents.find().sort({ age: -1 })

✅ Limit & Skip
db.residents.find().limit(3)
db.residents.find().skip(2)

🟡 UPDATE – Modify Operations

Used to change existing documents.

✅ updateOne()
db.residents.updateOne(
  { name: "Amit Sharma" },
  { $set: { age: 23 } }
)

✅ updateMany()
db.residents.updateMany(
  { active: true },
  { $set: { verified: true } }
)

✅ replaceOne()
db.residents.replaceOne(
  { name: "Neha" },
  { name: "Neha Singh", age: 22, flatNo: "B-101" }
)


⚠ This replaces the full document except _id.
✅ Update Operators
$set, $unset, $inc, $push, $pull, $rename

Example:

db.residents.updateOne(
  { name: "Rahul" },
  {
    $inc: { age: 1 },
    $set: { active: true }
  }
)

✅ Upsert (Update or Insert)
db.residents.updateOne(
  { name: "Sonal" },
  { $set: { age: 21, flatNo: "D-404" } },
  { upsert: true }
)



🔴 DELETE – Remove Operations
Used to remove documents.

✅ deleteOne()
db.residents.deleteOne({ name: "Rahul" })

✅ deleteMany()
db.residents.deleteMany({ active: false })

✅ Delete All Documents
db.residents.deleteMany({})

✅ Drop Collection
db.residents.drop()

📦 Bulk Operations
db.residents.bulkWrite([
  { insertOne: { document: { name: "Karan", age: 22 } } },
  { updateOne: { filter: { name: "Amit Sharma" }, update: { $set: { age: 24 } } } },
  { deleteOne: { filter: { name: "Neha Singh" } } }
])

🛡 Best Practices

Always use filters in update and delete

Avoid replaceOne unless necessary

Use projections to optimize queries

Test delete queries with find() first

Use indexes for better performance

🎯 Real-World CRUD Mapping

Create → Registration
Read → Dashboard / Search
Update → Profile update
Delete → Account removal

📌 Summary

CRUD operations are the foundation of MongoDB.
Mastering them enables real-world backend, database, and system design projects.
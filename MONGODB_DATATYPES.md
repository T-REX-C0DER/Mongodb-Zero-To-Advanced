# 📘 MongoDB Data Types — Complete & Practical Guide

> A structured and practical reference to MongoDB (BSON) data types, including how to store them, real-world usage, and date & time handling.

---

## 🔹 Introduction

MongoDB stores data in **BSON (Binary JSON)** format.  
Every field in a MongoDB document has a **specific data type** that defines how the value is stored, processed, indexed, and compared.

Using correct data types is essential for:
- Optimized storage and performance  
- Accurate calculations and sorting  
- Reliable querying and filtering  
- Clean and scalable database design  

Although MongoDB is schema-less, **data types still play a critical role** in real-world applications.

---

## 📦 Core Data Types

### 

```js

1. String  
Used to store text values.
{name: "Sanjay Lade"}

2. Numbers
MongoDB supports multiple numeric types.

Double (default)
{price: 199.99}
Integer (32-bit)

{age: NumberInt(20)}
Long (64-bit)

{views: NumberLong(9000000000)}
Decimal (high precision – finance)

{balance: NumberDecimal("10500.75")}
Use NumberDecimal for currency and scientific values.


3. Boolean
Stores true/false.
{isActive: true}


4. Null
Represents an empty or unknown value.
{middleName: null}
🧱 Structural Data Types


5. Object (Embedded Document)
{
  name: "Laptop",
  specs: {
    ram: "16GB",
    storage: "512GB",
    processor: "i7"
  }
}
Used for logically related data.


6. Array
{skills: ["Python", "MongoDB", "Machine Learning"]}
Array of objects:
{marks: [{subject: "AI", score: 95}, {subject: "DBMS", score: 90}]}
Arrays allow powerful querying and indexing.


🆔 Special Data Types
7. ObjectId
Default unique identifier.

{_id: ObjectId()}
Automatically generated primary key.


8. Date (Date & Time)
MongoDB stores date and time together in UTC.
Current timestamp:
{createdAt: new Date()}

Custom date:
{dob: new Date("2005-08-15")}

ISO format:
{loginTime: ISODate("2026-01-13T18:30:00Z")}
MongoDB does not support a separate “time-only” type.


9. Timestamp
Used mainly for internal and event-tracking systems.
{logTime: Timestamp(1705160000, 1)}


10. Binary Data
{file: BinData(0, "U29tZUJpbmFyeURhdGE=")}
For large media files, use GridFS.


11. Regular Expression
{name: /san/i}
Used for pattern matching and text filtering.


12. MinKey & MaxKey
{min: MinKey()}
{max: MaxKey()}
Mostly used internally and for advanced comparisons.



⏰ Working with Date & Time
Insert current date:

db.logs.insertOne({createdAt: new Date()})
Find by date range:

db.logs.find({createdAt: {$gt: new Date("2026-01-01")}})
Store date only:

{eventDate: new Date("2026-02-10T00:00:00Z")}
Extract year using aggregation:

db.logs.aggregate([
  {$project: {year: {$year: "$createdAt"}}}
])



📄 Real-World Example Document
{
  name: "Smart Watch",
  price: 2999.99,
  inStock: true,
  tags: ["electronics", "wearable"],
  manufacturer: {
    brand: "TechGear",
    country: "India"
  },
  ratings: [4, 5, 5, 4],
  launchDate: new Date("2025-12-01"),
  createdAt: new Date(),
  productId: ObjectId()
}

📘 MongoDB Operators – Complete Guide

MongoDB operators are special symbols/keywords used to perform operations on data such as comparison, logical checks, updates, array handling, and aggregation processing.

They are mainly used in:
find() (querying)
updateOne(), updateMany() (updating)
Aggregation pipeline ($match, $group, etc.)


🔹 1. Comparison Operators
Used to compare values.

$eq	 = Equal
$ne	 = Not equal
$gt	= Greater than
$gte = Greater than or equal
$lt	= Less than
$lte =	Less than or equal
$in	= Match any value in array
$nin =	Not in array

✅ Examples
db.students.find({ age: { $gt: 18 } })
db.students.find({ grade: { $in: ["A", "B"] } })



🔹 2. Logical Operators
Used to apply multiple conditions.

$and =	All conditions true
$or	= Any condition true
$not = Negates condition
$nor =	None true

✅ Examples
db.students.find({
  $and: [
    { age: { $gt: 18 } },
    { city: "Mumbai" }
  ]
})

db.students.find({
  $or: [
    { course: "AI" },
    { course: "DS" }
  ]
})



🔹 3. Element Operators
Used to check field existence and type.

$exists	= Field exists or not
$type =	Field data type

✅ Examples
db.students.find({ email: { $exists: true } })
db.students.find({ age: { $type: "int" } })



🔹 4. Evaluation Operators
Used to evaluate expressions.

$regex = Pattern matching
$expr =	Use aggregation expressions
$mod =	Modulus
$text =	Text search
$where = JS condition (avoid in production)

✅ Examples
db.students.find({ name: { $regex: "^S" } })
db.students.find({
  $expr: { $gt: ["$marks", "$attendance"] }
})



🔹 5. Array Operators
Used when fields contain arrays.

$all = Must contain all values
$size =	Exact array length
$elemMatch = Match object inside array

✅ Examples
db.students.find({ skills: { $all: ["Python", "MongoDB"] } })
db.students.find({ skills: { $size: 3 } })
db.students.find({
  scores: { $elemMatch: { subject: "Math", marks: { $gt: 70 } } }
})



🔹 6. Update Operators
Used to modify documents.

$set =	Add/update field
$unset =	Remove field
$rename	 = Rename field
$inc =	Increment
$mul =	Multiply
$min / $max	Set min/max value
$currentDate =	Current date/time

✅ Examples
db.students.updateOne(
  { name: "Amit" },
  { $set: { city: "Pune" } }
)
db.students.updateMany(
  {},
  { $inc: { loginCount: 1 } }
)


📦 Array Update Operators

$push =	Add element
$pull =	Remove element
$pop =	Remove first/last
$addToSet =	Add if not exists
$each =	Add multiple
$position =	Insert at index

✅ Examples
db.students.updateOne(
  { name: "Riya" },
  { $push: { skills: "NodeJS" } }
)
db.students.updateOne(
  { name: "Riya" },
  { $addToSet: { skills: "MongoDB" } }
)



🔹 7. Aggregation Operators (Most Powerful)
Used inside aggregation pipeline.

$match =	Filter
$group =	Group & calculate
$project =	Select/transform
$sort =	Order
$limit =	Limit
$skip =	Skip
$lookup =	Join
$unwind =	Break array
$count =	Count docs

✅ Example
db.students.aggregate([
  { $match: { course: "AI" } },
  { $group: { _id: "$city", total: { $sum: 1 } } },
  { $sort: { total: -1 } }
])



🧮 Aggregation Expression Operators
➗ Arithmetic
$sum, $avg, $min, $max, $multiply, $subtract

🔤 String
$concat, $toUpper, $toLower, $substr

⏰ Date
$year, $month, $dayOfMonth, $hour

🔍 Conditional
$cond, $ifNull, $switch


🔹 8. Bitwise Operators

$bitsAllSet = 	All bits set
$bitsAnySet =	Any bit set
$bitsAllClear =	All clear
$bitsAnyClear =	Any clear
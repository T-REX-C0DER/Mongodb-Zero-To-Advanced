🔥 MongoDB Aggregation Pipeline 

The Aggregation Pipeline is MongoDB’s powerful framework for data processing, transformation, analytics, and reporting.
It works like a data conveyor belt:
Documents enter → pass through multiple stages → get transformed → final output is produced.

Each stage performs a specific operation such as:
Filtering
Grouping
Sorting
Reshaping
Calculating fields
Joining collections

⚙️ Basic Syntax
db.collection.aggregate([
  { stage1 },
  { stage2 },
  { stage3 },
  ...
])

Each stage starts with $ and receives the output of the previous stage.


🧠 Why Aggregation Pipeline is Important

Performs complex analytics
Used for dashboards & reports
Reduces backend logics big data efficiently
Essential for AI, ML, and business insights
Works inside the database → faster processing

📦 Sample Data (For Practice)
db.students.insertMany([
  { name: "Amit", dept: "AI", marks: 85, age: 21, city: "Pune" },
  { name: "Neha", dept: "DS", marks: 92, age: 22, city: "Mumbai" },
  { name: "Rohit", dept: "AI", marks: 78, age: 20, city: "Delhi" },
  { name: "Priya", dept: "DS", marks: 88, age: 21, city: "Pune" },
  { name: "Karan", dept: "AI", marks: 95, age: 23, city: "Mumbai" }
])


🧩 Core Aggregation Stages (Most Important)
1️⃣ $match — Filtering Documents

Filters documents like find().
db.students.aggregate([
  { $match: { dept: "AI", marks: { $gt: 80 } } }
])

✔ Used to reduce data
✔ Should be placed early for performance
✔ Supports all query operators



2️⃣ $project — Shape the Output

Select, rename, remove, or create fields.
db.students.aggregate([
  { $project: { name: 1, marks: 1, _id: 0 } }
])


Rename field:
{ $project: { studentName: "$name", score: "$marks" } }

Computed field:
{ $project: { name: 1, result: { $cond: [{ $gte: ["$marks", 80] }, "Pass", "Fail"] } } }

✔ Controls final output
✔ Creates calculated fields
✔ Removes unnecessary data



3️⃣ $group — Group & Aggregate Data

Used for analytics and calculations.
db.students.aggregate([
  {
    $group: {
      _id: "$dept",
      totalStudents: { $sum: 1 },
      avgMarks: { $avg: "$marks" },
      maxMarks: { $max: "$marks" },
      minMarks: { $min: "$marks" }
    }
  }
])


Common accumulators:

$sum
$avg
$min
$max
$push
$addToSet
$first
$last

Example:

{ $group: { _id: "$city", students: { $push: "$name" } } }



4️⃣ $sort — Sorting Results
{ $sort: { marks: -1 } }

✔ 1 = ascending
✔ -1 = descending


5️⃣ $limit — Limit Output
{ $limit: 3 }


6️⃣ $skip — Skip Documents
{ $skip: 2 }


Used for pagination.

7️⃣ $count — Count Documents
{ $count: "totalStudents" }


8️⃣ $sortByCount — Group + Count + Sort
db.students.aggregate([
  { $sortByCount: "$dept" }
])


Equivalent to:

{ $group: { _id: "$dept", count: { $sum: 1 } } },
{ $sort: { count: -1 } }


9️⃣ $unwind — Break Array into Documents

Before:

{ name: "Amit", skills: ["Python", "MongoDB", "ML"] }


After:

{ name: "Amit", skills: "Python" }
{ name: "Amit", skills: "MongoDB" }
{ name: "Amit", skills: "ML" }

{ $unwind: "$skills" }


Used for:
Array processing
Joins
Filtering array values


🔗 $lookup — Join Collections (SQL JOIN)

Students collection:
{ _id: 1, name: "Amit", deptId: 101 }


Departments collection:
{ _id: 101, dept: "AI" }

{
  $lookup: {
    from: "departments",
    localField: "deptId",
    foreignField: "_id",
    as: "departmentDetails"
  }
}


✔ Performs left outer join
✔ Output is always an array

📂 $addFields — Add New Fields
{ $addFields: { status: { $cond: [{ $gte: ["$marks", 80] }, "Excellent", "Good"] } } }

🔄 $replaceRoot — Replace Main Document
{ $replaceRoot: { newRoot: "$departmentDetails" } }

📦 $facet — Multiple Pipelines Together
db.students.aggregate([
  {
    $facet: {
      topStudents: [{ $sort: { marks: -1 } }, { $limit: 2 }],
      cityStats: [{ $group: { _id: "$city", count: { $sum: 1 } } }]
    }
  }
])

✔ Used for dashboards
✔ Returns multiple reports at once


🧮 Arithmetic Operators in Aggregation
{ $project: { total: { $add: ["$marks", 10] } } }
{ $project: { diff: { $subtract: ["$marks", 5] } } }
{ $project: { product: { $multiply: ["$marks", 2] } } }
{ $project: { ratio: { $divide: ["$marks", 100] } } }
{ $project: { mod: { $mod: ["$marks", 2] } } }


🕒 Date Operators
{ $project: { year: { $year: "$createdAt" } } }
{ $project: { month: { $month: "$createdAt" } } }
{ $project: { day: { $dayOfMonth: "$createdAt" } } }


🔍 Conditional Operators
{
  $project: {
    grade: {
      $switch: {
        branches: [
          { case: { $gte: ["$marks", 90] }, then: "A" },
          { case: { $gte: ["$marks", 75] }, then: "B" }
        ],
        default: "C"
      }
    }
  }
}

⚡ Real-World Aggregation Example


👉 Top department by average marks:

db.students.aggregate([
  { $group: { _id: "$dept", avgMarks: { $avg: "$marks" } } },
  { $sort: { avgMarks: -1 } },
  { $limit: 1 }
])


👉 Number of students per city:

db.students.aggregate([
  { $group: { _id: "$city", total: { $sum: 1 } } }
])


👉 Pass/Fail classification:

db.students.aggregate([
  {
    $project: {
      name: 1,
      result: { $cond: [{ $gte: ["$marks", 40] }, "Pass", "Fail"] }
    }
  }
])


🚀 Best Practices (Important for Industry)

Use $match early to reduce data
Use $project to limit fields
Avoid heavy $lookup on large collections
Index fields used in $match
Keep pipelines short and readable
Use $facet for dashboards
Prefer aggregation over backend loops


📌 Where Aggregation is Used

Analytics systems
Admin dashboards
AI data preprocessing
Recommendation engines
Financial reports
Monitoring systems
Society & research management systems
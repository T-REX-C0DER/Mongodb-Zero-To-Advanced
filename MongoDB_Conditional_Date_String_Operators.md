📘 MongoDB Conditional, Date & String Operators — Detailed Guide

This document covers Conditional Operators, Date Operators, and String Operators in MongoDB in a clear, detailed, and practical way.
It is useful for backend development, data processing, and aggregation pipelines.



🔹 1. CONDITIONAL OPERATORS IN MONGODB

Conditional operators are used to apply if–else logic inside queries and aggregation pipelines.
They help to:

Build dynamic conditions
Transform data
Create calculated fields
Handle null or missing values
Mostly used inside aggregation pipelines.

✅ 1.1 $cond — If–Else Operator

Works like a ternary operator.
Syntax:
{
  $cond: { if: <condition>, then: <true_value>, else: <false_value> }
}

Example:
db.students.aggregate([
  {
    $project: {
      name: 1,
      status: {
        $cond: {
          if: { $gte: ["$marks", 40] },
          then: "Pass",
          else: "Fail"
        }
      }
    }
  }
])
📌 If marks ≥ 40 → Pass, else → Fail



✅ 1.2 $ifNull — Handle Null Values

Returns a default value if the field is null or missing.
Syntax:
{ $ifNull: [ <expression>, <replacement> ] }

Example:
{
  $project: {
    email: { $ifNull: ["$email", "Not Provided"] }
  }
}

📌 If email is null → “Not Provided”
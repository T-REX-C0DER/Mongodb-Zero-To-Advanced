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


✅ 1.3 $switch — Multiple Conditions (Like switch-case)
Used when you have many conditions.

{
  $switch: {
    branches: [
      { case: <condition1>, then: <result1> },
      { case: <condition2>, then: <result2> }
    ],
    default: <default_value>
  }
}
Example:
{
  $project: {
    grade: {
      $switch: {
        branches: [
          { case: { $gte: ["$marks", 90] }, then: "A" },
          { case: { $gte: ["$marks", 75] }, then: "B" },
          { case: { $gte: ["$marks", 50] }, then: "C" }
        ],
        default: "Fail"
      }
    }
  }
}



✅ 1.4 $cmp — Compare Two Values
Returns:
1 if first > second
0 if equal
-1 if first < second
{ $cmp: ["$a", "$b"] }



🔹 2. DATE OPERATORS IN MONGODB
Date operators are used to extract, compare, format, and manipulate date/time values.

✅ 2.1 Date Creation
new Date()
ISODate("2025-01-01T10:00:00Z")



✅ 2.2 Extracting Parts from Date

$year =	Extract year
$month =	Extract month
$dayOfMonth	 = Day of month
$hour	= Hour
$minute= 	Minute
$second	= Second
$dayOfWeek	1 (Sunday) – 7 (Saturday)
Example:
{
  $project: {
    year: { $year: "$createdAt" },
    month: { $month: "$createdAt" },
    day: { $dayOfMonth: "$createdAt" }
  }
}



✅ 2.3 $dateToString — Format Date
{
  $dateToString: { format: "%d-%m-%Y", date: "$createdAt" }
}

📌 Output → 18-01-2026



✅ 2.4 $dateFromString — Convert String to Date
{
  $dateFromString: { dateString: "2026-01-18" }
}



✅ 2.5 $add, $subtract with Dates
{
  $project: {
    nextWeek: { $add: ["$date", 7 * 24 * 60 * 60 * 1000] }
  }
}
📌 Adds 7 days.



✅ 2.6 $dateDiff — Difference Between Dates
{
  $dateDiff: {
    startDate: "$start",
    endDate: "$end",
    unit: "day"
  }
}


✅ 2.7 $currentDate — Store Current Date
db.users.updateOne(
  { _id: 1 },
  { $currentDate: { lastLogin: true } }
)



🔹 3. STRING OPERATORS IN MONGODB
String operators are used to modify, format, search, and analyze text data.


✅ 3.1 $concat — Join Strings
{
  $project: {
    fullName: { $concat: ["$firstName", " ", "$lastName"] }
  }
}



✅ 3.2 $toUpper and $toLower
{ $toUpper: "$name" }
{ $toLower: "$email" }



✅ 3.3 $substrBytes / $substrCP
{ $substrBytes: ["$name", 0, 4] }

📌 Extracts substring.


✅ 3.4 $strLenBytes / $strLenCP
{ $strLenCP: "$username" }

📌 Returns string length.


✅ 3.5 $trim, $ltrim, $rtrim
{ $trim: { input: "$name" } }

📌 Removes spaces.


✅ 3.6 $replaceOne and $replaceAll
{
  $replaceAll: {
    input: "$email",
    find: "gmail.com",
    replacement: "company.com"
  }
}


✅ 3.7 $split — Convert String to Array
{ $split: ["$skills", ","] }


✅ 3.8 $regexMatch — Pattern Matching
{
  $regexMatch: {
    input: "$email",
    regex: "@gmail.com$"
  }
}

📌 Returns true/false.

✅ 3.9 $indexOfCP — Find Position
{ $indexOfCP: ["$email", "@"] }

📌 Returns position.

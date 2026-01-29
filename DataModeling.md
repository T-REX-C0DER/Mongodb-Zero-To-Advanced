📘 MongoDB Data Modeling & $lookup Guide
This document is created to be part of your MongoDB repository. It explains MongoDB Data Modeling concepts in depth and gives a complete, practical understanding of the $lookup aggregation stage.


🔹 1. What is Data Modeling in MongoDB?

Data modeling in MongoDB is the process of designing how your data is structured, stored, and related inside collections and documents.

Unlike SQL databases (tables + joins), MongoDB is document‑oriented, so the main goal is:
How to structure documents
When to embed data
When to reference data
How to design for performance, scalability, and maintainability
Good data modeling:
Improves query performance
Reduces complexity
Scales better
Matches real‑world use cases



🔹 2. Core Concepts of MongoDB Data Modeling
▶ Document

A BSON object stored inside a collection.
{
  "_id": 1,
  "name": "Laptop",
  "price": 55000
}

▶ Collection
A group of documents (similar to a table, but schema‑flexible).
▶ Schema Design
Deciding:
Fields
Data types
Relationships
Nesting structure


🔹 3. Types of Relationships in MongoDB

MongoDB mainly supports:

One‑to‑One
One‑to‑Many
Many‑to‑Many

Each can be modeled using:

Embedded documents
Referenced documents



🔹 4. Embedding vs Referencing
✅ Embedded Data Model

Store related data inside a single document.

Example:

{
  "_id": 101,
  "name": "Amit",
  "address": {
    "city": "Pune",
    "pincode": 411001
  }
}
Advantages

Faster reads (no joins)
Atomic updates
Simple queries
Disadvantages
Document size limit (16MB)
Data duplication
Harder updates for repeated data


✅ Referenced Data Model

Store related data in different collections and link using IDs.

Example:

Users collection:

{ "_id": 1, "name": "Amit" }

Orders collection:

{ "_id": 201, "userId": 1, "product": "Mouse" }

Advantages

Avoids duplication
Better for large or growing data
Flexible relationships
Disadvantages
Requires $lookup
Slightly slower reads
More complex queries
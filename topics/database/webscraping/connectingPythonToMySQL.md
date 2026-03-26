# Connecting Python to MySQL Database (mysql.connector)

## 🎯 Purpose
This setup allows a Python program to connect to a MySQL database and execute SQL queries such as inserting, retrieving, or updating data.

---

## 🧩 Required Library
- `mysql.connector`

Install if needed:
```bash
pip install mysql-connector-python
```
## ⚙️ Establishing a Connection

``` 
import mysql.connector

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="root123",
    database="DataCatalog"
)
```
## Explanation
- host → where the database is located (usually localhost)
- user → MySQL username (e.g., root)
- password → your MySQL password
- database → the specific database to use

👉 This creates a connection between Python and MySQL.

## Creating a Cursor
```
cursor = conn.cursor()
```
## 🔍 What is a cursor?
A cursor is used to execute SQL commands
It acts as the interface between Python and the database
## ▶️ Executing SQL Queries
Example: Insert Data
```
cursor.execute("""
INSERT INTO Users (Email, Username, Gender, Age, Birthdate, Country)
VALUES (%s, %s, %s, %s, %s, %s)
""", (email, username, gender, age, birthdate, country))
```

👉 %s is used as a placeholder for values (prevents SQL injection)

## 💾 Saving Changes
```
conn.commit() 
```
- Required after INSERT, UPDATE, DELETE
- Without commit → changes will NOT be saved

## 📖 Retrieving Data
```
cursor.execute("SELECT * FROM Users")
results = cursor.fetchall()

for row in results:
    print(row)
```
## 🔒 Closing Connection
```
cursor.close()
conn.close()
```

👉 Always close to free resources

## ⚠️ Common Errors
- ❌ Access denied
    - Wrong username or password
- ❌ Unknown database
    - Database name is incorrect
- ❌ Connection refused
    - MySQL server is not running
## 🧠 Key Concepts
- Connection → establishes link to database
- Cursor → executes SQL queries
- commit() → saves changes
- execute() → runs SQL commands
## 🧩 Summary

Python → connects → MySQL
Cursor → executes SQL
Commit → saves data

## 🚀 Usage in Database Course Project

This setup is used to:

- Load users.csv into the Users table
- Insert crawled dataset data
- Generate and insert Faker usage records


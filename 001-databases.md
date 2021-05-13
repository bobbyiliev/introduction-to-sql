# Databases

Before we dive deep into SQL, let's quickly define what a database is.

The definition of databases from Wikipedia is:

> A database is an organized collection of data, generally stored and accessed electronically from a computer system.

In other words, a database is a collection of data sroted and structured in different database tables.

## Tables and columns

You've mostlikely worked with spreadsheet systems like Excel or Google Sheets. At the very basic, databases tables are quite similar to spreadsheets.

Each table has different **columns** which could contain different types of data.

For exmaple, if you have an todo list app, you would have a database and in your database you would have different tables storing different information like:

* Users - In the users table you would have some data for your users like: `username`, `name`, and `active` for example.
* Tasks - The tasks table would store all of the tasks that you are planning to do. The columns of the tasks table would be for exmaple `task_name`, `status`, `due_date` and `priority`.

The Users table will look like this:

```
+----+----------+---------------+--------+
| id | username | name          | active |
+----+----------+---------------+--------+
| 1  |    bobby | Bobby Iliev   |   true |
| 2  |     tony | Tony Lea      |   true |
| 3  |  devdojo | Dev Dojo      |  false |
+----+----------+---------------+--------+
```

Rundown of the table structure:
* We have 4 columns: `id`, `username`, `name` and `active`
* We also have 3 entries/users
* The `id` column is an unique identifier of each user and is auto incremented.

In the next chapter, we will learn how to install MySQL and create our first database.

# Basic Syntax

In this chapter we will go over the basic SQL syntax.

SQL statements are basically the 'commands' that you run against a specific database. Through the SQL statements you are telling MySQL what you want it to do, for example, if you wanted to get the `username` of all of your users stored in the `users` table, you would run the following SQL statement:

```
SELECT username FROM users ;
```

Rundown of the statement:

* `SELECT`: First we specify the `SELECT` keyword which indicates that we want to select some data from the database, other popular keywors are: `INSERT`, `UPDATE` and `DELETE`.
* `username`: Then we sepcify which column we want to select
* `users`: After that we sepcify the table that we want to select the data from.
* The `;` is required. Every SQL statments needs to end with a semicolumn.

If you run the above statement you will get no results as the new `users` table that we've just created is empty.

> As a good practice all SQL keywords should be with uppercase, however it would work just fine if you use lower case as well.

Let's go ahead and cover the basic operations next.

## INSERT

To add data to your database you would use the `INSERT` statemnt.

Let's insert 1 user into our `users` table:

```
INSERT INTO users ('username', 'name', status) VALUES('bobby', 'Bobby Iliev', 1);
```

Rundown of the insert statement:

* `INSERT INTO users`: first we specify the `INSERT INTO` keywordo which tells MySQL that we want to insert data into the `users` table.
* `users ('username', 'name', status)`: then we specify the table name `users` and the columns that we want to insert data into.
* `VALUES`: then we specify the values that we want to insert in.

## SELECT

Once we've inserted that user, let's go ahead and retrieve the information.

To retrieve informaiton from your database, you could use the `SELECT` statement:

```
SELECT * FROM users;
```

As we specify `*` right after the `SELECT` keyword, this means that we want to get all of the columns from the `users` table.

If we wanted to the only the `username` and the `name` columns instead we would change the statement to:

```
SELECT username,name FROM users;
```

Result:

```
TODO
```

## UPDATE

In order to modify data in your database you could use the `UPDATE` statement.

The syntax would look like this:

```
UPDATE users SET username='bobbyiliev' WHERE id=1;
```

Rundown of the statement:

* `UPDATE users`: first we specify the `UPDATE` keyword followed by the table that we want to update
* `username='bobbyiliev'` Then we specify the columnt that we want to update and the new value that we want to set.
* `WHERE id=1`: Finally by using the `WHERE` clause we specify which user should be updated. In our case it is the user with ID 1.

> NOTE: If we don't specify a `WHERE` clause, all of the entries inside the `users` table would be updated and all users would have the `username` set to `bobbyiliev`. You need to be careful when you use the `UPDATE` statement without a `WHERE` caluse as every single row will be updated.

We are going to cover `WHERE` mroe in depth in the next few chapters.

## DELETE

As the name suggests, the `DELETE` statement would remove data from your database.

The syntax is as follows:

```
DELETE FROM users WHERE id=1;
```

Similar to the `UPDATE` statement, if you don't specify a `WHERE` clause, all of the entries from the table will be affected, meaning that all of your users will be deleted.

## Conclusion

Those were some of the most common basic SQL statements.

In the next chapter we are going to go over each of the above statements more in depth.
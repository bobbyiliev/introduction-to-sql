# Tables

Before we get started with the SQL, let's learn how to create tables and columns.

As an example we are going to create a `users` table with the following columns:

* `id` - this is going to be the primary ID of the table and would be the unique identifier of each user.
* `username` - this column would hold the username of our users
* `name` - here we will store the full name of the users
* `status` - here we will store the status of a user which would indicate if a user is active or not.

You need to specify the data type of each column.

In our case it would be like this:

* `id` - Intiger
* `username` - Varchar
* `name` - Varchar
* `status` - Number

## Data types

The most common data types that you would come accross are:

* `char`(size)	Fixed-length character string. Size is specified in parenthesis. Max 255 bytes.
* `varchar`(size)	Variable-length character string. Max size is specified in parenthesis.
* `number`(size)	Number value with a max number of column digits specified in parenthesis.
* `date`	Date value
* `number`(size,d)	Number value with a maximum number of digits of "size" total, with a maximum number of "d" digits to the right of the decimal.

## Creating tables

## Updating tables
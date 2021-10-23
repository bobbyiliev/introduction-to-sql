# `Transaction Control Language`

  - `Transaction Control Language` can be defined as the portion of a database language used for `maintaining consistency` of the database and `managing transactions` in the database. 
  
  - A set of `SQL statements` that are `co-related logically and executed on the data stored in the table` is known as a `transaction`.

## `TCL` Commands

- `COMMIT` Command
- `ROLLBACK` Command
- `SAVEPOINT` Command

## `COMMIT`

The main use of `COMMIT` command is to `make the transaction permanent`. If there is a need for any transaction to be done in the database that transaction permanent through commit command. 

### Syntax
```sql
COMMIT;
```

## `ROLLBACK`

Using this command, the database can be `restored to the last committed state`. Additionally, it is also used with savepoint command for jumping to a savepoint in a transaction.

### Syntax
```sql
ROLLBACK TO savepoint-name;
```

## `SAVEPOINT`

The main use of the Savepoint command is to save a transaction temporarily. This way users can rollback to the point whenever it is needed.

### Syntax
```sql
SAVEPOINT savepoint-name;
```

## Examples

#### This is purchase table that we are going to use through this tutorial

| item         | price | customer_name |
|--------------|-------|---------------|
| Pen          |    10 | Sanskriti     |
| Bag          |  1000 | Sanskriti     |
| Vegetables   |   500 | Sanskriti     |
| Shoes        |  5000 | Sanskriti     |
| Water Bottle |   800 | XYZ           |
| Mouse        |   120 | ABC           |
| Sun Glasses  |  1350 | ABC           |

```sql
UPDATE purchase SET price = 20 WHERE item = "Pen";
```
##### O/P :  Query OK, 1 row affected (3.02 sec) (Update the price of Pen set it from 10 to 20)

```sql
SELECT * FROM purchase;
```
##### O/P
| item         | price | customer_name |
|--------------|-------|---------------|
| Pen          |    20 | Sanskriti     |
| Bag          |  1000 | Sanskriti     |
| Vegetables   |   500 | Sanskriti     |
| Shoes        |  5000 | Sanskriti     |
| Water Bottle |   800 | XYZ           |
| Mouse        |   120 | ABC           |
| Sun Glasses  |  1350 | ABC           |

```sql
START TRANSACTION;
```
##### Start transaction

```sql
COMMIT;
```
##### Saved/ Confirmed the transactions till this point 

```sql
ROLLBACK;
```
##### Lets consider we tried to rollback above transaction

```sql
SELECT * FROM purchase;
```
#### O/P:
| item         | price | customer_name |
|--------------|-------|---------------|
| Pen          |    20 | Sanskriti     |
| Bag          |  1000 | Sanskriti     |
| Vegetables   |   500 | Sanskriti     |
| Shoes        |  5000 | Sanskriti     |
| Water Bottle |   800 | XYZ           |
| Mouse        |   120 | ABC           |
| Sun Glasses  |  1350 | ABC           |
##### As we have committed the transactions the `rollback` will not affect anything

```sql 
SAVEPOINT  sv_update;
```
##### Create the `savepoint` the transactions above this will not be rollbacked

```sql 
UPDATE purchase SET price = 30 WHERE item = "Pen";
```
#### O/P : Query OK, 1 row affected (0.57 sec)
#### Rows matched: 1  Changed: 1  Warnings: 0

```sql 
SELECT * FROM purchase;
```

| item         | price | customer_name |
|--------------|-------|---------------|
| Pen          |    30 | Sanskriti     |
| Bag          |  1000 | Sanskriti     |
| Vegetables   |   500 | Sanskriti     |
| Shoes        |  5000 | Sanskriti     |
| Water Bottle |   800 | XYZ           |
| Mouse        |   120 | ABC           |
| Sun Glasses  |  1350 | ABC           |
##### price of pen is changed to 30 using the `update` command

```sql
ROLLBACK to sv_update;
```
##### Now if we `rollback` to the `savepoint` price should be 20 after `rollback` lets see

```sql 
SELECT * FROM purchase;
```

| item         | price | customer_name |
|--------------|-------|---------------|
| Pen          |    20 | Sanskriti     |
| Bag          |  1000 | Sanskriti     |
| Vegetables   |   500 | Sanskriti     |
| Shoes        |  5000 | Sanskriti     |
| Water Bottle |   800 | XYZ           |
| Mouse        |   120 | ABC           |
| Sun Glasses  |  1350 | ABC           |
| Torch        |   850 | ABC           |
##### As expected we can see `update` query is rollbacked to sv_update.



## Conclusion

With this short tutorial we have learnt TCL commands.

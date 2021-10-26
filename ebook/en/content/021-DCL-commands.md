# `Data Control Language`
DCL commands are used to grant and take back authority from any database user.

## `DCL` Commands
- `GRANT` Command
- `REVOKE` Command

## `GRANT`
`GRANT` is used to give user access privileges to a database.

### Syntax
```sql
GRANT privilege_name ON objectname TO user;
```

## `REVOKE`
`REVOKE` remove a privilege from a user. REVOKE helps the owner to cancel previously granted permissions.


### Syntax
```sql
REVOKE privilege_name ON objectname FROM user;
```

### `DCL` Examples
```sql
SELECT * FROM purchase;
```
Output:
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
```

- Lets start with `GRANT` command:

```sql
  GRANT INSERT ON purchase TO 'Sanskriti'@'localhost'; 
```
Output:
```
#### O/P Query OK, 0 rows affected (0.31 sec)
```

Description In above command we have granted user Sanskriti priviledge to `Insert` into purchase table.

- Now if I login as Sanskriti and try to run `Select` statement as given below what should happen?

```sql 
SELECT * FROM purchase;
```

Output:
```
#### O/P ERROR 1142 (42000): SELECT command denied to user 'Sanskriti'@'localhost' for table 'purchase'
```
Yup as expected it gives error because we have granted insert operation to Sanskriti.

- So lets try inserting data to purchase table:
```sql  
INSERT INTO purchase values("Laptop", 100000, "Sanskriti");
```

Output:
```
#### O/P Query OK, 1 row affected (0.34 sec)
```
Yes! It works!

- Now I am checking the purchase table from my original account:
```sql
SELECT * FROM purchase;
```

Output:
```
| item         | price  | customer_name |
|-------------|--------|---------------|
| Pen          |     20 | Sanskriti     |
| Bag          |   1000 | Sanskriti     |
| Vegetables   |    500 | Sanskriti     |
| Shoes        |   5000 | Sanskriti     |
| Water Bottle |    800 | XYZ           |
| Mouse        |    120 | ABC           |
| Sun Glasses  |   1350 | ABC           |
| Torch        |    850 | ABC           |
| Laptop       | 100000 | Sanskriti     |
```

As you can see, the row is inserted.

- Now lets try `Revoke` command:

```sql 
REVOKE INSERT ON purchase FROM 'Sanskriti'@'localhost';
```

Output:
```
#### O/P Query OK, 0 rows affected (0.35 sec)
```

Now we have revoked the insert priviledge from Sanskriti.

- If Sanskriti runs insert statement it should give error:

```sql 
INSERT INTO purchase values("Laptop", 100000, "Sanskriti");
```

Output:
```
#### O/P ERROR 1142 (42000): INSERT command denied to user 'Sanskriti'@'localhost' for table 'purchase'
```

## Conclusion

Through this tutorial we have learnt `DCL` commands and their usage.

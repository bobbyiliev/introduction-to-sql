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

revoke insert on purchase from 'Sanskriti'@'localhost';
Query OK, 0 rows affected (0.35 sec)
grant insert on purchase to 'Sanskriti'@'localhost';
Query OK, 0 rows affected (0.31 sec)

mysql> select *from purchase;
+--------------+--------+---------------+
| item         | price  | customer_name |
+--------------+--------+---------------+
| Pen          |     20 | Sanskriti     |
| Bag          |   1000 | Sanskriti     |
| Vegetables   |    500 | Sanskriti     |
| Shoes        |   5000 | Sanskriti     |
| Water Bottle |    800 | XYZ           |
| Mouse        |    120 | ABC           |
| Sun Glasses  |   1350 | ABC           |
| Torch        |    850 | ABC           |
| Laptop       | 100000 | Sanskriti     |

select * from purchase;
+--------------+-------+---------------+
| item         | price | customer_name |
+--------------+-------+---------------+
| Pen          |    20 | Sanskriti     |
| Bag          |  1000 | Sanskriti     |
| Vegetables   |   500 | Sanskriti     |
| Shoes        |  5000 | Sanskriti     |
| Water Bottle |   800 | XYZ           |
| Mouse        |   120 | ABC           |
| Sun Glasses  |  1350 | ABC           |
| Torch        |   850 | ABC           |

elect * from purchase;
ERROR 1142 (42000): SELECT command denied to user 'Sanskriti'@'localhost' for table 'purchase'
mysql> insert into purchase values("Laptop", 100000, "Sanskriti");
Query OK, 1 row affected (0.34 sec)

insert into purchase values("Laptop", 100000, "Sanskriti");
ERROR 1142 (42000): INSERT command denied to user 'Sanskriti'@'localhost' for table 'purchase'



## Conclusion

Through this tutorial we have learnt `TCL` commands and their usage.

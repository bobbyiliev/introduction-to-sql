# DELETE

As the name suggests, the `DELETE` statement would remove data from your database.

The syntax is as follows:

```
DELETE FROM users WHERE id=5;
```

The output should indicate that 1 row was affected:

```
Query OK, 1 row affected (0.01 sec)
```

> Important: Just like the `UPDATE` statement, if you don't specify a `WHERE` clause, all of the entries from the table will be affected, meaning that all of your users will be deleted. So it is critical to always add a `WHERE` clause when executing a `DELETE` statement.

Similar to the Linux `rm` command, when you use the `DELETE` statement, the data would be gone permanently, and the only way to recover your data would be by restoring a backup.
# DELETE

As the name suggests, the `DELETE` statement would remove data from your database.

The syntax is as follows:

```sql
DELETE FROM users WHERE id=5;
```

The output should indicate that 1 row was affected:

```
Query OK, 1 row affected (0.01 sec)
```

> Important: Just like the `UPDATE` statement, if you don't specify a `WHERE` clause, all of the entries from the table will be affected, meaning that all of your users will be deleted. So, it is critical to always add a `WHERE` clause when executing a `DELETE` statement.

```sql
DELETE FROM users;
```

The output should indicate (where x is the number of tuples in the table):
```
Query OK, x row(s) affected (0.047 sec)
```

Similar to the Linux `rm` command, when you use the `DELETE` statement, the data would be gone permanently, and the only way to recover your data would be by restoring a backup.

## Delete from another table

As we saw in the two precedents sections you can `INSERT` or `UPDPATE` tables rows based on other table data. You can do the same for the `DELETE`.

For example, if you want to delete the records from the `users` table if the corresponding prospect has been disabled, you could do it this way:

```sql
delete users
from users, prospect_users
where users.username = prospect_users.username
and NOT prospect_users.active
```

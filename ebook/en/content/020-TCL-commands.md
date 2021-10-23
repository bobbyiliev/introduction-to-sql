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
### `COMMIT` Examples

## `ROLLBACK`

Using this command, the database can be `restored to the last committed state`. Additionally, it is also used with savepoint command for jumping to a savepoint in a transaction.

### Syntax
```sql
ROLLBACK TO savepoint-name;
```

### `ROLLBACK` Examples

## `SAVEPOINT`

The main use of the Savepoint command is to save a transaction temporarily. This way users can rollback to the point whenever it is needed.

### Syntax
```sql
SAVEPOINT savepoint-name;
```

### `SAVEPOINT` Examples


## Conclusion

With this short tutorial we have learnt TCL commands.

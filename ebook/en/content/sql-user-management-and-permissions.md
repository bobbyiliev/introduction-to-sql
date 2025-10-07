# SQL User Management and Permissions

## Introduction

In SQL, managing users and their permissions is essential for maintaining security, protecting data, and ensuring controlled database access.  
This guide walks you through how to create users, assign and revoke privileges, and follow best practices for managing access in MySQL.

You’ll learn how to:

- Create and manage MySQL users  
- Grant and revoke privileges  
- Understand privilege levels  
- Apply roles and best practices  

---

## 1. Creating a New User

When you install MySQL, it usually comes with an administrative user (often `root`). However, it’s unsafe to use this account for daily operations. Instead, create specific users for specific tasks.

### Syntax
```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

### Example
```sql
CREATE USER 'john'@'localhost' IDENTIFIED BY 'john123';
```
This creates a user named **john** who can log in only from the local machine (`localhost`).

If you want to allow the user to connect remotely (from any host):
```sql
CREATE USER 'john'@'%' IDENTIFIED BY 'john123';
```

This setup is common for applications that connect from external servers, but always use strong passwords when enabling remote access.

---

## 2. Granting Privileges

After creating a user, you need to decide what they’re allowed to do in the database. Privileges define actions such as reading data, adding records, or deleting tables.

### Syntax
```sql
GRANT privileges ON database.table TO 'username'@'host';
```

### Common Privileges

| Privilege       | Description              |
|-----------------|--------------------------|
| SELECT          | Read data from tables    |
| INSERT          | Add new rows             |
| UPDATE          | Modify existing data     |
| DELETE          | Remove records           |
| ALL PRIVILEGES  | Grant all available permissions |
| USAGE           | No privileges (used to create a user only) |

### Example
```sql
GRANT SELECT, INSERT ON salesdb.customers TO 'john'@'localhost';
```
This allows **john** to read and insert customer data in the `salesdb` database.

If you want to give access to everything inside `salesdb`:
```sql
GRANT ALL PRIVILEGES ON salesdb.* TO 'john'@'localhost';
```

---

## 3. Applying Privileges

In older MySQL versions, you needed to refresh the privilege table manually using:
```sql
FLUSH PRIVILEGES;
```
In most recent versions, MySQL applies changes automatically, so you rarely need this command anymore.

---

## 4. Revoking Privileges

If a user no longer needs certain permissions, you can revoke them.

### Syntax
```sql
REVOKE privileges ON database.table FROM 'username'@'host';
```

### Example
```sql
REVOKE INSERT ON salesdb.customers FROM 'john'@'localhost';
```
Now, **john** can still read data but can no longer insert new records into that table.

---

## 5. Deleting a User

To remove a user completely from MySQL:
```sql
DROP USER 'john'@'localhost';
```
Always make sure that the user no longer owns any critical data or roles before deletion.

---

## 6. Viewing Privileges

You can check what permissions a user currently has.

For the logged-in user:
```sql
SHOW GRANTS;
```

For another user:
```sql
SHOW GRANTS FOR 'john'@'localhost';
```

This is very helpful during audits or troubleshooting permission issues.

---

## 7. Privilege Levels in MySQL

Privileges can be assigned at different levels of scope — from the entire server to individual columns.

| Level           | Description                        | Example |
|-----------------|------------------------------------|----------|
| Global          | Affects all databases and tables   | `GRANT ALL ON *.* TO 'u'@'h';` |
| Database        | Affects one specific database      | `GRANT SELECT ON salesdb.* TO 'u'@'h';` |
| Table           | Affects only one table             | `GRANT INSERT ON salesdb.orders TO 'u'@'h';` |
| Column          | Affects specific columns           | `GRANT SELECT (name, email) ON customers TO 'u'@'h';` |
| Stored Routine  | Applies to procedures/functions    | `GRANT EXECUTE ON PROCEDURE p1 TO 'u'@'h';` |

Understanding these levels helps you grant only the permissions users actually need.

---

## 8. Best Practices

- Avoid using the **root** account for regular operations or applications.  
- Always apply the **principle of least privilege** — grant only what’s necessary.  
- Use **strong, unique passwords** for each user.  
- Review user privileges periodically.  
- For large systems, use **roles** to group privileges efficiently.

---

## 9. Managing Roles (MySQL 8.0+)

Roles simplify user management by grouping privileges under a single label. You can then assign these roles to multiple users.

### Create a Role
```sql
CREATE ROLE 'read_only';
```

### Grant Privileges to the Role
```sql
GRANT SELECT ON salesdb.* TO 'read_only';
```

### Assign the Role to a User
```sql
GRANT 'read_only' TO 'john'@'localhost';
SET DEFAULT ROLE 'read_only' TO 'john'@'localhost';
```

This way, you can manage access for many users by modifying the role rather than updating each account.

---

## Summary

| Task | Command Example |
|------|-----------------|
| **Create User** | `CREATE USER 'u'@'h' IDENTIFIED BY 'p';` |
| **Grant Privilege** | `GRANT SELECT ON db.table TO 'u'@'h';` |
| **Revoke Privilege** | `REVOKE UPDATE ON db.table FROM 'u'@'h';` |
| **Delete User** | `DROP USER 'u'@'h';` |
| **View Grants** | `SHOW GRANTS FOR 'u'@'h';` |

---

## Key Takeaway

Managing users effectively is about balancing convenience and security.  
Give each user only what they need, keep privileges organized through roles, and regularly review access — these simple steps will keep your database safe and well-managed.

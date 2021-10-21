# SQL Sub Queries

###  A subquery is a SQL query nested inside a larger query.

- A subquery may occur in
  - A SELECT clause
  - A FROM clause
  - A WHERE clause

- The subquery can be nested inside a SELECT, INSERT, UPDATE, or DELETE statement or inside another subquery.
- A subquery is usually added within the WHERE Clause of another SQL SELECT statement.
- The inner query executes first before its parent query so that the results of an inner query can be passed to the outer query.

#### You can use a subquery in a SELECT, INSERT, DELETE, or UPDATE statement to perform the following tasks:
- Compare an expression to the result of the query.
- Determine if an expression is included in the results of the query.
- Check whether the query selects any rows.

**_Subqueries with the `SELECT` Statement_**:

Consider the CUSTOMERS table having the following records 

    +----+----------+-----+-----------+----------+
    | ID | NAME     | AGE | ADDRESS   | SALARY   |
    +----+----------+-----+-----------+----------+
    |  1 | Ramesh   |  35 | Ahmedabad |  2000.00 |
    |  2 | Khilan   |  25 | Delhi     |  1500.00 |
    |  3 | Kaushik  |  23 | Kota      |  2000.00 |
    |  4 | Chaitali |  25 | Mumbai    |  6500.00 |
    |  5 | Hardik   |  27 | Bhopal    |  8500.00 |
    |  6 | Komal    |  22 | MP        |  4500.00 |
    |  7 | Muffy    |  24 | Indore    | 10000.00 |
    +----+----------+-----+-----------+----------+
Now, let us check the following subquery with a SELECT statement.

_**Example**_:
```sql
SELECT *
FROM CUSTOMERS
WHERE ID IN (
    SELECT ID
    FROM CUSTOMERS
    WHERE SALARY > 4500
);
```
This would produce the following result.

    +----+----------+-----+---------+----------+
    | ID | NAME     | AGE | ADDRESS | SALARY   |
    +----+----------+-----+---------+----------+
    |  4 | Chaitali |  25 | Mumbai  |  6500.00 |
    |  5 | Hardik   |  27 | Bhopal  |  8500.00 |
    |  7 | Muffy    |  24 | Indore  | 10000.00 |
    +----+----------+-----+---------+----------+


**_Subqueries with the `UPDATE` Statement_**:

The subquery can be used in conjunction with the UPDATE statement. Either single or multiple columns in a table can be updated when using a subquery with the UPDATE statement.

**_Example_**:

Assuming, we have CUSTOMERS_BKP table available which is backup of CUSTOMERS table. The following example updates SALARY by 0.25 times in the CUSTOMERS table for all the customers whose AGE is greater than or equal to 27.
```sql
UPDATE CUSTOMERS
SET SALARY = SALARY * 0.25
WHERE AGE IN (
    SELECT AGE
    FROM CUSTOMERS_BKP
    WHERE AGE >= 27
);
```
This would impact two rows and finally CUSTOMERS table would have the following records.

    +----+----------+-----+-----------+----------+
    | ID | NAME     | AGE | ADDRESS   | SALARY   |
    +----+----------+-----+-----------+----------+
    |  1 | Ramesh   |  35 | Ahmedabad |   125.00 |
    |  2 | Khilan   |  25 | Delhi     |  1500.00 |
    |  3 | Kaushik  |  23 | Kota      |  2000.00 |
    |  4 | Chaitali |  25 | Mumbai    |  6500.00 |
    |  5 | Hardik   |  27 | Bhopal    |  2125.00 |
    |  6 | Komal    |  22 | MP        |  4500.00 |
    |  7 | Muffy    |  24 | Indore    | 10000.00 |
    +----+----------+-----+-----------+----------+

**_Subqueries with the `DELETE` Statement_**:

The subquery can be used in conjunction with the DELETE statement like with any other statements mentioned above.

**_Example_**:

Assuming, we have a CUSTOMERS_BKP table available which is a backup of the CUSTOMERS table. The following example deletes the records from the CUSTOMERS table for all the customers whose AGE is greater than or equal to 27.
```sql
DELETE FROM CUSTOMERS
WHERE AGE IN (
    SELECT AGE
    FROM CUSTOMERS_BKP
    WHERE AGE >= 27
);
```
This would impact two rows and finally the CUSTOMERS table would have the following records.

    +----+----------+-----+---------+----------+
    | ID | NAME     | AGE | ADDRESS | SALARY   |
    +----+----------+-----+---------+----------+
    |  2 | Khilan   |  25 | Delhi   |  1500.00 |
    |  3 | Kaushik  |  23 | Kota    |  2000.00 |
    |  4 | Chaitali |  25 | Mumbai  |  6500.00 |
    |  6 | Komal    |  22 | MP      |  4500.00 |
    |  7 | Muffy    |  24 | Indore  | 10000.00 |
    +----+----------+-----+---------+----------+
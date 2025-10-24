# Essential MySQL Functions 

MySQL has many built-in functions. We will covering some important most used built-in functions; for a complete list refer to the online MySQL Reference Manual (http://dev.mysql.com/doc/).

> NOTE: As of now we will be going through only function and their output, as they would be self explanatory.

## Numeric Functions
```sql
SELECT ROUND(5.73)
   ```
6

```sql
SELECT ROUND(5.73, 1)
   ```
5.7

```sql
SELECT TRUNCATE(5.7582, 2)
   ```
5.75

```sql
SELECT CEILING(5.2)
   ```
6

```sql
SELECT FLOOR(5.7)
   ```
5

```sql
SELECT ABS(-5.2)
   ```
5.2

```sql
SELECT RAND() -- Generates a random floating point number b/w 0 & 1
   ```

## STRING Functions
```sql
SELECT LENGTH('sky')
   ```
3

```sql
SELECT UPPER('sky')
   ```
SKY

```sql
SELECT LOWER('sky)
   ```
sky

```sql
SELECT LTRIM('    sky')
   ```
sky

```sql
SELECT RTRIM('sky    ')
   ```
sky

```sql
SELECT TRIM('   sky  ')
   ```
sky

```sql
SELECT LEFT('Kindergarten', 4)
   ```
Kind

```sql
SELECT RIGHT('Kindergarten', 6)
   ```
garten

```sql
SELECT SUBSTRING('Kindergarten', 3, 5) 
   ```
nderg

```sql
SELECT LOCATE('n','Kindergarten') -- LOCATE returns the first occurrence of a character or character string, if found, otherwise it returns 0
   ```
3

```sql
SELECT REPLACE('Kindergarten', 'garten', 'garden')
   ```
Kindergarden

```sql
SELECT CONCAT('first', 'last')
   ```
firstlast

## DATE Functions
```sql
SELECT NOW()
   ```
2021-10-21 19:59:47

```sql
SELECT CURDATE()
   ```
2021-10-21

```sql
SELECT CURTIME()
   ```
20:01:12

```sql
SELECT MONTH(NOW())
   ```
10

```sql
SELECT YEAR(NOW())
   ```
2021

```sql
SELECT HOUR(NOW())
   ```
13

```sql
SELECT DAYTIME(NOW())
   ```
Thursday

## Formatting Dates and Times

> In MySQL, the default date format is "YYYY-MM-DD", ex: "2025-05-12", MySQL allows developers to format it the way they want. We will discuss some of them.
```sql
SELECT DATE_FORMAT(NOW(), '%M %D %Y')
   ```
October 22nd 2021

```sql
SELECT DATE_FORMAT(NOW(), '%m %d %y')
   ```
10 22 21 

```sql
SELECT DATE_FORMAT(NOW(), '%m %D %y')
   ```
10 22nd 21 

```sql
SELECT TIME_FORMAT(NOW(), '%H %i %p')
   ```
14:11 PM

## Calculating Dates and Times

```sql
SELECT DATE_ADD(NOW(), INTERVAL 1 DAY) --return tomorrows date and time
   ```

2021-10-23 14:26:17

```sql
SELECT DATE_ADD(NOW(), INTERVAL -1 YEAR)
   ```
or
```sql
SELECT DATE_SUB(NOW(), INTERVAL 1 YEAR)
   ```
> Both the queries will return the same output

2020-10-22 14:29:47

```sql
SELECT DATEDIFF('2021-09-08 09:00', '2021-07-07 17:00') -- It will return the difference in number of days, time won't be considered
   ```

63

```sql
SELECT TIME_TO_SEC('09:00') - TIME_TO_SEC('09:02')
   ```
-120

## NULL Handling Functions

These functions are essential for managing and substituting `NULL` (missing) values, ensuring your queries return clean and predictable data.

### IFNULL()

Returns the first expression if it is not `NULL`. Otherwise, it returns the second expression. This is a quick way to provide a default value for a single column.

> Assume the `employees` table has a `bonus` column that may contain NULL values.
```sql
SELECT IFNULL(bonus, 0) FROM employees;
   ```
If bonus is NULL, the output is 0. Otherwise, it shows the bonus amount.

### COALESCE()

Returns the first non-NULL value from a list of expressions. It's useful when you have multiple potential sources for a value.

```sql
SELECT COALESCE(formal_nickname, fan_alias, 'No Nickname') FROM players;
   ```
Returns the first available nickname or 'No Nickname' if all are NULL.

```sql
SELECT COALESCE(NULL, NULL, NULL)
   ```
NULL

```sql
SELECT COALESCE('First Value', 'Second Value', 'Third Value')
   ```
First Value


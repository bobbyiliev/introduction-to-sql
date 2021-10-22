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
SELECT LOCATE('n','Kindergarten')
   ```
3
> NOTE:LOCATE returns the first occurence of a character or a sequence of characters, if the character if not present then mysql will return 0

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

> In MySQL by default date format is "YYYY-MM-DD", ex: "2025-05-12, MySQL allows developers to format it in the way they want. We will discus some of them.
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


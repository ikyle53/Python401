# Python401  

## SQL  

### **SQL Summary**:  

From the tutorial experience it seems SQL is pretty straight forward. Everything is stored in tables, columns, and rows. Should I want to retrieve data I use a querie to create a new table behind the scenes with constraints that filters data and outputs the results. Furthermore, these tables, columns, and rows can be changed/corrected/deleted with the right clauses. The tables can also be joined to piece together similar data. SQL uses various data types that are stored in the tables. I can make the data as dynamic or undynamic as I want. I think SQL can be very powerful given its abilities.  

`SELECT` queries:  

`SELECT` is a command to query data which defines what I'm looking for. It pulls from the columns of a table that's in the database and presents results.  

- Columns are concidered `properties`
- Rows are concidered the `instances`  

A basic query from a table called `mytable`  
`SELECT column, another_column, ...` #`column` is the actual name of the column  
`FROM mytable`  

I can also select all of the coulmns using `*`  
`SELECT *`  
`FROM mytable`  

## Constraints  

Constraints can be used which I kinda find like an `if` statement that pulls data if the conditions are true.  

In this case SQL uses the `WHERE` statement to pull data where a certain condition is true and the data is filtered and returned to me.  

`SELECT column`  
`FROM mytable`  
`WHERE condition`  
    `AND/OR` #another condition  
    `AND/OR` #another condition  

Here are a few operators that the conditions can use:  
`=` equal to  
`!=` not equal to  
`<` less than  
`<=` less than or equal to  
`>` more than  
`>=` more than or equal to  
`BETWEEN 1 AND 10` a number within a range  
`NOT BETWEEN 1 AND 10` a number NOT within a range  
`IN (a list within a column)` a number that exists in a list  
`NOT IN (a list within a column)` a number that doesn't exist in a list  

> NOTE: Capitalization of the keywords isn't required, but is highly recommended.  

## WHERE String constraints  

`=` case sensetive  
`!=` case sensetive inequality  
`LIKE` case insensetive exact string comparison  
`NOT LIKE` case insensetive string inequality comparison  
`%` matches a sequence of characters (%AT%)  
`-` mathces a single character but only usable with LIKE and NOT LIKE  
`IN` a string that exists in a list  
`NOT IN` a string that doesn't exist in a list  

> NOTE: All string must be in quotes  

`SELECT Title FROM Movies;`  
`WHERE title LIKE "Toy Story%";`  

`SELECT * FROM Movie;`  
`WHERE title LIKE "WALL-";  

## Filtering and Sorting  

`DISTINCT` is a keyword that rids the query of duplicate values. If I were to search salaries of employees and I see the salary of $24,000 eight times I can use distinct to only see it once instead of the eight times.  

`ORDER BY` is a keyword that orders my query results  

`SELECT DISTINCT column;`  
`FROM Movie;`  
`WHERE condition;`  
`ORDER BY column ASC/DESC` # Ascending or Descending  

> NOTE: `ORDER BY` will order the contents alpha-numerically  

## Limiting results to a subset  

Limiting results to a subset means I'm going to reduce the amount of data that's going to be queried. In this case I'm going to use `LIMIT` which basically tells SQL that I want a certain amount of rows returned and it doesn't have to query the others. This makes querying a bit faster in larger databases.  

`SELECT column`  
`FROM movies`  
`WHERE condition`  
`ORDER BY column ASC/DESC`  
`LIMIT num_limit OFFSET num_offset`  

`SELECT Title FROM movies`  
`ORDER BY Title ASC`  
`LIMIT 5 OFFSET 5;`  

## Database normalization  

Normalization is the process of minimizing duplicate data among several tables of data I pull from. The trade off is a more complex query, but it has better performance.  

Tables can share some of the same information needed so pulling from another table may be necessary. To do so I use `JOIN` keyword.  

`SELECT column`  
`FROM mytable`  
`INNER JOIN another_table`  
    `ON mytable.id = another_table.id`  
`WHERE condition`  
`ORDER BY column ASC/DESC`  
`LIMIT num_limit OFFSET num_offset`  

`INNER JOIN` matches rows from the first table and second table which have the same key (`ON` statement). A results row is then returned with both rows combined. Afterwards the other keywords are applied.  

There's also the `LEFT` and `RIGHT` join where the left join will append all of its table's rows to the right's table and the right join will append all of the right's table data to the left's.  

## Null values  

I want to avoid as many Nulls as possible if it can be helped. The way to do this is by testing for Nulls in the `WHERE`.  

An alternative to `NULL` is to use a 0 for integers and an empty string `""` when possible depending on the data type.  

`SELECT column`  
`FROM myTable`  
`WHERE column IS/IN NOT NULL`  
`AND/OR other condition`  

## Expressions in queries  

Matematical expressions can be used in queries to pull data faster.  

`SELECT vehicle_speed / 2 AS slow_vehicle`  
`FROM car_data`  
`WHERE RPM >= 3000`  

Using `AS` creates an alias of the expression to help with readability.  

`SELECT Titles, (domestic_sales + international_sales) / 1000000 AS gross_sales`  
`FROM movies`  
`JOIN boxoffice`  
`ON movies.id = boxoffice.movie_id;`  

## Queries with aggregates  

Aggregate functions help with queries where I want to count, sum, average, etc. These are built-in functions designed to help.  

`SELECT AGG_FUNC(column_or_expression)`  

This aggregate function would run on the entire set of results.  

Another useful aggregate is the `HAVING` keyword which works only with the `GROUP BY` keyword. It further filters grouped rows, which makes it easier for HUGE databases.  

Regular way:  
`SELECT role, COUNT(*) as Number_of_artists`  
`FROM employees`  
`WHERE role = "Artist";`  

`Having way:`  
`SELECT Role, SUM(Years_employed)`  
`FROM employees`  
`GROUP BY Role`  
`HAVING Role = "Engineer";`  

## Order of Execution  

SQL has an order to which keywords are ran and each query should emulate the following:  

1. `SELECT DISTINCT column, AGG_FUNC(column_or_expression), …`  
2. `FROM mytable`  
3. `JOIN another_table`  
4. `ON mytable.column = another_table.column`  
5. `WHERE constraint_expression`  
6. `GROUP BY column`  
7. `HAVING constraint_expression`  
8. `ORDER BY column ASC/DESC`  
9. `LIMIT count OFFSET COUNT;`  

## Adding data to the database  

`INSERT` - Sounds pretty self explanatory. Inserting data into the database. I'll need the following values:  

`INSERT INTO myTable`  
`(column_name, another_column_name)`  
`VALUES(value_or_expression, another_value, ...), (value, or expression, another_value, ...)`  

Example:  

`INSERT INTO Boxoffice`  
`(movie_id, rating, sales_in_millions)`  
`VALUES(1, 9.9, 283742034 / 1000000);`  

## Updating data  

`UPDATE` - I have to specify which table, column, and row. Also, the data type has to match.  

`UPDATE myTable`  
`SET column = value_or_expression, another_column = another_value`  
`WHERE condition`  

> NOTE: EXCLUDING THE `WHERE` CLAUSE WILL APPLY THE UPDATE TO ALL ROWS  

## Deleting data  

`DELETE FROM myTable`  
`WHERE Condition`  

> NOTE: EXCLUDING THE `WHERE` CLAUSE WILL CLEAR THE ENTIRE TABLE. It can be used intentionally.  

## Creating a table  

`CREATE TABLE IF NOT EXISTS tableName (`  
`columnName dataType tableConstraint DEFAULT default_value,`  
`another_column_name dataType tableConstraint DEFAULT default_value`  
`);`  

The `IF NOT EXISTS` clause will skip creating a table if one already exists under that table name.  

## Table datatypes  

`INTEGER`, `BOOLEAN` - 4, 0 False/1 True  
`FLOAT`, `DOUBLE`, `REAL` - 1.6, 4.94065645841247E-324, 33.5  
`CHARACTER(num_char)`, `VARCHAR(num_char)`, `TEXT` - CHAR(ASCII(SUBSTRING(@string, @position, 1))), VARCHAR(n), text  
`DATE`, `DATETIME` - YYYY-MM-DD, 00:00:00  
`BLOB` - Binary Large Object  

### Constraints on tables

`PRIMARY KEY` - Values in this column are unique. Each value can be used to id a single row.  
`AUTOINCREMENT` - For integer values. Automatically fills in for each row.  
`UNIQUE` - Values in this column have to be unique. Doesn't have to be a key.  
`NOT NULL` - Inserted value cannot be NULL  
`CHECK(expression)` - Allows me to run expressions to check whether the inserted values are valid  
`FOREIGN KEY` - This helps check for consistency in keys between multiple tables. It checks to see if the Id's match up.  

### Editing a table  

Adding columns:  
`ALTER TABLE tableName`  
`ADD columnName dataType optionalTableConstraints`  
`DEFAULT default_value`  

Removing columns:  
`ALTER TABLE tableName`  
`DROP column_to_be_deleted`  

Renaming a column:  
`ALTER TABLE tableName`  
`RENAME TO new_table_name`  

### Removing an entire table  

`DROP TABLE IF EXISTS tableName`  

![Tutorial Finished](finished.png)

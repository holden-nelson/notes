# SQL Review

## Basics

#### Select

`SELECT` is the basic query statement in SQL. 

```sql
SELECT *; -- select everything from every table
SELECT * FROM table_name; -- select everything from one table

SELECT column1, column2, ... -- select certain columns
FROM table_name;
```

#### Constraints

We can use `WHERE` to filter out certain rows

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1
	AND condition2
	OR condition3
```

There are operators that can be used in conditionals

```
=, !=, <, <=, >, >=				col_name != 4
BETWEEN ... AND ...				col_name BETWEEN 1.5 AND 10.5
NOT BETWEEN ... AND ...
IN (...)									col_name IN (2,4,6)
NOT IN (...)
LIKE											col_name LIKE "ABC" -- case ins.
NOT LIKE
%													col_name LIKE "%AT%" -- AT, ATTIC ..
_												  col_name LIKE "AN_" -- AND, but not AN
```

#### Filtering and Sorting

`DISTINCT` removes duplication in specified columns

`ORDER BY` lets us specify an ordering

`LIMIT` and `OFFSET` let us retrieve a subset of results

```
SELECT DISTINCT column, another_column, ...
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

#### Joins

`INNER JOIN` can be used when you only want rows that are present in both tables

`LEFT JOIN`, `RIGHT JOIN`,`FULL JOIN` can be used when a FK doesn't exist in another database but you still want that item to show up in results.

```sql
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

#### Expressions

Allows us to write more complex logic on column values in a query.

```sql
SELECT col_expression AS expr_description, …
FROM mytable;
```

#### Aggregates/Grouping

Summarizes information about groups of data.

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column
HAVING group_condition;
```

Aggregate functions: `COUNT`, `MIN`, `MAX`, `AVG`, `SUM` 

`HAVING` allows us to further filter the grouped rows.

#### Table Creation

```sql
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT def_val,
    ...
    );
```
Default values are optional.
`IF NOT EXISTS`  is there to suppress an error if trying to create a table that already exists.
### Example Data Types
`INTEGER`, `BOOLEAN`
Integer datatypes can store whole integer values like the count of a number or an age. In some implementations, booleans are represented with integers 0 or 1.

`FLOAT`, `DOUBLE`, `REAL`
Floating point datatypes can store more precise numerical data like measurements or fractional values. Different types can be used depending on the floating point precision required for that value.

`CHARACTER(num_chars)`, `VARCHAR(num_chars)`, `TEXT`
Text based datatypes can store strings and text in all sorts of locales. Distinctions between these types generally amount to underlaying efficiency of the database when working with these columns.

`DATE`, `DATETIME`
SQL can store date and time stamps to keep track of time series and event data. These can be tricky to work with, especially when manipulating data across timezones.

`BLOB`
Binary data in blobs can be stored right into a SQL database. These values are typically opaque, so we usually have to store them with sufficient metadata to query them.
### Example Table Constraints
`PRIMARY KEY`
Each value in this column are unique, and each value can be used to identify a single row in this table

`AUTOINCREMENT`
For integers, the value is automatically incremented and filled in on all row insertions. Not supported in all databases.

`UNIQUE`
Values in this column must be unique.

`NOT NULL`
Inserted values can not be `NULL`

`CHECK(expression)`
Run expressions to test whether inserted values are valid. 

`FOREIGN KEY`
Consistency check with ensures each value in this column corresponds to another value in a column in another table.

#### Creating Schemas
```sql
create schema schema_name;

-- add a table to a schema
create table schema_name.example_table (
    ...
);

-- delete a schema and everything in it
drop schema schema_name cascade
```
## Altering Tables
#### Insert
We use an `INSERT`  statement to insert data into a database. We declare which table to write into, the columns of data that we are filling, and one or more rows of data to insert.

In general, each row of data we insert should contain values for every corresponding column in the table. Multiple rows can be listed sequentially.
```sql
INSERT INTO mytable
VALUES (value_or_expr, another_val_or)expr, ...) , 
       (value_or_expr_2, another_val_or_expr_2, ...) ,
       ... ;
```
In some cases, if we have incomplete data and the table contains columns that support default values, we can insert rows with only the columns of data we have by specifying them explicitly.
```sql
INSERT INTO mytable
(column, another_column, ...)
VALUES (value_or_expr, another_val_or)expr, ...) , 
       (value_or_expr_2, another_val_or_expr_2, ...) ,
       ... 
RETURNING expr
;
```
In this case the number of values needs to match the number of columns specified. Although this is more verbose, inserting values this way is forward compatible: if we add a new column to the table with a default value, no hardcoded `INSERT`s will have to change as a result.

#### Update

`UPDATE` works very similar to `INSERT`
```sql
UPDATE mytable
SET column = value_or_expr,
    other_column = another_value_or_expr,
    ...
WHERE condition;
```
The statement works by taking multiple column/value pairs, and applying this to every row that satisfies the constraint in the `​WHERE`​ clause.
#### Delete
```sql
DELETE FROM mytable
WHERE condition;
```
### Alter Table
The `ALTER TABLE` statement is used to add, remove, or modify columns and table constraints for existing tables.

#### Adding Columns
```sql
ALTER TABLE mytable
ADD column DataType TableConstraint
    DEFAULT default_value;
```
#### Removing Columns
```sql
ALTER TABLE mytable
DROP column_to_be_deleted;
```
Some databases don’t support this feature.
#### Renaming Tables
```sql
ALTER TABLE mytable
RENAME TO new_table_name;
```
#### Dropping Tables
```sql
DROP TABLE IF EXISTS mytable;
```
## Subqueries

A subquery is a query that is nested inside of another query. This allows us to compute a value, or set of values, and pass it into another query.

When the subquery produces a single value, we call it a scalar subquery.
#### Uncorrelated Subqueries
For example, say we want to select all of the films whose length is greater than the average length of all of the films.
```sql
select
    title, length
from film
where length > (select avg(length) from film)
;
```
This is also called an uncorrelated subquery because it has no dependence on the main query and could be theoretically run independently.
#### Correlated Subqueries
The primary way to distinguish correlated and uncorrelated subqueries is that correlated subqueries are not just run once - they are run for each potential row of output in the outer query.

Let’s return, for each customer, the date of their most recent rental.
```sql
select
    c.customer_id, 
    c.first_name, 
    c.last_name,
    (select max(r.rental_date)
    from rental as r
    where r.customer_id = c.customer_id) as "Most Recent Rental"
from customer as c
;
```
Note that we referenced the aliased customer table from the outer query in the inner query - this is the moment our query became correlated.
#### Table Subqueries
It’s also possible to use subqueries in the `from` clause to return entire tables to then query.

These are especially useful if we need to make multiple “passes” over some data. They’re also useful for creating our own “virtual” table to then e.g. join onto already existing tables for operations.
#### Lateral Subqueries
A lateral subquery is essentially a correlated subquery that is used as a table subquery. For example, to list the three most recent rentals per customer:
```sql
select c.customer_id, d.rental_id, d.rental_date
from customer as c
    inner join lateral (
        select r.customer_id, r.rental_id, r.rental_date
        from rental as r
        where r.customer_id = c.customer_id
        order by r.rental_date desc
        limit 3
    ) as d
        on c.customer_id = d.customer_id
;
```
#### Common Table Expressions
CTEs are a really good way to organize SQL query code when it starts getting really hairy with subqueries. They don’t introduce any new functionality per se, but they are a really good way to improve readability of complicated SQL queries.

What we can do is take a table subquery and pull it out to the top of the query. For example,
```sql
select *
from (
    select
        title,
        rental_rate,
        replacement_cost,
        ceil(replacement_cost / rental_rate) as break_even
    from film
) as T
where break_even > 30
```
can become
```sql
with film_stats as (
    select
        title,
        rental_rate,
        replacement_cost,
        ceil(replacement_cost / rental_rate) as break_even
    from film
) select *
from film_stats
where break_even > 30
```
Multiple CTEs can be specified with a simple comma between them.

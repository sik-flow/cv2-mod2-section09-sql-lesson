
<h1>Table of Contents<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Lesson-Structure" data-toc-modified-id="Lesson-Structure-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Lesson Structure</a></span><ul class="toc-item"><li><span><a href="#Questions" data-toc-modified-id="Questions-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>Questions</a></span></li><li><span><a href="#Outcomes" data-toc-modified-id="Outcomes-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>Outcomes</a></span></li><li><span><a href="#Objectives" data-toc-modified-id="Objectives-1.3"><span class="toc-item-num">1.3&nbsp;&nbsp;</span>Objectives</a></span></li><li><span><a href="#Outline" data-toc-modified-id="Outline-1.4"><span class="toc-item-num">1.4&nbsp;&nbsp;</span>Outline</a></span></li></ul></li><li><span><a href="#(5-min)-Warm-Up" data-toc-modified-id="(5-min)-Warm-Up-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>(5 min) Warm Up</a></span></li><li><span><a href="#Demonstrating-SQLite-&amp;-Python" data-toc-modified-id="Demonstrating-SQLite-&amp;-Python-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Demonstrating SQLite &amp; Python</a></span><ul class="toc-item"><li><span><a href="#Connecting-to-our-sqlite-db-using-sqlite3" data-toc-modified-id="Connecting-to-our-sqlite-db-using-sqlite3-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Connecting to our sqlite db using sqlite3</a></span></li><li><span><a href="#Listing-the-tables-in-our-db" data-toc-modified-id="Listing-the-tables-in-our-db-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Listing the tables in our db</a></span></li><li><span><a href="#Now--select-everything-from-the-employees-table-just-to-get-a-feel-for-it" data-toc-modified-id="Now--select-everything-from-the-employees-table-just-to-get-a-feel-for-it-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Now  select everything from the employees table just to get a feel for it</a></span></li><li><span><a href="#Now-let's-load-this-into-a-dataframe-using-the-.read_sql-method,-we'll-use-some-of-the-same-components-from-above." data-toc-modified-id="Now-let's-load-this-into-a-dataframe-using-the-.read_sql-method,-we'll-use-some-of-the-same-components-from-above.-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Now let's load this into a dataframe using the <code>.read_sql</code> method, we'll use some of the same components from above.</a></span></li><li><span><a href="#(3-min)-What-are-the-pros-and-cons-of-loading-a-sql-query-into-a-dataframe?" data-toc-modified-id="(3-min)-What-are-the-pros-and-cons-of-loading-a-sql-query-into-a-dataframe?-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>(3 min) What are the pros and cons of loading a sql query into a dataframe?</a></span></li><li><span><a href="#A-throwback-favorite" data-toc-modified-id="A-throwback-favorite-3.6"><span class="toc-item-num">3.6&nbsp;&nbsp;</span>A throwback favorite</a></span></li><li><span><a href="#WHY-SHOULD-YOU-ALWAYS-SPECIFY-YOUR-PRIMARY-KEYS!?" data-toc-modified-id="WHY-SHOULD-YOU-ALWAYS-SPECIFY-YOUR-PRIMARY-KEYS!?-3.7"><span class="toc-item-num">3.7&nbsp;&nbsp;</span>WHY SHOULD YOU ALWAYS SPECIFY YOUR PRIMARY KEYS!?</a></span></li></ul></li><li><span><a href="#Exercises" data-toc-modified-id="Exercises-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Exercises</a></span><ul class="toc-item"><li><span><a href="#Query-Exercise-1" data-toc-modified-id="Query-Exercise-1-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Query Exercise 1</a></span></li><li><span><a href="#Query-Exercise-2" data-toc-modified-id="Query-Exercise-2-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Query Exercise 2</a></span></li><li><span><a href="#Query-Exercise-3" data-toc-modified-id="Query-Exercise-3-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Query Exercise 3</a></span></li><li><span><a href="#Query-Exercise-4" data-toc-modified-id="Query-Exercise-4-4.4"><span class="toc-item-num">4.4&nbsp;&nbsp;</span>Query Exercise 4</a></span></li><li><span><a href="#Query-Exercise-5" data-toc-modified-id="Query-Exercise-5-4.5"><span class="toc-item-num">4.5&nbsp;&nbsp;</span>Query Exercise 5</a></span></li><li><span><a href="#Great!-It's-working.-Let's-add-some-group-by-to-remove-duplicates!" data-toc-modified-id="Great!-It's-working.-Let's-add-some-group-by-to-remove-duplicates!-4.6"><span class="toc-item-num">4.6&nbsp;&nbsp;</span>Great! It's working. Let's add some group by to remove duplicates!</a></span></li></ul></li></ul></div>

# Lesson Structure

## Questions

## Outcomes

YWBAT
- perform a query that is ordered by a value
- load query results as a dataframe
- build functions to perform queries
- execute a query using multiple joins

## Objectives

- write an order by query on a given table
- convert the customers table to a dataframe using pandas and sqlite3
- compare and contrast using a dataframe vs using the results as a list, from a query
- write a single join query
- write a multiple join query

## Outline

* Take Questions
* Warm Up
* Connect to our sqlite db
* Compare and contrast reading a query as a list vs reading a query as a dataframe
* Practice various queries
* Complete a Join Query
* Complete a query using multiple Joins
* wrap up

# (5 min) Warm Up

Given the following 'Employees' table, write a query that completes the following task.  Send your query to me in a private chat here in zoom.

Expected Result:
- IdNum, LName, FName and Salary
- Order it by salary, starting with the highest salary

<img src="images/table.png" width=500 align="center">

<details>
    <summary>Solution</summary>
    
```SQL
SELECT IdNum, LName, FName, Salary
FROM Employees
ORDER BY Salary DESC
```
</details>

# Demonstrating SQLite & Python


```python
import pandas as pd
import numpy as np

import sqlite3

import matplotlib.pyplot as plt
```

## Connecting to our sqlite db using sqlite3


```python
conn = sqlite3.connect('data.sqlite')
cursor = conn.cursor()
```

## Listing the tables in our db


```python
# table_names to be a list of my table_names
query = "SELECT name FROM sqlite_master WHERE type='table';"
res = cursor.execute(query).fetchall()
print(res) # Notice these are tuples, let's extract only the names in the next cell
```


```python
table_names = [r[0] for r in res]
table_names
```

## Now  select everything from the employees table just to get a feel for it


```python
query = 'select * from employees;'
res = cursor.execute(query).fetchall()
res[:2]
```

## Now let's load this into a dataframe using the `.read_sql` method, we'll use some of the same components from above. 


```python
query = 'select * from employees;'
df = pd.read_sql(query, conn)
df.head() # Much better and more readable! 
```

## (3 min) What are the pros and cons of loading a sql query into a dataframe?
- Pros
    - 

- Cons
    -  


```python
def load_df(table_name=None, conn=None):
    query = 'select * from {}'.format(table_name)
    df = pd.read_sql(query, conn)
    return df
```

## A throwback favorite 


```python
d = {} # table_name: dataframe of table
for table_name in table_names:
    d[table_name] = load_df(table_name, conn)
```

## WHY SHOULD YOU ALWAYS SPECIFY YOUR PRIMARY KEYS!?

[Discussion here]

# Exercises

## Query Exercise 1

Create the following table using the customers table and the orders table

<img src="images/query1.png" width="500"/>


```python
# First look at the customers dataframe 


# Then look at the orders dataframe
```

<details>
    <summary>Query 1 - Solution</summary>
    
```python
    query = """
    SELECT c.customerName, c.phone, c.state, o.orderDate FROM
    customers as c
    JOIN orders as o using (customerNumber);
    """
```
</details>


```python
query = """SELECT
        """
```


```python
join_1 = pd.read_sql(query, conn)
join_1.head()
```

## Query Exercise 2

Create a query that results in a table with the every customer number for an employee along with the employee name and number.

Ex:

<img src="images/query2.png" width="500"/>


```python
# Work here
```


```python
query = """
        SELECT
        """

pd.read_sql(query, conn).head()
```

<details>
    <summary> Query 2 - Solution</summary>
    
```python
query = """
    select e.firstname, e.lastname, e.employeenumber, c.customerNumber
    from employees as e
    join customers as c
    on e.employeenumber = c.salesrepemployeenumber;"""
```
    
</details>


```python
# Now let's take this and incorporate the next table orders
orders_df.head(1)
```

## Query Exercise 3

Create a query that results in a table with every order number for every customer number for an employee along with the employee name and number.

Ex:

<img src="images/query3.png" width="500"/>


```python
# Work here
```


```python
query = """
        SELECT
        """

pd.read_sql(query, conn).head()
```

<details>
    <summary> Query 3 - Solution</summary>

```python
query = """
    select e.firstname, e.lastname, e.employeenumber, c.customerNumber, o.orderNumber
    from employees as e
    join customers as c
    on e.employeenumber = c.salesrepemployeenumber
    join orders as o
    on o.customerNumber = c.customerNumber;"""
```
    
</details>

## Query Exercise 4

Write a query that results in the following:

Employee First and Last Name, order number and each product code for that order. 

*There should be a row for each product code*

Screenshot:

<img src="images/query4.png" width="500"/>


```python
# work here
query = """
        select
        """

pd.read_sql(query, conn).head(2)
```

<details>
    <summary>Query 4 Solution</summary>
    
```python
query = """
    select e.firstname, e.lastname, o.orderNumber, od.productCode
    from employees as e
    join customers as c
    on e.employeenumber = c.salesrepemployeenumber
    join orders as o
    on o.customerNumber = c.customerNumber
    join orderdetails as od
    on od.orderNumber = o.orderNumber;"""
    ```
        
</details>

## Query Exercise 5

Write a query that results in the following:

Employee First and Last Name and each product name they sold, add an alias to match the image below

*There should be a row for each product name*

Screenshot:

<img src="images/query5.png" width="500"/>


```python
# work here
query = """
        select
        """

pd.read_sql(query, conn).head(2)
```

<details>
    <summary>Query 5 Solution</summary>
    
```python
query = """
    select e.firstname, e.lastname, p.productName
    from employees as e
    join customers as c
    on e.employeenumber = c.salesrepemployeenumber
    join orders as o
    on o.customerNumber = c.customerNumber
    join orderdetails as od
    on od.orderNumber = o.orderNumber
    join products as p
    on p.productCode = od.productCode;"""
    ```
        
</details>

## Great! It's working. Let's add some group by to remove duplicates!

**Add aliases to result in the following**

<img src="images/query5.png" width="500"/>


```python
# remember the alias' from above

query = '''SELECT ...'''

final_df = pd.read_sql(query, conn)
final_df.head(2)
```

<details>
<summary>Final Query Solution</summary>

```python
query = """
        select e.firstname as fn, e.lastname as ln, p.productName as pn
        from employees as e
        join customers as c
        on e.employeenumber = c.salesrepemployeenumber
        join orders as o
        on o.customerNumber = c.customerNumber
        join orderdetails as od
        on od.orderNumber = o.orderNumber
        join products as p
        on p.productCode = od.productCode
        group by fn, ln, pn;"""

```

</details>

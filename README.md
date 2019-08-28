
### Questions

### Outcomes
YWBAT
- perform a query that is ordered by a value
- load query results as a dataframe
- build functions to perform queries
- execute a query using multiple joins

### Outline
* Take Questions
* Warm Up
* Connect to our sqlite db
* Compare and contrast reading a query as a list vs reading a query as a dataframe
* Practice various queries
* Complete a Join Query
* Complete a query using multiple Joins
* wrap up

### (5 min) Warm Up
Given the following 'Employees' table, write a query that completes the following task.  Send your query to me in a private chat here in zoom.

Expected Result:
- IdNum, LName, FName and Salaryj
- Order it by salary, starting with the highest salary
![](images/table.png)

<details>
    <summary>Solution</summary>
    
    ```
    SELECT IdNum, LName, FName, Salary
    FROM Employees
    ORDER BY Salary DESC
    ```
</details>


```python
# Getting connected to 
```


```python
import pandas as pd
import numpy as np

import sqlite3

import matplotlib.pyplot as plt
```

### Connecting to our sqlite db using sqlite3


```python
conn = sqlite3.connect('data.sqlite')
cursor = conn.cursor()
```

### Listing the tables in our db


```python
# table_names to be a list of my table_names
query = "SELECT name FROM sqlite_master WHERE type='table';"
res = cursor.execute(query).fetchall()
print(res) # Notice these are tuples, let's extract only the names in the next cell
```

    [('orderdetails',), ('payments',), ('offices',), ('customers',), ('orders',), ('productlines',), ('products',), ('employees',), ('contacts',), ('contacts2',)]



```python
table_names = [r[0] for r in res]
table_names
```




    ['orderdetails',
     'payments',
     'offices',
     'customers',
     'orders',
     'productlines',
     'products',
     'employees',
     'contacts',
     'contacts2']



### Now  select everything from the employees table just to get a feel for it


```python
query = 'select * from employees;'
res = cursor.execute(query).fetchall()
res[:2]
```




    [('1002',
      'Murphy',
      'Diane',
      'x5800',
      'dmurphy@classicmodelcars.com',
      '1',
      '',
      'President'),
     ('1056',
      'Patterson',
      'Mary',
      'x4611',
      'mpatterso@classicmodelcars.com',
      '1',
      '1002',
      'VP Sales')]



### Now let's load this into a dataframe using the `.read_sql` method, we'll use some of the same components from above. 


```python
query = 'select * from employees;'
df = pd.read_sql(query, conn)
df.head() # Much better and more readable! 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>employeeNumber</th>
      <th>lastName</th>
      <th>firstName</th>
      <th>extension</th>
      <th>email</th>
      <th>officeCode</th>
      <th>reportsTo</th>
      <th>jobTitle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1002</td>
      <td>Murphy</td>
      <td>Diane</td>
      <td>x5800</td>
      <td>dmurphy@classicmodelcars.com</td>
      <td>1</td>
      <td></td>
      <td>President</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1056</td>
      <td>Patterson</td>
      <td>Mary</td>
      <td>x4611</td>
      <td>mpatterso@classicmodelcars.com</td>
      <td>1</td>
      <td>1002</td>
      <td>VP Sales</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1076</td>
      <td>Firrelli</td>
      <td>Jeff</td>
      <td>x9273</td>
      <td>jfirrelli@classicmodelcars.com</td>
      <td>1</td>
      <td>1002</td>
      <td>VP Marketing</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1088</td>
      <td>Patterson</td>
      <td>William</td>
      <td>x4871</td>
      <td>wpatterson@classicmodelcars.com</td>
      <td>6</td>
      <td>1056</td>
      <td>Sales Manager (APAC)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1102</td>
      <td>Bondur</td>
      <td>Gerard</td>
      <td>x5408</td>
      <td>gbondur@classicmodelcars.com</td>
      <td>4</td>
      <td>1056</td>
      <td>Sale Manager (EMEA)</td>
    </tr>
  </tbody>
</table>
</div>



### (3 min) What are the pros and cons of loading a sql query into a dataframe?
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

### A throwback favorite 


```python
d = {} # table_name: dataframe of table
for table_name in table_names:
    d[table_name] = load_df(table_name, conn)
```

### WHY SHOULD YOU ALWAYS SPECIFY YOUR PRIMARY KEYS!?

SQL will get super mad if we try and produce duplicates. It keeps us safe. 


```python
customers_df.head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerNumber</th>
      <th>customerName</th>
      <th>contactLastName</th>
      <th>contactFirstName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>addressLine2</th>
      <th>city</th>
      <th>state</th>
      <th>postalCode</th>
      <th>country</th>
      <th>salesRepEmployeeNumber</th>
      <th>creditLimit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>103</td>
      <td>Atelier graphique</td>
      <td>Schmitt</td>
      <td>Carine</td>
      <td>40.32.2555</td>
      <td>54, rue Royale</td>
      <td></td>
      <td>Nantes</td>
      <td></td>
      <td>44000</td>
      <td>France</td>
      <td>1370</td>
      <td>21000.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>112</td>
      <td>Signal Gift Stores</td>
      <td>King</td>
      <td>Jean</td>
      <td>7025551838</td>
      <td>8489 Strong St.</td>
      <td></td>
      <td>Las Vegas</td>
      <td>NV</td>
      <td>83030</td>
      <td>USA</td>
      <td>1166</td>
      <td>71800.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
orders_df = pd.read_sql("select * from orders", conn)
orders_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>orderDate</th>
      <th>requiredDate</th>
      <th>shippedDate</th>
      <th>status</th>
      <th>comments</th>
      <th>customerNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>2003-01-06</td>
      <td>2003-01-13</td>
      <td>2003-01-10</td>
      <td>Shipped</td>
      <td></td>
      <td>363</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10101</td>
      <td>2003-01-09</td>
      <td>2003-01-18</td>
      <td>2003-01-11</td>
      <td>Shipped</td>
      <td>Check on availability.</td>
      <td>128</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10102</td>
      <td>2003-01-10</td>
      <td>2003-01-18</td>
      <td>2003-01-14</td>
      <td>Shipped</td>
      <td></td>
      <td>181</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10103</td>
      <td>2003-01-29</td>
      <td>2003-02-07</td>
      <td>2003-02-02</td>
      <td>Shipped</td>
      <td></td>
      <td>121</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10104</td>
      <td>2003-01-31</td>
      <td>2003-02-09</td>
      <td>2003-02-01</td>
      <td>Shipped</td>
      <td></td>
      <td>141</td>
    </tr>
  </tbody>
</table>
</div>




```python
orders_df.shape, join_1.shape
```




    ((326, 7), (326, 4))




```python
# let's do a join on customer number
# customerName, customer.phone,  customer state, customer order-date
query = """
        SELECT c.customerName, c.phone, c.state, o.orderDate FROM
        customers as c
        JOIN orders as o using (customerNumber);
        """
```


```python
join_1 = pd.read_sql(query, conn)
join_1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerName</th>
      <th>phone</th>
      <th>state</th>
      <th>orderDate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Atelier graphique</td>
      <td>40.32.2555</td>
      <td></td>
      <td>2003-05-20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Atelier graphique</td>
      <td>40.32.2555</td>
      <td></td>
      <td>2004-09-27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Atelier graphique</td>
      <td>40.32.2555</td>
      <td></td>
      <td>2004-11-25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Signal Gift Stores</td>
      <td>7025551838</td>
      <td>NV</td>
      <td>2003-05-21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Signal Gift Stores</td>
      <td>7025551838</td>
      <td>NV</td>
      <td>2004-08-06</td>
    </tr>
  </tbody>
</table>
</div>



### Level Up: Display the names of each product each employee has sold


```python
employees_df = pd.read_sql("select * from employees", conn)
employees_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>employeeNumber</th>
      <th>lastName</th>
      <th>firstName</th>
      <th>extension</th>
      <th>email</th>
      <th>officeCode</th>
      <th>reportsTo</th>
      <th>jobTitle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1002</td>
      <td>Murphy</td>
      <td>Diane</td>
      <td>x5800</td>
      <td>dmurphy@classicmodelcars.com</td>
      <td>1</td>
      <td></td>
      <td>President</td>
    </tr>
  </tbody>
</table>
</div>




```python
customers_df.head(1)
```


    -------------------------------------------------------------------------

    NameError                               Traceback (most recent call last)

    <ipython-input-8-227c7b2c39ed> in <module>
    ----> 1 customers_df.head(1)
    

    NameError: name 'customers_df' is not defined



```python
query1 = """
        create temporary table emp_cust as
        select e.lastName, e.firstName, c.salesRepEmployeeNumber, c.customerNumber
        from employees as e
        join customers as c 
        on e.employeeNumber = c.salesRepEmployeeNumber;
        """

query2 = """
         create temporary table eco as 
         select ec.customerNumber, ec.lastName, ec.firstName, o.orderNumber
         from emp_cust as ec
         join orders as o
         using (customerNumber);"""


# cursor.execute(query1).fetchall()
cursor.execute(query2).fetchall()
```




    []




```python
query3 = """
         create temporary table ecod as
         select eco.orderNumber, eco.lastName, eco.firstName, eco.orderNumber, od.productCode
         from eco
         join orderdetails as od
         using (orderNumber);"""
# cursor.execute(query3).fetchall()
```


```python
orders_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>orderDate</th>
      <th>requiredDate</th>
      <th>shippedDate</th>
      <th>status</th>
      <th>comments</th>
      <th>customerNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>2003-01-06</td>
      <td>2003-01-13</td>
      <td>2003-01-10</td>
      <td>Shipped</td>
      <td></td>
      <td>363</td>
    </tr>
  </tbody>
</table>
</div>




```python
query = 'select * from eco;'
cursor.execute(query).fetchall()[:3]
```




    [('124', 'Jennings', 'Leslie', '10113'),
     ('124', 'Jennings', 'Leslie', '10135'),
     ('124', 'Jennings', 'Leslie', '10142')]




```python
orderdetails_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>productCode</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>S18_1749</td>
      <td>30</td>
      <td>136.00</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python
query = """
        select eco.firstName, eco.lastName, eco.productCode, p.productName
        from ecod as eco
        join products as p
        using (productCode)
        group by eco.firstName, eco.lastName, p.productName
        order by eco.firstName, p.productName"""
cursor.execute(query).fetchall()[:3]
```




    [('Andy', 'Fixter', 'S18_3136', '18th Century Vintage Horse Carriage'),
     ('Andy', 'Fixter', 'S24_2841', '1900s Vintage Bi-Plane'),
     ('Andy', 'Fixter', 'S24_4278', '1900s Vintage Tri-Plane')]




```python
products_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
    </tr>
  </tbody>
</table>
</div>



### What did you learn?
* PRAGMA
* Making long strings
* Temporary tables
* pd.read_sql
* dictionary key:value -> tablename: dataframes

# Subquery Section


```python
### let's look at employee customer tables first
employees_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>employeeNumber</th>
      <th>lastName</th>
      <th>firstName</th>
      <th>extension</th>
      <th>email</th>
      <th>officeCode</th>
      <th>reportsTo</th>
      <th>jobTitle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1002</td>
      <td>Murphy</td>
      <td>Diane</td>
      <td>x5800</td>
      <td>dmurphy@classicmodelcars.com</td>
      <td>1</td>
      <td></td>
      <td>President</td>
    </tr>
  </tbody>
</table>
</div>




```python
customers_df = pd.read_sql("select * from customers;", conn)
customers_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerNumber</th>
      <th>customerName</th>
      <th>contactLastName</th>
      <th>contactFirstName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>addressLine2</th>
      <th>city</th>
      <th>state</th>
      <th>postalCode</th>
      <th>country</th>
      <th>salesRepEmployeeNumber</th>
      <th>creditLimit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>103</td>
      <td>Atelier graphique</td>
      <td>Schmitt</td>
      <td>Carine</td>
      <td>40.32.2555</td>
      <td>54, rue Royale</td>
      <td></td>
      <td>Nantes</td>
      <td></td>
      <td>44000</td>
      <td>France</td>
      <td>1370</td>
      <td>21000.00</td>
    </tr>
  </tbody>
</table>
</div>



# Query 0


```python
query = """
        select e.firstname, e.lastname, e.employeenumber, c.customerNumber
        from employees as e
        join customers as c
        on e.employeenumber = c.salesrepemployeenumber;"""

pd.read_sql(query, conn).head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>employeeNumber</th>
      <th>customerNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1165</td>
      <td>124</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1165</td>
      <td>129</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Now let's take this and incorporate the next table orders
orders_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>orderDate</th>
      <th>requiredDate</th>
      <th>shippedDate</th>
      <th>status</th>
      <th>comments</th>
      <th>customerNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>2003-01-06</td>
      <td>2003-01-13</td>
      <td>2003-01-10</td>
      <td>Shipped</td>
      <td></td>
      <td>363</td>
    </tr>
  </tbody>
</table>
</div>



# Query 1


```python
# add orders to previous queries joining on customernumber
query = """
        select e.firstname, e.lastname, e.employeenumber, c.customerNumber, o.orderNumber
        from employees as e
        join customers as c
        on e.employeenumber = c.salesrepemployeenumber
        join orders as o
        on o.customerNumber = c.customerNumber;"""

pd.read_sql(query, conn).head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>employeeNumber</th>
      <th>customerNumber</th>
      <th>orderNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1165</td>
      <td>124</td>
      <td>10113</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1165</td>
      <td>124</td>
      <td>10135</td>
    </tr>
  </tbody>
</table>
</div>




```python
# now let's incororate the orderdetails table
orderdetails_df.head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>orderNumber</th>
      <th>productCode</th>
      <th>quantityOrdered</th>
      <th>priceEach</th>
      <th>orderLineNumber</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10100</td>
      <td>S18_1749</td>
      <td>30</td>
      <td>136.00</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



# Query 2


```python
# adding product code, removing employee number and customer number
query = """
        select e.firstname, e.lastname, o.orderNumber, od.productCode
        from employees as e
        join customers as c
        on e.employeenumber = c.salesrepemployeenumber
        join orders as o
        on o.customerNumber = c.customerNumber
        join orderdetails as od
        on od.orderNumber = o.orderNumber;"""

pd.read_sql(query, conn).head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>orderNumber</th>
      <th>productCode</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>10113</td>
      <td>S12_1666</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>10113</td>
      <td>S18_1097</td>
    </tr>
  </tbody>
</table>
</div>




```python
# now let's investigate the product table
pd.read_sql("select * from products", conn).head(1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>productCode</th>
      <th>productName</th>
      <th>productLine</th>
      <th>productScale</th>
      <th>productVendor</th>
      <th>productDescription</th>
      <th>quantityInStock</th>
      <th>buyPrice</th>
      <th>MSRP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>S10_1678</td>
      <td>1969 Harley Davidson Ultimate Chopper</td>
      <td>Motorcycles</td>
      <td>1:10</td>
      <td>Min Lin Diecast</td>
      <td>This replica features working kickstand, front...</td>
      <td>7933</td>
      <td>48.81</td>
      <td>95.70</td>
    </tr>
  </tbody>
</table>
</div>



# Query 3 - Final


```python
# Now let's add product name with some final group by to remove duplicates
# removing order number and product code
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

pd.read_sql(query, conn).head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>productName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1958 Setra Bus</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>1940 Ford Pickup Truck</td>
    </tr>
  </tbody>
</table>
</div>



### Great! It's working. Let's add some group bys for duplicates!


```python
# alias the columns and add group by at the bottom
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

final_df = pd.read_sql(query, conn)
final_df.head(2)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>fn</th>
      <th>ln</th>
      <th>pn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Andy</td>
      <td>Fixter</td>
      <td>18th Century Vintage Horse Carriage</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Andy</td>
      <td>Fixter</td>
      <td>1900s Vintage Bi-Plane</td>
    </tr>
  </tbody>
</table>
</div>




```python
final_df.shape, final_df.drop_duplicates().shape
```




    ((1368, 3), (1368, 3))




```python

```

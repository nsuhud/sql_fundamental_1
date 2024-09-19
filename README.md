# SQL Fundamental 1

## What is SQL?
Structured Query Language (SQL) is a programming language used to store and process data in relational databases. Information is organised into tables, with rows and columns representing various attributes and their relationships. SQL statements are employed to store, update, delete, search, and retrieve data, as well as to maintain and optimise database performance.

## Basic Structure of SQL Syntax:
SQL syntax is built on several core components that define how queries are structured and executed. The following are a brief explanation of the core components of SQL syntax.

**1. Keywords:** are reserved words in SQL that has a predefined meaning to instruct the database (e.g., `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `FROM`, `WHERE`, `JOIN`, etc.). Keywords are usually written in uppercase but are not case-sensitive.

**2. Clauses:** are components of an SQL query that performs a specific function, such as retrieving data, filtering results, or defining conditions (e.g., `FROM`, `WHERE`, `ORDER BY`).

**3. Expressions:** Combine column names, operators, and functions to generate values (e.g., salary > 50000).

**4. Operators:** Symbols or keywords used for operations that modify the behaviour of a clause or expression (e.g., =, >, <, `BETWEEN`, `DESC`).

## Order of operations:
In SQL, the order of operations and the order of keywords, clauses, expressions and operators in a query matter because SQL processes queries step by step. For example, clauses like `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, and `LIMIT` must follow a specific order for the query to work correctly.

**Example:**

`FROM`: Specifies the table(s) to retrieve data from.

`WHERE`: Filters rows based on conditions.

`GROUP BY`: Groups rows with similar values.

`HAVING`: Filters groups based on conditions.

`ORDER BY`: Sorts the result set.

`LIMIT`: Restricts the number of rows returned.

If we do not write the order of these clauses correctly, the query will either fail to execute or produce incorrect results. This will become clearer as we go through the exercise.

## How to run SQL?
To use SQL effectively, we need specialised software that connects to the database, as SQL cannot manage the data directly by itself.

SQL functions like **a set of instructions**, telling the database what actions to perform, such as "find this data," "add this information," or "calculate this value." The **database**, in turn, is like a storage room that holds all the information, much like a filing cabinet with numerous folders. This data is organised into tables, similar to rows and columns in a spreadsheet. Lastly, the database **software** acts as the manager, understanding SQL and serving as the intermediary. It listens to our SQL commands and communicates with the storage room (the database) to retrieve or modify the data.

Generally, programmers use popular database software, such as MySQL and PostgreSQL, to run SQL commands. However, for those eager to learn SQL without investing in complex or costly database systems, there is a simpler alternative: [SQLite](https://www.sqlite.org/).

**SQLite** is lightweight, easy to set up, and doesn’t require any dedicated server. This makes it ideal for beginners or anyone who wants to practice SQL without the hassle of managing a large database system. It's free, and we can run it directly on our personal computers, making it a great starting point for learning SQL.

### 1.1. Setting Up SQLite for Jupyter in Anaconda Navigator
Let's begin by opening [Anaconda Navigator](https://www.anaconda.com/download) and launching JupyterLab, followed by installing [SQLAlchemy](https://www.sqlalchemy.org/) and [IPython-SQL](https://pypi.org/project/ipython-sql/) using the following commands.

```python
conda install -c anaconda sqlalchemy
conda install -c conda-forge ipython-sql
```

### 1.2. Loading the SQL extension and connecting to SQLite
Load the IPython-SQL extension by typing the command below.

```python
%load_ext sql
```

To set up a temporary SQLite database in memory, type the following command in a new cell and run it.

```python
%sql sqlite://
```
<img width="818" alt="2" src="https://github.com/user-attachments/assets/d2396ed0-3d8f-4e89-80af-5af65f251c3e">

## 2. Dataset preparation
The first part of the exercise will use artificial Human Resources data consisting of employees' personal details and job-related information. This data is available in **five separate CSV files**, which can be downloaded from the section above.

<ol>
    <li>Employees.csv</li>
    <li>Departments.csv</li>
    <li>Jobs.csv</li>
    <li>JobsHistory.csv</li>
    <li>Locations.csv</li>
</ol>

We will also use [pandas](https://pandas.pydata.org/about/) library, an open-source software library created for Python that is essential for data science and data analysis workflows, making it easier to clean, explore, and manipulate large datasets efficiently. Run the following command to install Pandas.

```python
import pandas as pd
```

### 2.1. Create headers for each column
Now we need to create header lists since the CSV files do not come with headers (column names).

```python
e_head = ["EMP_ID","F_NAME","L_NAME","SSN","B_DATE","SEX","ADDRESS","JOB_ID","SALARY","MANAGER_ID","DEP_ID"]
dept_head = ["DEPT_ID_DEP","DEP_NAME","MANAGER_ID","LOC_ID"]
j_head = ["JOB_IDENT","JOB_TITLE","MIN_SALARY","MAX_SALARY"]
jh_head = ["EMPL_ID","START_DATE","JOBS_ID","DEPT_ID"]
loc_head = ["LOCT_ID","DEP_ID_LOC"]
```

We will now use pandas to read our CSV files and load the data into a pandas DataFrame, while simultaneously **adding the relevant headers** to each file by using the header lists we have created above. The format of the command is as follows:

`df = pd.read_csv(filepath, names = header)`

```python
employees = pd.read_csv("/insert-your-file-path-here/Employees.csv", names = e_head)
departments = pd.read_csv("/insert-your-file-path-here/Departments.csv", names = dept_head)
jobs = pd.read_csv("/insert-your-file-path-here/Jobs.csv", names = j_head)
jhistory = pd.read_csv("/insert-your-file-path-here/JobsHistory.csv", names = jh_head)
locations = pd.read_csv("/insert-your-file-path-here/Locations.csv", names = loc_head)
```

### 2.2. Create all necessary tables

Now that we have loaded all of our CSV files into pandas DataFrames, we will use `PERSIST` to make our DataFrames available as SQL tables. `PERSIST` is not a standard SQL command; it is a convenience feature provided by the **ipython-sql** extension to simplify the process of making DataFrames available as SQL tables. The format of the command is as follows:

```python
%sql PERSIST employees;
%sql PERSIST departments;
%sql PERSIST jobs;
%sql PERSIST jhistory;
%sql PERSIST locations;
```
<img width="819" alt="3" src="https://github.com/user-attachments/assets/1ea3aedf-8110-4a9c-b368-e357c7ebd8d7">

### 2.3. Display the tables to see if they have been successfully created

We will use the `SELECT * FROM` statement with the `*` wildcard to retrieve all columns from a specified table.

We will also use the `LIMIT` clause to restrict the number of rows returned by our query, so we won’t have an excessively long list of rows displayed in our notebook.

```python
%sql SELECT * FROM employees LIMIT 5;
```
<img width="933" alt="4" src="https://github.com/user-attachments/assets/b80dc2b4-da3b-4db3-8a72-885e89aa0c5a">

### 3.1. String patterns, Sorting and Grouping

### Exercise 1: STRING PATTERNS
The `LIKE` operator is used to search for a specified pattern within a column. It is often used with the `WHERE` clause to filter records based on partial matches in **text** data.

`LIKE` allows us to match **text** patterns using wildcard characters.

`%` represents zero, one or multiple characters.

`_` represents single character.

**Examples:**

`WHERE` column_name `LIKE` "A%": Finds any values starting with "A".

`WHERE` column_name `LIKE` "%xyz": Finds any values ending with "xyz".

`WHERE` column_name `LIKE` "_bcd": Finds any values with "bcd" starting from the second character.

<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/8c9de8a8-95df-4ab3-9e76-06e773f54630">

**TASK 1.1.:**
Retrieve all employees whose address is in Elgin,IL

```python
%sql SELECT * FROM employees WHERE ADDRESS like "%Elgin,IL%";
```
<img width="934" alt="5" src="https://github.com/user-attachments/assets/c7fc8316-d555-44a2-8ab9-b6a0a0268b53">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/31bf1b0c-4a02-4541-af62-dbc93e3be16f">

**TASK 1.2.:**
Retreive all employees born during the 1970s

```python
%sql SELECT * FROM employees WHERE B_DATE like "%197%";
```
<img width="938" alt="6" src="https://github.com/user-attachments/assets/6a8fc461-c507-4839-8a85-bb80b6e66493">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/a9f47161-52c0-47ec-a8ac-7fb6702e9f89">

**TASK 1.3.:**
Retrieve all employees in Department 5 whose salaries are between 60,000 and 70,000.

We will use the following operators to retrieve the records we need.

The `=` operator for equality between two values.

**Examples:**

`SELECT * FROM employees WHERE department = "Sales";`

The `BETWEEN` operator to filter the result within a certain range of values, inclusive of the boundary values.

**Examples:**

`SELECT * FROM products WHERE price BETWEEN 10 AND 20;`

The `AND` operator to combine two or more conditions in a query, returning the rows where **all** conditions are true.

**Examples:**

`SELECT * FROM employees WHERE department = "Sales" AND salary > 50000;`

```python
%sql SELECT * FROM employees WHERE DEP_ID = 5 AND SALARY BETWEEN 60000 AND 70000;
```
<img width="934" alt="7" src="https://github.com/user-attachments/assets/88438821-00de-498e-9b1d-813995edbdd9">

### Exercise 2: SORTING

**TASK 2.1.:**
Retrieve all employees ordered by department ID

To complete this task, we will use the`ORDER BY` operator. The `ORDER BY` operator is used to sort the result set of a query by one or more columns. By default, it sorts the data in ascending order, but it can also be used to sort in descending order by using the `DESC` operator.

**Examples:**

`SELECT * FROM employees ORDER BY salary DESC;`

This will return all rows from the employees table, sorted by salary in descending order (highest to lowest).

```python
%sql SELECT F_NAME, L_NAME, DEP_ID FROM employees ORDER BY DEP_ID;
```
<img width="946" alt="8" src="https://github.com/user-attachments/assets/5f9c0a4c-0ab2-4602-9e2c-e509e1148a15">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/bd0c8d6d-99a6-40a7-a0b6-a69220d2a0c5">

**TASK 2.2.:**
Retrieve a list of employees in descending order by department ID, within each department ordered alphabetically by last name in descending order.

```python
%sql SELECT F_NAME, L_NAME, DEP_ID FROM employees ORDER BY DEP_ID, L_NAME DESC;
```
<img width="939" alt="9" src="https://github.com/user-attachments/assets/7415db4d-3a79-46b2-8f70-be47c7083fa3">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/a3d32ae8-ffca-408b-803c-d289a6607da6">

**TASK 2.3.:**
Retrieve a list of employees in descending order by DEPARTMENT NAME and within each department ordered alphabetically by LAST NAME in descending order.

**To complete this task, we will write the query as follows:**

**1.** (Optional) We will use `%%sql` to structure our query into paragraphs, which will help illustrate the order of operations.

**2.** We will use aliases for the employees and departments tables to shorten the table names, making the query easier to write and read, especially when working with multiple tables, as in this task. The aliases are:

`employees E`: Assigns the alias E to the employees table. We can then refer to columns in the employees table using this alias, like `E.COLUMN_NAME`.

`departments D`: Similarly, D is an alias for the departments table, so we can refer to its columns using `D.COLUMN_NAME`.

**3.** We will use a cross join condition (implicit join) with the `WHERE` clause to filter the results where the department IDs match. This is how the query links each employee to the correct department.

**4.** We will sort the results using `ORDER BY` clause - first by the department name and then by the employees' last names, both in descending order.

```python
%%sql 
SELECT F_NAME, L_NAME, DEP_ID, DEP_NAME
FROM employees E, departments D
WHERE E.DEP_ID = D.DEPT_ID_DEP
ORDER BY DEP_NAME DESC, L_NAME DESC;
```

This query retrieves the first name **F_NAME**, last name **L_NAME**, department ID **DEP_ID**, and department name **DEP_NAME** of employees. It joins the employees and departments tables using the matching department IDs **E.DEP_ID** and **D.DEPT_ID_DEP**. The results are then sorted first by department name in descending order, and within each department, by last name in descending order.

<img width="942" alt="10" src="https://github.com/user-attachments/assets/28043d39-53e6-4d8e-ad92-7f76876a3bda">

### Exercise 3: GROUPING

**TASK 3.1.:**
For each department ID, retrieve the number of employees.

To complete this task, we will use `COUNT` and `GROUP BY`.

`COUNT()` is an aggregate function used to count the number of rows in a result set that match a specified condition or just the number of rows if no condition is specified.

**Examples:**
`SELECT COUNT(*) AS total_rows FROM employees;`

In this example, `COUNT(*)` counts **all** the rows in the employees table, and the result is given an alias total_rows using `AS`.

`GROUP BY` is a clause used to group rows that have the same values in specified columns. It often works in conjunction with aggregate functions such as `COUNT()`, `SUM()`, `AVG()`, etc.

**Examples:**

`SELECT department, COUNT(*) AS employees_count FROM employees GROUP BY department;`

Now, let's run the following query to complete our task.

```python
%%sql
SELECT DEP_ID, COUNT(*) AS PERSONS
FROM employees
GROUP BY DEP_ID;
```
<img width="943" alt="11" src="https://github.com/user-attachments/assets/ea7e5939-48e0-4d15-b5c0-a6762d84cd29">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/aa1c0608-c3f9-4c1a-8e4c-f122f18a6bc7">

**TASK 3.2.:**
For each dept., retrieve the number of employees and the average salary in the dept.

To complete this task, we will use `AVG()`, a function used to calculate the average (or mean) value of a numeric column in a table.

```python
%%sql
SELECT DEP_ID, DEP_NAME, COUNT(*) AS PERSONS, AVG(SALARY) AS AVG
FROM employees E, departments D
WHERE E.DEP_ID = D.DEPT_ID_DEP
GROUP BY DEP_ID;
```
<img width="945" alt="12" src="https://github.com/user-attachments/assets/ce0482b6-aaaf-4fd8-8876-a66e190c3a82">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/90733f4a-499d-4d80-a444-50651432dd39">

**TASK 3.3.:**
Label the computed columns above as NUM_EMPLOYEES and AVG_SALARY.

Here,we will use `ROUND()` to tidy up the output of our query. `ROUND()` is an aggregate function used to round a numeric value to a specified number of decimal places. The syntax is as follows, where **number** denotes the value to be rounded, and **decimal places** represents the number of decimal places to which the value will be rounded.

`ROUND(number, decimal places)`

Run the following query to obtain the desired result.

```python
%%sql
SELECT DEP_ID, DEP_NAME, COUNT(*) AS NUM_EMPLOYEES, ROUND(AVG(SALARY),2) AS AVG_SALARY
FROM employees E, departments D
WHERE E.DEP_ID = D.DEPT_ID_DEP
GROUP BY DEP_ID;
```
<img width="944" alt="13" src="https://github.com/user-attachments/assets/528f2411-e1f1-4ea4-95fe-4623dff56adf">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/1e4941f8-eb92-48f4-a05c-e055abac31ec">

**TASK 3.4.:**
Order the results by average salary.

```python
%%sql
SELECT DEP_ID, DEP_NAME, COUNT(*) AS NUM_EMPLOYEES, ROUND(AVG(SALARY),2) AS AVG_SALARY
FROM employees E, departments D
WHERE E.DEP_ID = D.DEPT_ID_DEP
GROUP BY DEP_ID
ORDER BY AVG_SALARY;
```
<img width="945" alt="14" src="https://github.com/user-attachments/assets/cb24395c-8d94-4010-94f5-42856de7e4df">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/cad98171-8292-4e21-af24-6672eb60e62e">

**TASK 3.5.:**
Retrieve the number of employees of each department, then compute the average salary of the employees in each department. Limit the results to departments with fewer than 4 employees and order the results by average salary.

Here, we will use the `HAVING` clause to filter data after it has been grouped together, to keep only the groups that meet our criteria.

```python
%%sql
SELECT DEP_ID, DEP_NAME, COUNT(*) AS NUM_EMPLOYEES, ROUND(AVG(SALARY),2) AS AVG_SALARY
FROM employees E, departments D
WHERE E.DEP_ID = D.DEPT_ID_DEP
GROUP BY DEP_ID
HAVING NUM_EMPLOYEES < 4
ORDER BY AVG_SALARY;
```
<img width="944" alt="15" src="https://github.com/user-attachments/assets/4d3534c3-8478-4e71-929f-831a2f714b55">
<img width="906" alt="sapcer" src="https://github.com/user-attachments/assets/b749afb1-5119-4332-88eb-d1589863aa1c">

**End of Part 1 👩🏻‍💻**

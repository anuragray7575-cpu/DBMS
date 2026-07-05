# PostgreSQL Assignment_1  Project

# Employee Management System

## 1. Project Overview

This project demonstrates core PostgreSQL concepts by designing a simple
Employee Management System with two related tables: - Department -
Employee

Each employee belongs to one department.

## 2. Data Model

### Entities

**Department** - Dept_ID (Primary Key) - Dept_Name

**Employee** - Emp_ID (Primary Key) - Name - Salary - Dept_ID (Foreign
Key)

### Relationship

One Department → Many Employees (1:N)

    Department
    ├── Dept_ID (PK)
    └── Dept_Name
            │
            │ 1
            │
            └───────────────< N
                             Employee
                             ├── Emp_ID (PK)
                             ├── Name
                             ├── Salary
                             └── Dept_ID (FK)

## 3. Database Schema

### Department

  Column      Data Type     Constraint
  ----------- ------------- -------------
  Dept_ID     INT           PRIMARY KEY
  Dept_Name   VARCHAR(50)   NOT NULL

### Employee

  Column    Data Type       Constraint
  --------- --------------- --------------------------------------------
  Emp_ID    INT             PRIMARY KEY
  Name      VARCHAR(50)     NOT NULL
  Salary    DECIMAL(10,2)   NOT NULL
  Dept_ID   INT             FOREIGN KEY REFERENCES Department(Dept_ID)

## 4. Why Two Tables?

Normalization avoids duplicate department names. The foreign key
guarantees every employee belongs to a valid department.

## 5. SQL Queries with Explanation

### Create Department Table

``` sql
CREATE TABLE Department(
    Dept_ID INT PRIMARY KEY,
    Dept_Name VARCHAR(50) NOT NULL
);
```

Creates the Department table. `PRIMARY KEY` makes each department
unique.

### Create Employee Table

``` sql
CREATE TABLE Employee(
    Emp_ID INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Salary DECIMAL(10,2) NOT NULL,
    Dept_ID INT,
    FOREIGN KEY (Dept_ID)
        REFERENCES Department(Dept_ID)
);
```

Creates the Employee table and links it to Department using a foreign
key.

### Insert Data

``` sql
INSERT INTO Department VALUES
(101,'Computer Science'),
(102,'Mechanical'),
(103,'Electrical'),
(104,'Civil'),
(105,'Management');
```

``` sql
INSERT INTO Employee VALUES
(1,'Rahul',50000,101),
(2,'Priya',60000,102),
(3,'Amit',55000,101),
(4,'Sneha',70000,103),
(5,'Rohan',45000,104);
```

### Display Employees with Department Name

``` sql
SELECT e.Emp_ID,e.Name,e.Salary,d.Dept_Name
FROM Employee e
INNER JOIN Department d
ON e.Dept_ID=d.Dept_ID;
```

**Explanation:** INNER JOIN combines matching rows from both tables.

### Employee with Highest Salary

``` sql
SELECT *
FROM Employee
WHERE Salary=(SELECT MAX(Salary) FROM Employee);
```

Uses the aggregate function `MAX()` inside a subquery.

### Average Salary

``` sql
SELECT AVG(Salary) AS Average_Salary
FROM Employee;
```

Calculates the mean salary.

### Employee Count by Department

``` sql
SELECT d.Dept_Name,
COUNT(e.Emp_ID) AS Total_Employees
FROM Department d
LEFT JOIN Employee e
ON d.Dept_ID=e.Dept_ID
GROUP BY d.Dept_Name;
```

`LEFT JOIN` ensures departments without employees are still shown.

### Employees Earning Above Average

``` sql
SELECT *
FROM Employee
WHERE Salary>(
SELECT AVG(Salary)
FROM Employee);
```

The subquery computes the average salary first.

### Increase Salary by 10%

``` sql
UPDATE Employee
SET Salary=Salary*1.10
WHERE Emp_ID=2;
```

Updates one employee's salary.

### Delete Employee

``` sql
DELETE FROM Employee
WHERE Emp_ID=5;
```

Removes one employee record.

### Verify

``` sql
SELECT * FROM Employee;
```

Displays the remaining data.

## 6. Concepts Learned

-   Database Design
-   Data Modeling
-   Primary Key
-   Foreign Key
-   One-to-Many Relationship
-   Normalization
-   CREATE TABLE
-   INSERT
-   SELECT
-   INNER JOIN
-   LEFT JOIN
-   GROUP BY
-   COUNT
-   AVG
-   MAX
-   UPDATE
-   DELETE

## 7. Conclusion

This project demonstrates how relational databases organize data
efficiently using normalization and relationships. It also covers
essential SQL operations required for day-to-day database management in
PostgreSQL.

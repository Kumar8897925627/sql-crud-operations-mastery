# sql-crud-operations-mastery
"A comprehensive guide to the CRUD lifecycle in PostgreSQL. Features safe data updates, selective deletions, and the use of Transactions (BEGIN/COMMIT) to ensure data integrity."
-- ==========================================================
-- PROJECT: CRUD Operations End-to-End (Task 6)
-- CONCEPTS: INSERT, SELECT, UPDATE, DELETE, TRANSACTIONS
-- ==========================================================

-- 1. CREATE: Initialize the Employees Table
DROP TABLE IF EXISTS employees;

CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    department VARCHAR(50),
    salary NUMERIC(10, 2),
    hire_date DATE DEFAULT CURRENT_DATE
);

-- 2. INSERT: Bulk Data Entry
INSERT INTO employees (first_name, last_name, department, salary) VALUES 
('Michael', 'Scott', 'Management', 75000),
('Pam', 'Beesly', 'Reception', 45000),
('Jim', 'Halpert', 'Sales', 60000),
('Dwight', 'Schrute', 'Sales', 65000),
('Angela', 'Martin', 'Accounting', 62000),
('Kelly', 'Kapoor', 'Customer Service', 50000);

-- 3. READ: Filtered Data
-- Find high-earners in the Sales department
SELECT * FROM employees 
WHERE department = 'Sales' AND salary > 60000;

-- 4. UPDATE: Modify Records Safely
-- Annual salary increase for the Accounting department
-- Use a WHERE clause to avoid updating everyone!
UPDATE employees 
SET salary = salary + 5000 
WHERE department = 'Accounting';

-- 5. TRANSACTIONS: Safe Deletion Practice
-- We use Transactions so we can "Undo" if we make a mistake.
BEGIN;

    -- Selective Delete
    DELETE FROM employees 
    WHERE first_name = 'Kelly';

    -- Check state before finalizing
    SELECT * FROM employees;

-- If everything looks good, run: COMMIT;
-- If you deleted the wrong person, run: ROLLBACK;
COMMIT;

-- 6. VALIDATE: Final state
SELECT department, SUM(salary) AS department_budget
FROM employees
GROUP BY department;

# 🔄 CRUD Operations: The Data Lifecycle

CRUD is the backbone of almost every application in the world. This project demonstrates how to manage data from the moment it is created until it is removed.

## 🛠️ The Four Pillars of CRUD

1. **CREATE (`INSERT`)**: Adding new records to the database.
2. **READ (`SELECT`)**: Retrieving specific data using filters.
3. **UPDATE (`UPDATE`)**: Modifying existing records (always use a `WHERE` clause!).
4. **DELETE (`DELETE`)**: Removing records.



## 🛡️ Safe Operations & Transactions
One of the most important lessons in SQL is the "Safe Delete." Running a `DELETE` or `UPDATE` without a `WHERE` clause will affect every row in the table. 

To prevent disasters, I utilized **Transactions**:
- `BEGIN`: Starts a "draft" session.
- `COMMIT`: Saves the changes permanently.
- `ROLLBACK`: Undoes the changes if an error is spotted.



## 📊 Business Logic Implemented
- **Bulk Loading:** Populating a staff directory.
- **Salary Adjustments:** Using math operators within an `UPDATE` statement.
- **Departmental Analysis:** Validating changes using `GROUP BY` after updates.

## 🛠️ How to Test
1. Execute the `crud_operations.sql` script in **pgAdmin**.
2. Experiment with the `ROLLBACK` command to see how data is restored after a deletion.

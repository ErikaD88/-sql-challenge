# Employees Database Analysis

## Description

This project models and analyzes a company’s employee database. The database includes information about employees, their salaries, departments, and managers. The goal is to design a normalized schema, load data into PostgreSQL, and execute queries to answer specific data analysis questions.

## 1. Data Modeling

To design the database, an Entity-Relationship Diagram (ERD) approach was used. Six tables (entities) were identified:
    
    1.    employees
    2.    departments
    3.    salaries
    4.    titles
    5.    dept_manager (department managers)
    6.    dept_emp (department employees)

Each table includes:
   
    •    Attributes: Column names with appropriate data types.
    •    Primary Keys: For unique row identification.
    •    Foreign Keys: For establishing relationships between tables.

The ER Diagram visually represents these relationships:
The ER diagram also looks as follows: 

For detailed schema modeling, refer to the modeling documentation:
Employees_db_modeling.pdf

## 2. Data Engineering

A detailed schema file was created to define:
    
    •    Tables and their attributes.
    •    Primary keys and foreign keys for maintaining relationships.
    •    Constraints for data integrity.

The full schema file can be found here:
Employees_db_schema.sql

Table Import Order

To maintain referential integrity, tables must be created and populated in the following order:
    
    1.    titles
    2.    employees
    3.    departments
    4.    dept_manager
    5.    dept_emp
    6.    salaries
    
## 3. Data Import

To load data into PostgreSQL from the provided CSV files, use the following commands:

    \COPY titles FROM 'path/to/titles.csv' WITH (FORMAT csv, HEADER true);
    \COPY employees FROM 'path/to/employees.csv' WITH (FORMAT csv, HEADER true);
    \COPY departments FROM 'path/to/departments.csv' WITH (FORMAT csv, HEADER true);
    \COPY dept_manager FROM 'path/to/dept_manager.csv' WITH (FORMAT csv, HEADER true);
    \COPY dept_emp FROM 'path/to/dept_emp.csv' WITH (FORMAT csv, HEADER true);
    \COPY salaries FROM 'path/to/salaries.csv' WITH (FORMAT csv, HEADER true);

## 4. Data Analysis

After the data was successfully imported, SQL queries were executed to retrieve insights. The full query file is available here:
Employees_db_Query.sql

List employee details with salaries

   SELECT e.emp_no, e.last_name, e.first_name, e.sex, s.salary
   FROM employees e
   JOIN salaries s ON e.emp_no = s.emp_no;

List first name, last name, and hire date for employees hired in 1986.  
   
    SELECT first_name, last_name, hire_date
    FROM employees
    WHERE EXTRACT(YEAR FROM hire_date) = 1986;

List the manager of each department, including department number, department name, manager’s employee number, last name, and first name.

    SELECT dm.dept_no, d.dept_name, e.emp_no, e.last_name, e.first_name
    FROM dept_manager dm
    JOIN employees e ON dm.emp_no = e.emp_no
    JOIN departments d ON dm.dept_no = d.dept_no;

List each employee’s department details, including employee number, last name, first name, and department name.
    
    SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
    FROM dept_emp
    JOIN employees ON dept_emp.emp_no = employees.emp_no
    JOIN departments ON dept_emp.dept_no = departments.dept_no;

List first name, last name, and sex for employees whose first name is “Hercules” and last names begin with “B”.
    
    SELECT first_name, last_name, sex
    FROM employees
    WHERE first_name = 'Hercules'
    AND last_name LIKE 'B%';

List all employees in the Sales department, including their employee number, last name, first name, and department name.

    SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
    FROM dept_emp
    JOIN employees ON dept_emp.emp_no = employees.emp_no
    JOIN departments ON dept_emp.dept_no = departments.dept_no
    WHERE departments.dept_name = 'Sales';

List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.

    SELECT dept_emp.emp_no, employees.last_name, employees.first_name, departments.dept_name
    FROM dept_emp
    JOIN employees ON dept_emp.emp_no = employees.emp_no
    JOIN departments ON dept_emp.dept_no = departments.dept_no
    WHERE departments.dept_name IN ('Sales', 'Development');

List the frequency counts of employee last names in descending order.
    
    SELECT last_name, COUNT(last_name) AS frequency
    FROM employees
    GROUP BY last_name
    ORDER BY frequency DESC;

## 5. Results

The analysis provided the following insights:
    
    •    Employee details with salaries.
    •    Employees hired in a specific year.
    •    Department managers and their respective details.
    •    Employees working in Sales and Development departments.
    •    Frequency counts of employee last names.
    
## 6. Files

    •    Employees_db_modeling.pdf - ERD Modeling Documentation
    •    Employees_db_schema.sql - Table Schema
    •    Employees_db_Query.sql - SQL Analysis Queries
    
## 7. Setup Instructions

To replicate the project:
    
    1. Install PostgreSQL: PostgreSQL.org.
    2. Create the Database:
       CREATE DATABASE employees_db;
    3. Run the Schema File:
       psql -U postgres -d employees_db -f Employees_db_schema.sql
    4. Import CSV Data: Use the COPY commands provided in the Data Import section.

## 8. Conclusion

This project demonstrates:
    
    •    Data Modeling using ERDs.
    •    Data Engineering with PostgreSQL schema creation and CSV imports.
    •    Data Analysis using SQL queries to extract meaningful insights.


# Pewlett-Hackard-Analysis

## **Introduction to the Analysis**

For this project, Bobby had to organize and group large amounts of data in order to provide **Pewlett-Hackard** with information regarding the number of employees that will be eligible for retirement based on their age, hire date, and current working status. He then, had to create another table to determine how many participants will also be eligible for the mentorship program, which is based on their date of birth. 

In order to create this table, the following information had to be determined first:
  - **Who is eligible for retirement?** Employees whose birth date fell between January 1, 1952 and December 31, 1955
  - **When did they began working?** Employees who met the first requirement where then grouped by the date they were hired. In order to be elgibible, their hire date had to be        between January 1, 1985 and December 31, 1988.
  - **Is the employee still a current employee?** In order to be a qualifying retirment employee, you must be currently employed by Pewlett-Hackard.
  - **Who is eligible for the mentorship program?** Individuals who were born between January 1, 1965 and December 31, 1965.
  
Once all this information was obtained, the tables were executed. The table for employees eligible to retire has the following information:
  1. Employee number
  2. First and last name
  3. Title
  4. from_date
  5. Salary
The table for the mentorship program has the following information:
  1. Employee number
  2. First and last name
  3. Title
  4. from_date and to_date
  
## Challenges

As explained earlier, in order to begin the project, several factors had to be established first. In order to find the eligible employees for retirement, the following codes were used:
  -- Create new table for retiring employees
SELECT emp_no, first_name, last_name
INTO retirement_info
FROM employees
WHERE (birth_date BETWEEN '1952-01-01' AND '1955-12-31')
AND (hire_date BETWEEN '1985-01-01' AND '1988-12-31');
-- Check the table
SELECT * FROM retirement_info;

CREATE TABLE dept_emp (
	emp_no INT NOT NULL,
	dept_no VARCHAR (4) NOT NULL,
	from_date DATE NOT NULL,
	to_date DATE NOT NULL,
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no),
	FOREIGN KEY (dept_no) REFERENCES departments (dept_no),
	PRIMARY KEY (emp_no, dept_no)
);

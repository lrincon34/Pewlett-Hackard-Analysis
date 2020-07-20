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
	INTO retirement_inf
	FROM employees
	WHERE (birth_date BETWEEN '1952-01-01' AND '1955-12-31')
	AND (hire_date BETWEEN '1985-01-01' AND '1988-12-31');
	
And the following code was used to determine current employment status:
	SELECT d.dept_name,
     	dm.emp_no,
     	dm.from_date,
     	dm.to_date
	FROM departments as d
	INNER JOIN dept_manager as dm
	ON d.dept_no = dm.dept_no;
	SELECT ri.emp_no,
	ri.first_name,
	ri.last_name,
	de.to_date 
	FROM retirement_info as ri
	LEFT JOIN dept_emp as de
	ON ri.emp_no = de.emp_no;
	SELECT ri.emp_no,
	ri.first_name,
	ri.last_name,
	de.to_date
	INTO current_emp
	FROM retirement_info as ri
	LEFT JOIN dept_emp as de
	ON ri.emp_no = de.emp_no
	WHERE de.to_date = ('9999-01-01');

**One challenge I had was to group the table by job title.** I was not able to find the correct code for it, therefore it was not grouped by job title but the table did have all the other information. The error I kept getting for it was the following:
	ERROR: column must appear in the GROUP BY clause or be used in an aggregate function LINE 2:

Once the table was executed, we then had to remove duplicates. This was also a challenge but eventually the following code worked:
	-- Partition the data to show only most recent title per employee
	SELECT emp_no, 
	first_name, 
	last_name, 
	title, 
	from_date 
	INTO recent_employees
	FROM
	(SELECT emp_no, 
	first_name, 
	last_name, 
	title, 
	from_date, ROW_NUMBER() OVER
	(PARTITION BY (emp_no) 
	ORDER BY from_date DESC) rn
	FROM retiring_employees
	) tmp WHERE rn = 1
	ORDER BY emp_no;

As far as creating the table for **Mentorship Eligibility**, that was not as challenging as the first table. Using the following code, I was able to execute the results.
	--Deliverable 2: Mentorship Eligibility
	SELECT e.emp_no,
	e.first_name,
	e.last_name,
	ti.title,
	ti.from_date,
	ti.to_date
	INTO mentorship_eligibility
	FROM employees as e
	INNER JOIN titles as ti
	ON (e.emp_no = ti.emp_no)
	WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31');
	SELECT DISTINCT ON (emp_no) * 
	INTO mentorship_elig
	FROM mentorship_eligibility;
	


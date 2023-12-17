# sql-challenge

## Data Modeling (10 points)
### Entity Relationship Diagram is included or table schemas provided for all tables (10 points)
  - Provided the ERD image as well as schema for the ERD in the ERD folder
  - The PNG provides an image of the ERD and how they relate. The PDF is the code I used to create the PKs, FKs, and how they were related. 
  - This was all related to items we performed in class so no outside resources were required here.

## Data Engineering (70 points)
### All required columns are defined for each table (10 points)
  - Created a column for each column that was present in the corresponding CSVs
  - Can see the schema by looking in the SQL_Queries_Schema folder and selecting the sqlschema.sql file
### Columns are set to the correct data type (10 points)
  - Columns were set to either be integer or varchar depending on what they were used for
  - I was debating using a Date data types for the date columns, but we didn't really go over that in class so I just used the Varchar
### Primary Keys set for each table (10 points)
  - Primary keys are the columns that uniquely identify the each row in the table 
  - Primary keys have been identified for each table by using the Primary Key (column_name) format
  - For the tables that didn't have a unique 
### Correctly references related tables (10 points)
  - Foreign keys are columns that refer to the primary key in another table
  - I'm not entirely sure how this is different from using the Foreign Keys to reference related tables
  - Foreign Keys include the specific table we are referencing
### Tables are correctly related using Foreign Keys (10 points)
  - Foreign keys have been identified for each table using Foreign Key (column_name) References reference_table_name(reference_column_name)
### Correctly uses NOT NULL condition on necessary columns (10 points)
  - Added not null to all the columns as it seemed important there were no missing data in any rows of these tables
### Accurately defines value length for columns (10 points)
  - For the length I looked at the maximum character length in each column
  - I tried to leave a few extra characters in some columns such as first_name to account for newer entries in the future
  - I kept the Sex column to a single character because there were no entries longer than a single character

## Data Analysis (20 points)
  - Code for this can be found in the SQL_Queries_Schema folder in the SQLQUERIES.sql file
### List the employee number, last name, first name, sex, and salary of each employee (2 points)
  - To calculate these values we had to join the employees and salaries tables
  - Tables could be joined by using the emp_no which was present in each table
  - Pulled the desired columns from each table by joining on emp_no

        select
	        e.emp_no,
	        e.last_name,
        	e.first_name,
        	e.sex,
        	s.salary
        from employees as e
        inner join salaries as s ON
        e.emp_no = s.emp_no;
    
  - **Code for this was taken from activities we performed in class**
    
### List the first name, last name, and hire date for the employees who were hired in 1986 (2 points)
  - To calculate these values we had to reference the employees table
  - The tricky part here was just looking at the year number within the date format

        select
        	first_name,
        	last_name,
        	hire_date
        from employees
        where EXTRACT(YEAR FROM TO_DATE(hire_date, 'MM/DD/YY')) = 1986;

  - **The code within the where statement was provided by chatgpt as I was wondering if there was a way to identify it as a date since it was formatted that way**
    
### List the manager of each department along with their department number, department name, employee number, last name, and first name (2 points)
  - To pull these results we had to join multiple tables (departments and employees) to the dept_manager table
  - dept_manager and departments could be joined by using the dept_no column
  - dept_manager and employees could be joined by using the emp_no column
  - Join allowed me to pull the columns from each table in the desired format

        select 
        	dm.dept_no,
        	d.dept_name,
        	dm.emp_no,
        	e.last_name,
        	e.first_name
        from dept_manager as dm
        inner join departments as d ON
        dm.dept_no = d.dept_no
        inner join employees as e ON
        dm.emp_no = e.emp_no;

  - **Code for this question was taken from activities performed in class**
    
### List the department number for each employee along with that employee’s employee number, last name, first name, and department name (2 points)
  - To pull these results we had to join multiple tables (employees and departments) to the dept_emp table
  - dept_emp and employees could be joined by using the emp_no column
  - dept_emp and departments could be joined by using the dept_no column
  - There are some employees showing up twice in here if they work in multiple departments

        select
        	de.dept_no,
        	de.emp_no,
        	e.last_name,
        	e.first_name,
        	d.dept_name
        from dept_emp as de
        inner join employees as e ON
        de.emp_no = e.emp_no
        inner join departments as d ON
        de.dept_no = d.dept_no;

  - **Code for this question was taken from activities performed in class**
    
### List first name, last name, and sex of each employee whose first name is Hercules and whose last name begins with the letter B (2 points)
  - To pull this information we had to use the employees table
  - Used the where clause to specify how we wanted to pull the results. Hercules as the first_name. Used like 'B%' to pull all last names that started with B

        select
        	first_name, 
        	last_name,
        	sex
        from employees
        where 
        	first_name = 'Hercules' 
        	AND last_name LIKE 'B%';

  - **Code for this was taken from activities we performed in class as well as slide show 9.1**
    
### List each employee in the Sales department, including their employee number, last name, and first name (2 points)
  - To pull this information we had to join a few tables (employees and departments) to the dept_emp table
  - dept_emp and employee could be joined using the emp_no column
  - dept_emp and departments could be joined by using the dept_no column
  - Also had to add a where clause to specify we only wanted to pull results where the dept_name = 'Sales'

        select
        	de.emp_no,
        	e.last_name,
        	e.first_name,
        	d.dept_name
        from dept_emp as de
        inner join employees as e ON
        de.emp_no = e.emp_no
        inner join departments as d ON
        de.dept_no = d.dept_no
        where dept_name = 'Sales'

  - **Code for this was taken from activites we performed in class joining tables using a where clause**
  - 
### List each employee in the Sales and Development departments, including their employee number, last name, first name, and department name (4 points)
  - To pull this information we did the same thing as the previous question but added an addition to the where clause.
  - We used or to state we wanted to pull employees that either worked in sales or development

        select
        	de.emp_no,
        	e.last_name,
        	e.first_name,
        	d.dept_name
        from dept_emp as de
        inner join employees as e ON
        de.emp_no = e.emp_no
        inner join departments as d ON
        de.dept_no = d.dept_no
        where 
        	dept_name = 'Sales'
        	OR dept_name = 'Development';

  - **Code for this was also taken from activities we performed in class**
    
### List the frequency counts, in descending order, of all the employee last names (that is, how many employees share each last name) (4 points)
  - To pull these results we pulled last names and counted each last name from the employee column
  - Renamed count(last_name) as "Frequency Count"
  - Grouped by last_name and then order by the "Frequency Count" descending in order to get the results in the desired format.

        select
        	last_name,
        	count(last_name) as "Frequency Count"
        from employees
        group by last_name
        order by "Frequency Count" desc;

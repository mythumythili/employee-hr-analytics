create table departments(
     department_id int primary key,
	 department_name varchar(100) not null,
	 location varchar(100)
)
create table employees(
    employee_id int primary key,
	first_name varchar(50) not null,
	last_name varchar(50) not null,
	gender varchar(20),
	hire_date date not null,
	job_title varchar(100),
	department_id int references departments(department_id),
	salary numeric(10,2),
	employment_status varchar(20) default 'Active',
	exit_date date
)
select * from employees
select * from departments
INSERT INTO departments
    (department_id, department_name, location)
VALUES
    (1, 'Human Resources', 'Chennai'),
    (2, 'Information Technology', 'Bengaluru'),
    (3, 'Finance', 'Chennai'),
    (4, 'Sales', 'Hyderabad'),
    (5, 'Marketing', 'Bengaluru');

select * from departments

INSERT INTO employees
(employee_id, first_name, last_name, gender,
 hire_date, job_title, department_id, salary,
 employment_status, exit_date)
VALUES
(101, 'Ananya', 'Rao', 'Female', '2021-03-15', 'HR Executive', 1, 42000, 'Active', NULL),
(102, 'Rahul', 'Sharma', 'Male', '2019-07-10', 'HR Manager', 1, 68000, 'Active', NULL),
(103, 'Priya', 'Nair', 'Female', '2022-01-20', 'HR Analyst', 1, 50000, 'Active', NULL),
(104, 'Karthik', 'Iyer', 'Male', '2020-06-01', 'Software Engineer', 2, 75000, 'Active', NULL),
(105, 'Divya', 'Menon', 'Female', '2018-11-12', 'Senior Developer', 2, 98000, 'Active', NULL),
(106, 'Arjun', 'Reddy', 'Male', '2023-02-06', 'Data Analyst', 2, 62000, 'Active', NULL),
(107, 'Meera', 'Krishnan', 'Female', '2021-09-18', 'QA Engineer', 2, 58000, 'Active', NULL),
(108, 'Vikram', 'Patel', 'Male', '2017-04-24', 'IT Manager', 2, 110000, 'Active', NULL),
(109, 'Sneha', 'Das', 'Female', '2022-08-08', 'Accountant', 3, 48000, 'Active', NULL),
(110, 'Rohan', 'Gupta', 'Male', '2020-02-17', 'Finance Analyst', 3, 65000, 'Active', NULL),
(111, 'Lakshmi', 'Pillai', 'Female', '2019-05-05', 'Finance Manager', 3, 90000, 'Active', NULL),
(112, 'Aditya', 'Verma', 'Male', '2024-01-15', 'Junior Accountant', 3, 38000, 'Active', NULL),
(113, 'Ishita', 'Sen', 'Female', '2021-12-01', 'Sales Executive', 4, 45000, 'Active', NULL),
(114, 'Manoj', 'Kumar', 'Male', '2018-03-22', 'Sales Manager', 4, 82000, 'Active', NULL),
(115, 'Pooja', 'Shah', 'Female', '2023-06-19', 'Sales Executive', 4, 44000, 'Active', NULL),
(116, 'Sanjay', 'Rao', 'Male', '2020-10-14', 'Sales Analyst', 4, 60000, 'Active', NULL),
(117, 'Nisha', 'Kapoor', 'Female', '2022-04-11', 'Marketing Executive', 5, 46000, 'Active', NULL),
(118, 'Amit', 'Joshi', 'Male', '2019-08-26', 'Marketing Manager', 5, 85000, 'Active', NULL),
(119, 'Farah', 'Ali', 'Female', '2021-02-09', 'Content Specialist', 5, 52000, 'Active', NULL),
(120, 'Deepak', 'Mishra', 'Male', '2020-01-30', 'Marketing Analyst', 5, 61000, 'Active', NULL);


select * from employees

select count(*) as total_dapartments
from departments;
select count(*) as total_employees
from employees;
select *
from employees
order by employee_id;

--Questions
--1. Display all employees
select * from employees;

--2.Display selected columns
select employee_id, first_name, last_name, job_title, salary
from employees;

--3.  Find employees earning more than 60,000
select first_name, last_name, job_title, salary
from employees
where salary > 60000
order by salary desc;

--4.Find the highest and lowest salaries
select max(salary) as highest_salary,
min(salary) as lowest_salary
from employees;

--5. Count employees by department
select department_id,
count(*) as total_employees
from employees
group by department_id;

--6. Join employees with departments
select 
e.employee_id,
e.salary,
e.first_name,
e.last_name,
d.department_name,
d.location
from employees as e
inner join departments as d
on e.department_id=d.department_id
order by d.department_name,
e.salary desc;

--Having 7. Find departments with average salary above 60,000
select department_id,
round(avg(salary), 2) as average_salary
from employees
group by department_id
having avg(salary)>60000
order by average_salary desc;

--Case when 8. Categorize employee salary
select 
employee_id,
first_name,
salary,
case 
 when salary>=80000 then 'highest salary'
 when salary<=50000 then 'Medium Salary'
 else 'Entry level salary'
end  as salary_categories
from employees
order by salary desc;

--Sub Query 9. Find employees earning above the company's average
select
 employee_id,
 first_name,
 last_name,
 salary
 from employees
 where salary > (
	select avg(salary)
	from employees
 )
order by salary desc;

--Advanced SQL for mployees and HR Analytics
--1. NULL handling
select * from employees;
select 
	employee_id,
	first_name,
	employment_status,
	exit_date
from employees
 where exit_date is null;

--2.To count employees with and without exit dates

select
 	count(*) as total_employees,
	count(exit_date) as employees_with_exitdate,
	count(*)-count(exit_date) as employees_without_exitdates
from employees;

--3. Date function -- calculate employee tenure

select 
	employee_id,
	first_name,
	hire_date,
	AGE(current_date, hire_date) as tenure
from employees
order by tenure desc;

--4. To calculate completed years of service

select
	employee_id,
	first_name,
	hire_date,
	EXTRACT(YEAR FROM AGE(current_date, hire_date)) as Completed_years
from employees
order by completed_years desc;

--5. CTEs- COMMON TABLE EXPRESSIONS
--Find an employees whose salary is higher than the company's average salary

With company_average as (
	select round(avg(salary),2) as avg_salary
	from employees
)
select *
from company_average;


--Find higher paid employees

with company_average as (
	select round(avg(salary), 2) as avg_salary
	from employees
)
select
	e.first_name,
	e.salary
from employees as e
cross join company_average as c
where e.salary > c.avg_salary
order by e.salary desc;

--2. Use a CTE to calculate average salary by department
with department_salary as(
	select 
		department_id,
		round(avg(salary)) as avg_salary
	from employees
	group by department_id
)
select *
from department_salary
order by department_id;


--1. Replace null values using COALESCE()
select
	employee_id,

	first_name,
	employment_status,
	coalesce(
		cast(exit_date as varchar),'Still Employee'
	) as exit_date_status 
from employees
order by employee_id;

--Window Function
--1. How can we number employees from the highest salary to the lowest salary withi each department?
select
	employee_id,
	first_name,
	department_id,
	salary,
	row_number() over(
		partition by department_id
		order by salary desc
	) as employee_number
from employees
order by department_id, employee_number;

--3. Rank() employees based on their salary. The highest salary gets rank 1.
select
	first_name,
	department_id,
	salary,
	rank() over(
		partition by department_id
		order by salary desc
	) as salary_rank
from employees
order by department_id, salary_rank;

--2. Assign a rank() based on value, and it does not skip ranks whn employees have the sa,e salary.
select 
	employee_id,
	first_name,
	department_id,
	salary,
	dense_rank() over(
		partition by department_id
		order by salary desc
	) as salary_rank
from employees
order by department_id, salary_rank;


--LAG()
--1. Lets you access a value from the previou row without joining the table to itself.
select 
	employee_id,
	first_name,
	department_id,
	salary,
	lag(salary) over (
		partition by department_id
		order by salary desc
	) as previous_salary
from employees
order by department_id, salary desc;

--LEAD()
--Access a value from the next row without joining the table to itself.
select
	employee_id,
	first_name,
	department_id,
	salary,
	lead(salary) over (
		partition by department_id
		order by salary desc
	) as next_salary
from employees
order by department_id, salary desc;

--PARTITION BY
select 
	first_name,
	department_id,
	salary,
	round(
		avg(salary) over (
			partition by department_id
		), 2
	) as department_average
from employees
order by department_id, salary desc;

--Find the highest paid employee in each project
select
	employee_id,
	first_name,
	department_id,
	salary
from (
	select
		employee_id,
		first_name,
		department_id,
		salary,
		dense_rank() over(
			partition by department_id
			order by salary desc
		) as salary_rank
	from employees
) as ranked_employees
where salary_rank = 1
order by department_id;

-- Department wise salary and employment statistics
select
	d.department_name,
	count(e.employee_id) as total_employees,
	round(avg(e.salary), 2) as average_salary,
	max(e.salary) as highest_salary,
	min(e.salary) as lowest_salary,
	sum(e.salary) as total_salary_expenses
from departments as d
join employees as e
	on d.department_id = e.department_id
group by d.department_id,
d.department_name
order by total_salary_expenses desc;

--Data Quality Checks
--1. Find Duplicate employee IDs
select 
	employee_id,
	count(*) as id_count
from employees
Group by employee_id
having count(*) > 1;

--2. Find missing employee information
select *
from employees
where 
	first_name is null
	or hire_date is null
	or salary is null;

--3. Find invalid salaries
select * 
from employees
where salary < 0;

--4.Find employees linked to missing department
select
	e.employee_id,
	e.first_name,
	e.department_id
from employees as e
left join departments as d
on e.department_id = d.department_id
where d.department_id is null;

--5. Check employment status
select *
from employees
where employment_status not in ('Active', 'Incentive');

--Check the Database
select
	(select count(*) from departments) as department_count,
	(select count(*) from employees) as employee_count;

--Create a department wise HR summary
select 
	d.department_name,
	count(e.employee_id) as total_employees,
	round(avg(e.salary), 2) as average_salary,
	max(e.salary) as highest_salary,
	min(e.salary) as lowest_salary
from departments as d
left join employees as e
	on d.department_id = e.department_id
	group by d.department_id,
 	d.department_name
	order by average_salary desc;
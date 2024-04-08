# HR-Employee-Distribution-Report

## Project Overview

The goal of this project, HR employee distribution, is to provide HR managers and business leaders with a comprehensive approach to monitor and evaluate employee data in order to make data-driven choices regarding hiring.

## Data Used

 HR Data with over 22000 rows from the year 2000 to 2020.

 ## Tools
 
 - Excel (Data Cleaning)
   
 - Sql - Mysql Workbench (Data Analysis)
   
 - Power BI (Creating Reports)

## **DATA CLEANING**

Open Dataset in Excel and Make a Copy of Dataset for security purpose.

Remove Duplicates.

Change the formatting of necessary columns.

Spell Check.

Change Case - Lower/Upper/Proper.

Trim the unwanted spaces.

Remove null values if its not going to affect the result.

Find & Replace

**DATA ANALYSIS**

Include some interesting Codes/features worked with

``` sql
select * from Table 1
where condition = 3;
```

## Questions

1. What is the gender breakdown of employees in the company?
``` sql
select gender, count(*) as count from Table 1
group by gender;
```

  
2. What is the race/ethnicity breakdown of employees in the company?
```sql
select race, count(*) as count from table 1
where age >=18 and termdate='0000-00-00'
group by race
order by count(*)desc;
```
 
3. What is the age distribution of employees in the company?
```sql
select 
max(age) as oldest,
min(age) as youngest from Table 1
where age>=18 and termdate = '0000-00-00';

select
	case 
		when age>=18 and age<=24 then'18-24'
		when age>=25 and age<=34 then'25-34'
		when age>=35 and age<=44 then'35-44'
        when age>=45 and age<=54 then'45-54'
        when age>=55 and age<=64 then'55-64'
        else '65+'
	end as age_group,
    count(*) as count
from Table 1
where age>=18 and termdate = '0000-00-00'
group by age_group
order by age_group;
```


4. How many employees work at headquarters versus remote locations?
```sql
select location,count(*) as count from Table 1 
where age >= 18 and termdate =  '0000-00-00'
group by location;
```



5. What is the average length of employment for employees who have been terminated?
```sql
select round(avg(datediff(termdate,hire_date))/365,0) as avg_length_employment from Table 1
 where termdate<= curdate() and termdate<>'0000-00-00' and age >=18;
```
 
6. How does the gender distribution vary across departments and job titles?
```sql
select department,gender,count(*) as count from Table 1
where age>=18 and termdate='0000-00-00'
group by department,gender
order by department;
```
 
 
7. What is the distribution of job titles across the company?
```sql
select jobtitle,count(*) as count from Table 1
where age>=18 and termdate = '0000-00-00'
group by jobtitle
order by jobtitle desc;
```

 
8. Which department has the highest turnover rate?
```sql
select department,total_count,terminated_count,terminated_count/total_count as termination_rate
from (
	 select department, count(*) as total_count,
     sum(case when termdate<> '0000-00-00' and termdate <= curdate() then 1 else 0 end) as terminated_count
	 from Table 1
     where age>=18 group by department) as subquery
     order by termination_rate desc;
```

9. What is the distribution of employees across locations by state?
```sql
select location_state,count(*) as count from Table 1
where age>=18 and termdate = '0000-00-00'
group by location_state order by count(*) desc;
```
   
10. How has the company's employee count changed over time based on hire and term dates?
```sql
select year,hires,terminations,hires-terminations as net_change,
round((hires-terminations)/hires*100,2) as net_change_percent 
from(
	select 
	year(hire_date)as year, count(*) as hires, 
	sum(case when termdate<>'0000-00-00' and termdate <= curdate() then 1 else 0 end)as terminations
	from Table 1
	where age>=18
	group by year(hire_date))as subquery
order by year asc;
```
   
11. What is the tenure distribution for each department?
```sql
select department,round(avg(datediff(termdate,hire_date)/365),0) as avg_tenure from Table 1
where termdate<=curdate() and termdate<>'0000-00-00' and age>=18
group by department;
```


## Summary of Findings
 - There are more male employees
 - White race is the most dominant while Native Hawaiian and American Indian are the least dominant.
 - The youngest employee is 20 years old and the oldest is 57 years old
 - 5 age groups were created (18-24, 25-34, 35-44, 45-54, 55-64). A large number of employees were between 25-34 followed by 35-44 while the smallest group was 55-64.
 - A large number of employees work at the headquarters versus remotely.
 - The average length of employment for terminated employees is around 7 years.
 - The gender distribution across departments is fairly balanced but there are generally more male than female employees.
 - The Marketing department has the highest turnover rate followed by Training. The least turn over rate are in the Research and development, Support and Legal departments.
 - A large number of employees come from the state of Ohio.
 - The net change in employees has increased over the years.
- The average tenure for each department is about 8 years with Legal and Auditing having the highest and Services, Sales and Marketing having the lowest.

## Limitations

- Some records had negative ages and these were excluded during querying(967 records). Ages used were 18 years and above.
- Some termdates were far into the future and were not included in the analysis(1599 records). The only term dates used were those less than or equal to the current date.


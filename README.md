# HR-Employee-Distribution-Report

## Introduction

This project provides valuable insights into the distribution of employees in an organization, which can be crucial for HR management and strategic planning.

## Project overview

Here in this project, I have been given a raw data of Human Resources that contains all the information about the employee like first name, last name, birthdate, race, gender, department, job title, location, hiring date, termination date, employee city and employee state. My job in this project is to clean the raw data , then find the useful insights from the data and then display all those useful insights in the form of Visualizations.

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


## **DATA ANALYSIS**

Include some interesting Codes/features worked with

``` sql
select * from Table 1
where condition = 3;
```

## Questions and Answers

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

 - There is a higher distribution of male employees.
   
 -The races with the greatest number is white, while American Indian and Native Hawaiian are the least prevalent.
   
 - The youngest employee is 21 years old and the oldest is 58 years old
   
 - 5 age groups were created (18-24, 25-34, 35-44, 45-54, 55-64). A large number of employees were between 35-44 followed by 25-34 while the smallest group was 55-64.
   
 - Many employees are based in the headquarters as opposed to working remotely.
   
 - The average length of employment for terminated employees is around 8 years.
   
 - In general there are more male employees than female employees, however the ratio of genders among departments is quite balanced.
   
 - The Marketing department has the highest turnover rate followed by Training. The least turn over rate are in the Research and development, Support and Legal departments.
   
 - A large number of employees come from the state of Ohio.
   
 - The net change in employees has increased over the years.

 - Research assistant II has the most number of distributions based on the job title.
   
- The year 2009 had the greatest number of hires (1094) , and the year 2000 had the lowest number of hires (211).

- The majority of terminations occurred in 2001 and 2004. 

## Limitations

- During the searching process, records with negative ages were eliminated, totaling 967 records. The age range used was 18 years old and up.
- A few termdates (1599 records) were excluded from the analysis because they were too far in the future. Dates that were less than or equal to the present date were the only terms that were used.


![HR Dashboard1](https://github.com/nansa93/HR-Employee-Distribution-Report/assets/166003206/822f909c-e0d8-4c63-be12-3ecb47540cd2)


![HR Dashboard 2](https://github.com/nansa93/HR-Employee-Distribution-Report/assets/166003206/c7826fd2-68d8-428e-beaa-6a04aad2d509)


![HR Dashboard 3](https://github.com/nansa93/HR-Employee-Distribution-Report/assets/166003206/25e2bc9b-4545-4069-b741-fa4e2493cd9f)













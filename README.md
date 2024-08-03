# Introduction
📊 Dive into the data job market! Focusing on data analyst roles, this project explores 💰 top-paying jobs, 🔥 in-demand skills, and 📈 where high demand meets high salary in data analytics.

🔍 SQL queries? Check them out here: [project_sql folder](/project_sql/)

# Background
Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining others work to find optimal jobs.

Data hails from my [SQL Course](https://lukebarousse.com/sql). It's packed with insights on job titles, salaries, locations, and essential skills.

### The questions I wanted to answer through my SQL queries were:

1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?
# The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here’s how I approached each question:

### 1. Top Paying Data Analyst Jobs
/* Q1: What are the top_paying jobs for my role?
-- identify the top 10 highest-paying data science roles that are available remotely
-- focus on job postings with specified salaries (remove nulls)
-- why? highlight the top-paying opportunties for data science, offering insights into employment options and location flexibility.company_dim
-- what is the most skills */ 
```sql
SELECT job_id,
	job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name as company_name
FROM job_postings_fact
left join company_dim on job_postings_fact.company_id = company_dim.company_id
where job_title_short = 'Data Scientist'
	and job_location = 'Anywhere'
    and salary_year_avg is not null
order by salary_year_avg DESC
limit 10;
```
Here's the breakdown of the top data Scientist jobs in 2023:
- **High Salary Range::** The average salary for these roles ranges from $240,000 to $340,000, indicating a lucrative market for data science professionals.
- **Leadership Positions:** Many roles listed (e.g., Head of Data Science, Director of Data Science) imply a focus on leadership and strategic decision-making, highlighting the importance of managerial skills in addition to technical expertise.
- **Full-Time Opportunities:** There's a high diversity in job titles, from Data Analyst to -
- ** Director of Analytics:**  reflecting varied roles and specializations within data analytics.
- 
The data scientist job market in 2023 emphasizes high-paying leadership roles with a strong demand for specialized skill sets, particularly in strategic data management and analysis. The remote nature of many positions also indicates a shift towards flexible working conditions in the field.

### 2.  what is main skills of each of those roles are the main drivers behavid the high salary

To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.
```sql
WITH top_paying_jobs AS (
      SELECT job_id,
          job_title,
          salary_year_avg,
          name as company_name
      FROM job_postings_fact
      left join company_dim on job_postings_fact.company_id = company_dim.company_id
      where job_title_short = 'Data Scientist'
          and job_location = 'Anywhere'
          and salary_year_avg is not null
      order by salary_year_avg DESC
)
SELECT top_paying_jobs.*,
	skills_dim.skills
FROM top_paying_jobs
INNER Join skills_job_dim on top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim on skills_job_dim.skill_id = skills_dim.skill_id
ORDER by salary_year_avg DESC;
```
The roles with the highest salaries often require a combination of programming expertise, machine learning proficiency, and experience with data handling tools. Mastery of these skills not only enhances employability but also justifies higher salary expectations in the competitive data science landscape.

### 3. In-Demand Skills for Data Scientist

This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
SELECT 
	skills_dim.skills,
    count(skills_job_dim.job_id) as demand_count
FROM job_postings_fact
INNER Join skills_job_dim on job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim on skills_job_dim.skill_id = skills_dim.skill_id
where job_title_short = 'Data Scientist'
GROUP by skills_dim.skills
order by demand_count DESC
limit 5;
```
Here's the breakdown of the most demanded skills for data Scientist in 2023
- ** Python ** and ** SQL ** lead in demand, followed by SAS, R, and Tableau, highlighting the key technical skills essential for data science roles.

| Skills  | Demand Count|
|---------|-------------|
| Python  | 1357        |
|   SQL   | 957         |
| SAS     | 784         |
| R       | 675         |
| Tableau | 367         |

*Table of the demand for the top 5 skills in data Scientist job postings*

### 4. Skills Based on Salary and Most Optimal Skills to Learn
Exploring the average salaries associated with different skills revealed which skills are the highest paying.
```sql
SELECT skills_dim.skill_id,
	skills_dim.skills,
	count (job_postings_fact.job_id) AS job_demand,
	ROUND(AVG(salary_year_avg),2) as AVG_Salary_per_Skill
FROM job_postings_fact
INNER Join skills_job_dim on job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim on skills_job_dim.skill_id = skills_dim.skill_id
where job_title_short = 'Data scinentist' 
GROUP by skills_dim.skills
HAVING COUNT(job_postings_fact.job_id) > 10
order by AVG_Salary_per_Skill DESC, job_demand DESC
```
Here's a breakdown of the results for top paying skills for Data Analysts:
- **High Demand:** Python and SQL show the highest demand, indicating their importance in the industry.
- **Salary Correlation:** Python and SQL show the highest demand, indicating their importance in the industry.
- **Conclusion:** 
Focusing on skills like Python and SQL, which are in high demand and likely offer competitive salaries, can significantly enhance career prospects for data scientists. These skills are crucial for financial growth in the field.



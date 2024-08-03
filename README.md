# bookish-palm-tree
SQL_PROJECT_DATA_JONS_SCIENCE
/* Q1: What are the top_paying jobs for my role?
-- identify the top 10 highest-paying data science roles that are available remotely
-- focus on job postings with specified salaries (remove nulls)
-- why? highlight the top-paying opportunties for data Scientist, offering insights into employment options and location flexibility.company_dim
-- what is the most skills */ 
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
**Comment**
 we can analyze the job titles and other relevant information for insights into the top data scientist roles.
** Insights from Data Scientist Job Postings
1- High Salary Range:
The average salary for these roles ranges from $240,000 to $340,000, indicating a lucrative market for data science professionals.
2- Leadership Positions:
Many roles listed (e.g., Head of Data Science, Director of Data Science) imply a focus on leadership and strategic decision-making, highlighting the importance of managerial skills in addition to technical expertise.
3- Full-Time Opportunities:
All positions are full-time, suggesting a stable demand for dedicated professionals in data science.
4- Remote Flexibility:
Several roles are remote, reflecting the ongoing trend of flexible work arrangements in the tech industry.
5- Diverse Applications:
Positions span various sectors, including biostatistics and advertising, indicating the versatility of data science across industries.
** Conclusion
The data scientist job market in 2023 emphasizes high-paying leadership roles with a strong demand for specialized skill sets, particularly in strategic data management and analysis. The remote nature of many positions also indicates a shift towards flexible working conditions in the field.

-- Q2: what is main skills of each of those roles are the main drivers behavid the high salary
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
**Comment**
High salaries stem from a mix of technical skills, leadership, and domain expertise.
/* -- Q3: what are the most demand skills for data science?
-- join job postings to inner join table similar to query2 
-- identify the top 5-in demand skills for a data Scientist.
-- focus on all job postings
-- why retrives the top 5 skills with the highest demand in the job market, providing insights into the most valuable skills for job seekers.
*/
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

SELECT 
	skills_dim.skills,
    count(skills_job_dim.job_id) as demand_count
FROM job_postings_fact
INNER Join skills_job_dim on job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim on skills_job_dim.skill_id = skills_dim.skill_id
where job_title_short = 'Data Scientist' AND job_work_from_home = True
GROUP by skills_dim.skills
order by demand_count DESC
limit 5;
**Comment**
Most In-Demand Skills for Data Science
Python: 1357
SQL: 957
SAS: 784
R: 675
Tableau: 367
Summary
Python and SQL lead in demand, followed by SAS, R, and Tableau, highlighting the key technical skills essential for data science roles.
/* Q4: what are the top skills based on salary 
-- look at the average salary associated with each skill for data scinentist postion
-- focuses on roles with specified salaries, regardless of location
-- why it reveals how different skills impact salary levels for data scinentist 
and helps identify the most financially rewarding skills to acquire to improve.
What are the optimal skills to learn? High demand and High salary?*/

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
**Comment**
Top Skills Based on Salary for Data Scientist Positions
Python
Demand Count: 1357
SQL
Demand Count: 957
SAS
Demand Count: 784
R
Demand Count: 675
Tableau
Demand Count: 367

Analysis of Skills Impacting Salary
High Demand: Python and SQL show the highest demand, indicating their importance in the industry.
Salary Correlation: Skills with higher demand often correlate with higher salaries, reflecting their value to employers.
Optimal Skills to Learn
High Demand & High Salary Skills:
Python
SQL
Conclusion
Focusing on skills like Python and SQL, which are in high demand and likely offer competitive salaries, can significantly enhance career prospects for data scientists. These skills are crucial for financial growth in the field.






















































# Introduction
Explore the data analyst job market! This project analyzes, high-paying job opportunities, in-demand skills, and 📈 the relationship between skill demand and salary to uncover valuable insights into the data analytics field in 2023.

* Check SQL queries in here: [project_sql_folder](/project_sql/)

# Background
As a data analyst, I wanted to understand where I stand in today's job market and how my skills align with industry demands. Through this project, I use SQL to explore job opportunities, identify the most in-demand and high-paying skills, and understand how different skills influence salary potential. The goal is to gain insights into the job market, identify areas for professional growth, and make more informed decisions about my career development.

The dataset comes from Luke Barousse's SQL Course and includes information about job titles, salaries, locations, and required skills.

Key Questions I Wanted to Answer
1. What are the top-paying data analyst jobs?
2. Which skills are required for these high-paying positions?
3. What are the most in-demand skills for data analysts?
4. Which skills are associated with higher salaries?
5. Which skills offer the best combination of high demand and competitive salaries?
# Tools I Used
- PostgreSQL
- Visual Studio Code
- Git & Github

# The Analysis
### 1. Top Paying Data Analyst Jobs
```sql
SELECT 
     job_id,
     job_title,
     name AS company_name,
     job_location,
     job_schedule_type,
     salary_year_avg,
     job_posted_date
FROM job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE 
     job_title_short = 'Data Analyst' AND
     job_location = 'Anywhere' AND
     salary_year_avg IS NOT NULL
ORDER BY
      salary_year_avg DESC
LIMIT 10;
```
Here's the breakdown of the top data analyst jobs in 2023
Wide Salary Range:
- The top 10 highest-paying positions offer annual salaries ranging from $184,000 to $650,000, showing the earning potential within data analytics.

- Diverse Employers: Companies such as SmartAsset, Meta, and AT&T offer competitive salaries, indicating strong demand for analytics professionals across different industries.

- Variety of Job Roles: High-paying positions range from Data Analyst to Director of Analytics, reflecting different levels of responsibility and specialization.

![Top Paying Roles](/Access/Top%2010%20Highest-Paying%20Data%20Analyst%20Jobs%20(2023).png)

*Bar chart showing the top 10 highest-paying data analyst jobs in 2023, based on SQL query results. Visualized using Python with ChatGPT assistance.*

### 2. Skills for Top Paying Jobs
To understand which skills are valued in high-paying data analyst positions, I used SQL joins to connect job postings with their required skills.
```sql

WITH top_paying_jobs AS(
    SELECT 
     job_id,
     job_title,
     name AS company_name,
     salary_year_avg
FROM job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE 
     job_title_short = 'Data Analyst' AND
     job_location = 'Anywhere' AND
     salary_year_avg IS NOT NULL
ORDER BY
      salary_year_avg DESC
LIMIT 10
    
)

SELECT 
      top_paying_jobs.*,
      skills 
FROM top_paying_jobs
INNER JOIN  skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
      salary_year_avg DESC
```
![Top Common Skills](/Access/Most%20Common%20Skills%20in%20Top-Paying%20Data%20Analyst%20Jobs%20(2023).png)

*Bar chart showing the most frequently requested skills in high-paying data analyst job postings in 2023, based on SQL query results. Visualized using Python with ChatGPT assistance.*

- **SQL dominates:** Required in all 8 jobs, making it the most frequently mentioned skill.

- **Python is highly valued:** Appears in 7 of 8 postings, highlighting the importance of programming for analytics.

- **Visualization matters:** Tableau appears in 6 postings, showing the importance of communicating insights.

- **Additional technical skills:** R, Pandas, Excel, and Snowflake also appear among the commonly requested skills.


### 3. In-Demand Kills for Data Analysts
```sql
SELECT
      skills,
      COUNT(skills_job.job_id) AS demand_count
FROM job_postings_fact AS job_postings
INNER JOIN skills_job_dim AS skills_job 
       ON job_postings.job_id = skills_job.job_id
INNER JOIN skills_dim ON skills_job.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst' AND job_work_from_home = TRUE
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 10;

```


| Skills | Demand Count |
|--------|-------------:|
| SQL | 7,291 |
| Excel | 4,611 |
| Python | 4,330 |
| Tableau | 3,745 |
| Power BI | 2,609 |
| R | 2,142 |
| SAS | 1,866 |
| Looker | 868 |
| Azure | 821 |
| PowerPoint | 819 |

- SQL dominates demand, appearing in 7,291 job postings, followed by Excel (4,611).

- Python ranks third, highlighting the importance of programming and automation in data analytics.

- Visualization tools remain important, with Tableau, Power BI, and Looker frequently requested.

### 4. Skills Based on Salary
```sql
SELECT 
      skills,
      ROUND(AVG(salary_year_avg),2) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'  AND salary_year_avg IS NOT NULL
GROUP BY skills
ORDER BY avg_salary DESC
LIMIT 25;

```

| Skills | Average Salary (USD) |
|--------|---------------------:|
| SVN | $400,000.00 |
| Solidity | $179,000.00 |
| Couchbase | $160,515.00 |
| Datarobot | $155,485.50 |
| Golang | $155,000.00 |
| MXNet | $149,000.00 |
| Dplyr | $147,633.33 |
| VMware | $147,500.00 |
| Terraform | $146,733.83 |
| Twilio | $138,500.00 |
| GitLab | $134,126.00 |
| Kafka | $129,999.16 |
| Puppet | $129,820.00 |
| Keras | $127,013.33 |
| PyTorch | $125,226.20 |
| Perl | $124,685.75 |
| Ansible | $124,370.00 |
| Hugging Face | $123,950.00 |
| TensorFlow | $120,646.83 |
| Cassandra | $118,406.68 |
| Notion | $118,091.67 |
| Atlassian | $117,965.60 |
| Bitbucket | $116,711.75 |
| Airflow | $116,387.26 |
| Scala | $115,479.53 |

- **SVN has the highest average salary** at $400,000, considerably above the other skills.
- **Specialized technical skills** such as Solidity, Couchbase, and Golang are associated with high-paying positions.
- **Machine learning technologies**, including PyTorch, TensorFlow, and Keras, also appear among the highest-paying skills.

### 5. Most Optimal Skills to Learn
```sql
SELECT
     skills_dim.skill_id,
     skills_dim.skills,
     COUNT(skills_job_dim.job_id) AS demand_count,
     ROUND(AVG(job_postings_fact.salary_year_avg),2) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst' 
     AND salary_year_avg IS NOT NULL
     AND job_work_from_home = TRUE
GROUP BY skills_dim.skill_id
HAVING
      COUNT(skills_job_dim.job_id) > 10
ORDER BY
      avg_salary DESC,
      demand_count DESC
LIMIT 25;
```

| Skill ID | Skills | Demand Count | Average Salary (USD) |
|---------:|--------|-------------:|---------------------:|
| 8 | Go | 27 | $115,319.89 |
| 234 | Confluence | 11 | $114,209.91 |
| 97 | Hadoop | 22 | $113,192.57 |
| 80 | Snowflake | 37 | $112,947.97 |
| 74 | Azure | 34 | $111,225.10 |
| 77 | BigQuery | 13 | $109,653.85 |
| 76 | AWS | 32 | $108,317.30 |
| 4 | Java | 17 | $106,906.44 |
| 194 | SSIS | 12 | $106,683.33 |
| 233 | Jira | 20 | $104,917.90 |
| 79 | Oracle | 37 | $104,533.70 |
| 185 | Looker | 49 | $103,795.30 |
| 2 | NoSQL | 13 | $101,413.73 |
| 1 | Python | 236 | $101,397.22 |
| 5 | R | 148 | $100,498.77 |
| 78 | Redshift | 16 | $99,936.44 |
| 187 | Qlik | 13 | $99,630.81 |
| 182 | Tableau | 230 | $99,287.65 |
| 197 | SSRS | 14 | $99,171.43 |
| 92 | Spark | 13 | $99,076.92 |
| 13 | C++ | 11 | $98,958.23 |
| 186 | SAS | 63 | $98,902.37 |
| 7 | SAS | 63 | $98,902.37 |
| 61 | SQL Server | 35 | $97,785.73 |
| 9 | JavaScript | 20 | $97,587.00 |

- **Highest average salary:** Go leads at $115,319.89, but appears in only 27 job postings.

- **Strong demand and salary:** Python (236 postings, $101,397) and Tableau (230 postings, $99,288) stand out for combining relatively high demand with competitive salaries.

- **Cloud skills offer opportunities:** Snowflake, Azure, and AWS have average salaries above $108,000, although they appear in fewer postings.
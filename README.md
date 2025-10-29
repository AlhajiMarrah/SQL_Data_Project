# Introduction
 📊 Dive into the data job market! focusing on data analyst roles, this project explores 💰 top-paying jobs, in-demand skills, and 📈 where high demand meets high salary in data analytics.

  SQL Queries? Check them out here: [project_sql folder](/project_sql/)

# Background
Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining orthers work to find optimal jobs.

Data hails from my [SQL Course](https://lukebarousse.com/sql), it's packed with insights on job titles, salaries, locations, and essential skills.

### The quensions I wanted to answer through my SQL Queries were:

1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skill are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I used
For my deep dive into the data analyst job marked. I harnessed the power of several key tools:

- **SQL:** The backbone of analysis, allowing me to query the database and unearth critical insights.
- **PostgresSQL:** The chosen database management system, ideal for handling the job posting data.
- **Visual Studio Code:** My go-to for database management and executing SQL queries.
- **Git & Github:** Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking. 
# The Analysis
Each query for this project is aimed at investigating specific aspect of the data analyst job market.
Here is how I approched each quesion:

### 1. Top Paying Data Analyst Jobs
To identify the highest-paying roles I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.

```sql
SELECT
      job_id,
      job_title_short,
      job_location,
      job_via,
      job_schedule_type,
      job_posted_date::DATE,
      salary_year_avg,
      name AS company_name
FROM 
      job_postings_fact
LEFT JOIN company_dim ON
      job_postings_fact.company_id = company_dim.company_id
WHERE
      job_title_short = 'Data Analyst' AND
      job_location = 'Anywhere' AND
      salary_year_avg IS NOT NULL
ORDER BY
      salary_year_avg DESC
LIMIT 10;
```
Here's the breakdown of the top data analyst jobs in 2023:
- **Wide Salary Range:** Top 10 paying data analyst role span from $184,000 to $650,000, indicating significant salary potential in the field.
- **Divers Employes:** Companies like SmartAsset, Meta, AT&T are among those offering high salaries, showing a broad interest across different industries.
- **Job Title Variety:** There's a high diversity in job titles, from data Analyst to director of Analytics, reflecting varied roles and specialization within data Analytics.

### 2. Skills For Top Paying Jobs
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.
```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON
        job_postings_fact.company_id = company_dim.company_id
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
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
     salary_year_avg DESC
```
Here's the breakdown of the most demanded skills for the top 10 highest paying data analyst jobs in 2023.

- **SQL** Is leading with a bold count of 8
- **Python** Follos closely with a bold count of 7
- **Tableau** Is also high sought after, with a bold count of 6. Other skills like **R**, **Snowflake**, **Pandas**, and **Excel** show varying degrees of demand.

### 3. In Demand Skills For Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
SELECT 
      skills,
      COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
     job_title_short = 'Data Analyst' AND
     job_work_from_home = true
GROUP BY
       skills
ORDER BY
      demand_count DESC
LIMIT 5
```
Here's the breakdown of the most demanded skills for data analysts in 2023.
- **SQL** and **Excel** Remain foundemental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
- **Programming** and **Visualization Tools** like **Python, Tableau** and **Power BI** are essential, pointing towards the increasing importances of technical skills in data storytelling and decision support.

 |Skills      |Demand Count|
 |------------|------------|
 |SQL         | 7291       |
 |Excel       | 4611       |
 |Python      | 4330       |
 |Tableau     | 3745       |
 |Power BI    | 2609       |

 *Table of the demand for the top 5 skills in data analyst job postings*

 ### 4. Skills Based On Salary
 Exploring the average salaries associated with different skills revealed which skills are the highest paying.

 ```sql
 SELECT 
      skills,
      ROUND(AVG(salary_year_avg), 2) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
     job_title_short = 'Data Analyst' AND
     salary_year_avg IS NOT NULL AND
     job_work_from_home = true
GROUP BY
       skills
ORDER BY
      avg_salary DESC
LIMIT 25
```
Here's a breakdown of the result for top paying skills for data analysts:

- **High Demand For Big Data & ML Skills:** Top salaries are comanded by analysts skilled in big data technologies (PySpark, CouchBase), machine learning tools (DataRobot, Jupyter), and python libraries (Pandas, NumPy), reflecting the industry's high valuation of data processing and predictive modeling capabilities.
- **Software Development & Deployment Proficiency:** Knowledge in development and deployment tools (GitLab, Kubernentes, Airflow), indicates a lucrative crossover between data analysts and engineering, with a premium on skills that facilitate automation and effecient data pipline management.
- **Cloud Computing Expertise:** Familiarity with cloud and data engineering tools (Elasticsearch, Databricks, GCP), underscores the growing importance of cloud-based analytics environment suggesting that cloud proficiency significantly boosts earning potential in data analystics.

|Skills        |  Average Salary ($)
|--------------|---------------------
| pyspark      |            208,172
| bitbucket    |            189,155
| couchbase    |            160,515
| watson       |            160,515
| datarobot    |            155,486
| gitlab       |            154,500
| swifts       |            153,750
| jupyter      |            152,777
| pandas       |            151,821
| elasticsearch|            145,000

Table of the average salary for the top 10 paying skills for data analysts

### 5. Most Optimal Skills To Learn

Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries offering a strategic focus for skill development.

```sql
WITH skills_demand AS (
    SELECT 
         skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' AND
        salary_year_avg IS NOT NULL  AND
        job_work_from_home = true
    GROUP BY
        skills_dim.skill_id
),  Average_salary AS (
    SELECT
         skills_job_dim.skill_id,
        ROUND(AVG(salary_year_avg), 2) AS avg_salary
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' AND
        salary_year_avg IS NOT NULL AND
        job_work_from_home = true
    GROUP BY
        skills_job_dim.skill_id
)
SELECT 
      skills_demand.skill_id,
      skills_demand.skills,
     demand_count,
     avg_salary
FROM 
    skills_demand
INNER JOIN Average_salary ON skills_demand.skill_id = Average_salary.skill_id
WHERE
    demand_count > 10
ORDER BY
       avg_salary DESC,
       demand_count DESC
LIMIT 25
```

|Skill ID|Skills         |Demand Count|Average Salary ($)
|--------|---------------|------------|-------
|   8    | go            | 27         |115,120
|  224   | confluence    | 11         |114,210
|  97    | hadoop        | 22         |113,193
|  80    | snowflake     | 37         |112,948
|  74    | azure         | 34         |111,225
|  77    | bigquery      | 13         |109,654
|  76    | aws           | 32         |108,317
|  4     | java          | 17         |106,906
| 194    | ssis          | 12         |106,683
| 233    | jira          | 20         |104,918

Table of the most optimal skills for data analyst sorted by salary

Here's the breakdown of the most optimal skills for data analysts in 2023:

- **High-Demand Programming Language:** Python and R stand out for high their demand, with demand counts of 236 and 148 respectively.
Despite their high demand, their average salaries are around $101,397 for python and $100,499 for R, indicating that proficiency in these language is highly valued and also widely available.
- **Cloud Tools & Technologies:** Skills specialize technologies such as snowflake, azure, aws, and bigquery show significant demand with relatively high average salaries, pointing towards the growing importance of cloud platform and big data technologies in data analysis.
- **Business Intelligence and Visualization Tools:** Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around $99,288 and $103,795, highlight the critical roles of data visualization and business intelligence in deriving actionable insights from data.
- **Database Technologies:** The demand for skills in traditional and NoSQL databse (Oracle, SQL Server, NoSQL) with average salaries ranging from $97,786 to $104,134 reflects the enduring need for data storage, retrieval and management expertise.
# What I learned

Throughout this adventure, I've turbocharge my SQL toolkit with some serious fireworks:

- **🧩Complex Query Crafting:** Mastered the art of advance SQL, merging tables like a pro and wilding with clause for ninja-level temp table maneuvers.
- **📊Data Aggregation** Get cozy with GROUP BY and turned aggregate functions like COUNT() and AVG() into my data-sumarizing sidekicks.
- **💡Analytical Wizardry:** Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.

### Conclusions

### Insights
1. **Top-paying Data analyst jobs**: The highest-paying jobs for data analysts that allow remote work offer a wide range of salaries, the highest at $445,000!
2. **Skills For Top-Paying Jobs**: High-paying data analyst job require advance proficiency in SQL, suggesting, it's a critical skill for earning a top salary.
3. **Most In-Demand Skills**: SQL is also the most demanded skill in the data analyst job market, thus making it essential for job seekers.
4. **Skills With Higher Salaries**: Specialized skills, such as svn and solidity, are associated  with the highest average salaries, indicating a premium on niche expertise. 
5. **Optimal Skills For Job Market Valued** SQL leads in demand and offers for a high average salary, positining it as one the most optimal skills for data analysts to learn to maximize their market value.

### Closing Thoughts
This project enhanced my SQL skills and provided valuable insights into the data analysts job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlight the importance of continuous learning and adaptation emerging trends in the field of data analytics. 
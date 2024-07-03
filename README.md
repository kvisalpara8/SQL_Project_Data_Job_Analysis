# Introduction
📊 Dive into the Data job market! Focusing on Data Analyst jobs, this project explores top-paying 💰 jobs, in-demand skills 🔥, and where high-demand meets high-salary in Data Analytics.

🔍 SQL queries? Check them out here: [project_sql folder](/project_sql/).

# Background
Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining others work to find optimal jobs.
### The questions I wanted to answer through my SQL queries were:
1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

# Tools I used for this project
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **SQL**: The backbone of my analysis, allowing me to query the database and unearth critical insights.
- **PostgreSQL**: The chosen database management system, ideal for handling the job posting data.
- **Visual Studio Code**: My go-to for database management and executing SQL queries.
- **Git & GitHub**: Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.
- **AI Tools**: Using AI tools like **ChatGPT** and **Julius** for creating visuals and breaking down the queries into much understandable form and help stakeholders (here job-seekers) make data-driven decisions.

# Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here’s how I approached each question:


### 1. Top Paying Data Analyst Jobs
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on jobs in India. This query highlights the high paying opportunities in the field.

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_location = 'India'
    AND salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```
Here's the breakdown of the top data analyst jobs in 2023:

- **Wide Salary Range**: Top 10 paying data analyst roles span from $64800 to $1192500, indicating significant salary potential in the field.
- **Diverse Employers**: Companies like Deutsche Bank, ACA Group and Clarivate are among those offering high salaries, showing a broad interest across different industries.
- **Job Title Variety**: The range of roles from business intelligence to AI research provides opportunities for candidates with diverse skill sets.

![encoded_assistant_message: eyJmdW5jdGlvbl9jYWxsIjp7ImFyZ3VtZW50cyI6IntcInJfY29kZVwiOlwiIyBDcmVhdGUgYSBiYXIgcGxvdCBmb3IgYXZlcmFnZSBzYWxhcnkgZGlzdHJpYnV0aW9uIGJhc2VkIG9uIGpvYiB0aXRsZVxcbmxpYnJhcnkoZ2dwbG90MilcXG5nZ3Bsb3QoYXZnX3NhbGFyaWVzLCBhZXMoeCA9IHJlb3JkZXIoam9iX3RpdGxlLCAtYXZnX3NhbGFyeSksIHkgPSBhdmdfc2FsYXJ5KSkgK1xcbiAgZ2VvbV9iYXIoc3RhdCA9ICdpZGVudGl0eScsIGZpbGwgPSAnc2t5Ymx1ZScpICtcXG4gIHRoZW1lKGF4aXMudGV4dC54ID0gZWxlbWVudF90ZXh0KGFuZ2xlID0gNDUsIGhqdXN0ID0gMSkpICtcXG4gIGxhYnModGl0bGUgPSAnQXZlcmFnZSBTYWxhcnkgRGlzdHJpYnV0aW9uIGJ5IEpvYiBUaXRsZScsXFxuICAgICAgIHggPSAnSm9iIFRpdGxlJyxcXG4gICAgICAgeSA9ICdBdmVyYWdlIFNhbGFyeSAoVVNEKScpICtcXG4gIHNjYWxlX3lfY29udGludW91cyhsYWJlbHMgPSBzY2FsZXM6OmRvbGxhcl9mb3JtYXQoKSkgK1xcbiAgY29vcmRfZmxpcCgpICAjIEZsaXAgY29vcmRpbmF0ZXMgZm9yIGhvcml6b250YWwgYmFyc1wifSIsIm5hbWUiOiJydW5fcl9jb2RlIn0sInJvbGUiOiJhc3Npc3RhbnQiLCJjb250ZW50IjoiQ2VydGFpbmx5ISBMZXQncyBjcmVhdGUgYSBiYXIgZ3JhcGggdGhhdCBzaG93cyB0aGUgYXZlcmFnZSBzYWxhcnkgZGlzdHJpYnV0aW9uIGJhc2VkIG9uIGpvYiB0aXRsZS5cblxuSSdsbCBnZW5lcmF0ZSB0aGUgYmFyIGdyYXBoIHVzaW5nIHRoZSBkYXRhIHdlIGhhdmUuXG5cbiIsImlkIjoiY2hhdGNtcGwtOWdueGFIeWFCQlNuaVZ2MVl5QnZGNkx6VU5IaGYiLCJtZXNzYWdlX2lkIjoiYzkwZWE3NzEtYzVhZS00ZWM2LWI2YWEtMjkyMmRlYjBmMTVkIn0=](https://api.chatwithyourdata.io/images/ccc8194f-87b0-43e5-8df9-624c5519ab54.png)


### 2. Skills for top paying jobs
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.
```sql
WITH top_paying_jobs AS (
        SELECT
            job_id,
            job_title,
            job_location,
            job_schedule_type,
            salary_year_avg,
            job_posted_date,
            name AS company_name
        FROM
            job_postings_fact
        LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
        WHERE
            job_title_short = 'Data Analyst'
            AND job_location = 'India'
            AND salary_year_avg IS NOT NULL
        ORDER BY
            salary_year_avg DESC
        LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills_dim.skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```

Here's the breakdown of the most demanded skills for the top 10 highest paying data analyst jobs in 2023:

- **SQL** and **Excel** are the most frequently mentioned skills, each appearing 7 times.
- **Python** follows closely with a bold count of 5.
- **R** and **Word** are also highly sought after, each with a bold count of 3. Other skills like Flow, Windows, and Power BI show varying degrees of demand.

![encoded_assistant_message: eyJmdW5jdGlvbl9jYWxsIjp7Im5hbWUiOiJydW5fcl9jb2RlIiwiYXJndW1lbnRzIjoie1xuICBcInJfY29kZVwiOiBcImxpYnJhcnkoZHBseXIpXFxubGlicmFyeSh0aWR5cilcXG5saWJyYXJ5KGdncGxvdDIpXFxuXFxuIyBSZWFkIHRoZSBKU09OIGZpbGVcXG5kZiA8LSBqc29ubGl0ZTo6ZnJvbUpTT04oJ3Bhc3RlLmpzb24nKVxcblxcbiMgU3BsaXQgdGhlIHNraWxscyBjb2x1bW4gYW5kIGNvdW50IG9jY3VycmVuY2VzXFxuc2tpbGxzX2NvdW50IDwtIGRmICU+JVxcbiAgc2VwYXJhdGVfcm93cyhza2lsbHMsIHNlcCA9ICcsICcpICU+JVxcbiAgY291bnQoc2tpbGxzLCBzb3J0ID0gVFJVRSkgJT4lXFxuICB0b3BfbigyMCwgbikgICMgR2V0IHRvcCAyMCBza2lsbHNcXG5cXG4jIENyZWF0ZSB0aGUgYmFyIHBsb3RcXG5nZ3Bsb3Qoc2tpbGxzX2NvdW50LCBhZXMoeCA9IHJlb3JkZXIoc2tpbGxzLCBuKSwgeSA9IG4pKSArXFxuICBnZW9tX2JhcihzdGF0ID0gJ2lkZW50aXR5JywgZmlsbCA9ICdza3libHVlJykgK1xcbiAgY29vcmRfZmxpcCgpICtcXG4gIGxhYnModGl0bGUgPSAnVG9wIDIwIFNraWxscyBieSBGcmVxdWVuY3knLFxcbiAgICAgICB4ID0gJ1NraWxscycsXFxuICAgICAgIHkgPSAnRnJlcXVlbmN5JykgK1xcbiAgdGhlbWVfbWluaW1hbCgpICtcXG4gIHRoZW1lKGF4aXMudGV4dC55ID0gZWxlbWVudF90ZXh0KHNpemUgPSA4KSlcXG5cXG4jIFByaW50IHRoZSBkYXRhXFxucHJpbnQoc2tpbGxzX2NvdW50KVwiXG59In0sInJvbGUiOiJhc3Npc3RhbnQiLCJjb250ZW50IjoiXG5DZXJ0YWlubHkhIEknbGwgY3JlYXRlIGEgYmFyIGdyYXBoIHNob3dpbmcgdGhlIHNraWxscyBhbmQgdGhlIGZyZXF1ZW5jeSBvZiB0aGVpciBhcHBlYXJhbmNlIGluIHRoZSBkYXRhc2V0LiBGaXJzdCwgd2UgbmVlZCB0byBwcm9jZXNzIHRoZSBkYXRhIHRvIGV4dHJhY3QgdGhlIHNraWxscyBhbmQgY291bnQgdGhlaXIgb2NjdXJyZW5jZXMuIFRoZW4sIHdlJ2xsIGNyZWF0ZSBhIGJhciBncmFwaCB0byB2aXN1YWxpemUgdGhpcyBpbmZvcm1hdGlvbi5cblxuIiwiaWQiOiIzNjg4MjFhNS1jNWQ0LTQzZjctYjM3Ni0wYTljM2Y1ODhkZmMiLCJtZXNzYWdlX2lkIjoiNzcwMDQ0NmEtN2IwOC00YzBhLTgyZTEtMDI2ODUzY2M5OWQxIn0=](https://api.chatwithyourdata.io/images/74bc4f23-b78f-430d-ae45-c4f158314ea7.png)


### 3. In-Demand Skills for Data Analyst
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.
```sql
WITH job_skills AS (
        SELECT
            skill_id,
            COUNT(*) AS skill_count
        FROM
            skills_job_dim AS skills_to_job
        INNER JOIN job_postings_fact AS job_postings ON job_postings.job_id = skills_to_job.job_id
        WHERE
            job_postings.job_location = 'India'
            AND job_postings.job_title_short = 'Data Analyst'
        GROUP BY
            skill_id
)

SELECT
    skills AS skill_name,
    skill_count
FROM
    job_skills
INNER JOIN skills_dim AS skills ON skills.skill_id = job_skills.skill_id
ORDER BY
    skill_count DESC
LIMIT 5;
```

Here's the breakdown of the most demanded skills for data analysts in 2023

- **SQL** and **Excel** remain fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
- **Programming** and **Visualization Tools** like **Python**, **Tableau**, and **Power BI** are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.

![encoded_assistant_message: eyJmdW5jdGlvbl9jYWxsIjp7Im5hbWUiOiJydW5fcl9jb2RlIiwiYXJndW1lbnRzIjoiXG57XG4gIFwicl9jb2RlXCI6IFwibGlicmFyeShnZ3Bsb3QyKVxcblxcbiMgQ3JlYXRlIGEgZGF0YSBmcmFtZSBmcm9tIHRoZSBwcm92aWRlZCBkYXRhXFxuZGYgPC0gZGF0YS5mcmFtZShcXG4gIHNraWxscyA9IGMoJ3NxbCcsICdleGNlbCcsICdweXRob24nLCAndGFibGVhdScsICdwb3dlciBiaScpLFxcbiAgZGVtYW5kX2NvdW50ID0gYygxMDE2LCA3MTcsIDY4NywgNTQ1LCA0MDIpXFxuKVxcblxcbiMgQ2FsY3VsYXRlIHBlcmNlbnRhZ2VzXFxuZGYkcGVyY2VudGFnZSA8LSBkZiRkZW1hbmRfY291bnQgLyBzdW0oZGYkZGVtYW5kX2NvdW50KSAqIDEwMFxcblxcbiMgQ3JlYXRlIHRoZSBwaWUgY2hhcnRcXG5waWVfY2hhcnQgPC0gZ2dwbG90KGRmLCBhZXMoeCA9IFxcXCJcXFwiLCB5ID0gZGVtYW5kX2NvdW50LCBmaWxsID0gc2tpbGxzKSkgK1xcbiAgZ2VvbV9iYXIoc3RhdCA9IFxcXCJpZGVudGl0eVxcXCIsIHdpZHRoID0gMSkgK1xcbiAgY29vcmRfcG9sYXIoXFxcInlcXFwiLCBzdGFydCA9IDApICtcXG4gIHRoZW1lX3ZvaWQoKSArXFxuICBnZW9tX3RleHQoYWVzKGxhYmVsID0gcGFzdGUwKHJvdW5kKHBlcmNlbnRhZ2UsIDEpLCBcXFwiJVxcXCIpKSwgcG9zaXRpb24gPSBwb3NpdGlvbl9zdGFjayh2anVzdCA9IDAuNSkpICtcXG4gIGxhYnModGl0bGUgPSBcXFwiRGlzdHJpYnV0aW9uIG9mIFNraWxscyBieSBEZW1hbmQgQ291bnRcXFwiKSArXFxuICB0aGVtZShsZWdlbmQudGl0bGUgPSBlbGVtZW50X2JsYW5rKCkpXFxuXFxuIyBEaXNwbGF5IHRoZSBwaWUgY2hhcnRcXG5wcmludChwaWVfY2hhcnQpXFxuXFxuIyBQcmludCB0aGUgZGF0YVxcbnByaW50KGRmKVwiXG5cbn0ifSwicm9sZSI6ImFzc2lzdGFudCIsImNvbnRlbnQiOiJcbkNlcnRhaW5seSEgSSdsbCBjcmVhdGUgYSBwaWUgY2hhcnQgdG8gdmlzdWFsaXplIHRoZSBkaXN0cmlidXRpb24gb2Ygc2tpbGxzIGJhc2VkIG9uIHRoZWlyIGRlbWFuZCBjb3VudC4gTGV0J3MgdXNlIFIgdG8gZ2VuZXJhdGUgdGhpcyBwaWUgY2hhcnQuXG5cbiIsImlkIjoiZWRkYzA5NDYtNzUxMy00NzIyLWE3MDAtZTRhYzMyMzRkMmNiIiwibWVzc2FnZV9pZCI6ImI3YTcwMjJkLTdlZDYtNDk4Yi04Yjg1LWNkNzZkZTExYjA1NiJ9](https://api.chatwithyourdata.io/images/e74a5c63-3db3-4877-a3c8-b14f00841c9b.png)


### 4. Skill Based on Salary
Exploring the average salaries associated with different skills revealed which skills are the highest paying.
```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_postings_fact.salary_year_avg IS NOT NULL
    AND job_location = 'India'
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 10;
```

Here's a breakdown of the results for top paying skills for Data Analysts:
- Skills like **Visio**, **Confluence**, and **Jira** command high salaries likely due to their specialized use in project management and documentation within large organizations.
- **Azure** and **Power BI** skills indicate strong demand in cloud computing and business intelligence sectors, reflecting ongoing digital transformation efforts.
- Proficiency in **PowerPoint, Flow, Sheets, Word, and SQL** remains valuable across various roles, highlighting their role in enhancing productivity, workflow automation, and data management.


|   skills    | avg_salary |
|-------------|------------|
| visio       | 119250     |
| confluence  | 119250     |
| jira        | 119250     |
| azure       | 118140     |
| power bi    | 118140     |
| powerpoint  | 104550     |
| flow        | 96604      |
| sheets      | 93600      |
| word        | 89579      |
| sql         | 85397      |

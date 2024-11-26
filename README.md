# Filtering, Join, and Aggregation with SQL

## Project Description

This project demonstrates advanced SQL techniques involving filtering, joins, and aggregation. These operations are crucial for extracting actionable insights from relational databases. By leveraging SQL functionalities such as views, subqueries, grouping sets, rollups, and cubes, this project provides solutions to complex business queries, offering a comprehensive understanding of data relationships and trends.

---

## Key Concepts and Examples

### **1. Comparing Individual Salaries with Department Averages**
- **Objective:** Compare an employee's salary with their department's average salary.
- **Examples:**
  - Fetch an employee's salary alongside their department average:
    ```sql
    SELECT
        s.last_name, s.salary, s.department,
        (SELECT ROUND(AVG(salary), 2)
         FROM staff s2
         WHERE s2.department = s.department) AS department_average_salary
    FROM staff s;
    ```
  - Create a view to determine how many employees earn above or below their department's average:
    ```sql
    CREATE VIEW vw_salary_comparision_by_department AS
    SELECT 
        s.department,
        (s.salary > (SELECT ROUND(AVG(s2.salary), 2)
                     FROM staff s2
                     WHERE s2.department = s.department)) AS is_higher_than_dept_avg_salary
    FROM staff s;
    ```

---

### **2. Identifying Top Earners**
- **Objective:** Find employees with the highest salaries within the company or their departments.
- **Examples:**
  - Find the highest salary in the company:
    ```sql
    SELECT last_name, department, salary
    FROM staff
    WHERE salary = (
        SELECT MAX(s2.salary)
        FROM staff s2
    );
    ```
  - Find the highest earners within each department:
    ```sql
    SELECT s.department, s.last_name, s.salary
    FROM staff s
    WHERE s.salary = (
        SELECT MAX(s2.salary)
        FROM staff s2
        WHERE s2.department = s.department
    )
    ORDER BY 1;
    ```

---

### **3. Joining and Merging Data**
- **Objective:** Combine data across multiple tables to gain meaningful insights.
- **Examples:**
  - Get employee details with company divisions:
    ```sql
    SELECT s.last_name, s.department, cd.company_division
    FROM staff s
    LEFT JOIN company_divisions cd
        ON cd.department = s.department;
    ```
  - Identify employees without assigned company divisions:
    ```sql
    SELECT s.last_name, s.department
    FROM staff s
    LEFT JOIN company_divisions cd
        ON cd.department = s.department
    WHERE company_division IS NULL;
    ```

---

### **4. Advanced Aggregation**
- **Objective:** Use grouping sets, rollups, and cubes for multi-level data aggregation.
- **Examples:**
  - Number of employees per region and division using grouping sets:
    ```sql
    SELECT company_regions, company_division, COUNT(*) AS total_employees
    FROM vw_staff_div_reg
    GROUP BY GROUPING SETS(company_regions, company_division)
    ORDER BY 1, 2;
    ```
  - Generate subtotals and grand totals using rollup:
    ```sql
    SELECT country, company_regions, COUNT(*) AS total_employees
    FROM vw_staff_div_reg_country
    GROUP BY ROLLUP(country, company_regions)
    ORDER BY country, company_regions;
    ```

---

### **5. Top-N Analysis**
- **Objective:** Fetch the top records based on a specific criterion.
- **Examples:**
  - Top 10 salary earners:
    ```sql
    SELECT last_name, salary
    FROM staff
    ORDER BY salary DESC
    FETCH FIRST 10 ROWS ONLY;
    ```
  - Top 5 company divisions with the highest number of employees:
    ```sql
    SELECT company_division, COUNT(*) AS total_employees
    FROM vw_staff_div_reg_country
    GROUP BY company_division
    ORDER BY COUNT(*) DESC
    FETCH FIRST 5 ROWS ONLY;
    ```

---

## Tools and Environment
- **Database Management Systems:** PostgreSQL, MySQL
- **Setup Requirements:**
  - Import the relevant tables (`staff`, `company_divisions`, `company_regions`) into your database.
  - Execute the queries sequentially to replicate and analyze the results.

---

## Learning Outcomes
- Master advanced SQL operations like joins, subqueries, and views.
- Understand multi-level aggregation techniques using grouping sets, rollups, and cubes.
- Apply filtering and conditional logic for precise data extraction.
- Perform top-N analysis to identify key performers and contributors.

---

## Use Cases
- **Salary Analysis:** Compare employee salaries with department averages and identify top earners.
- **Data Integration:** Combine and analyze data from multiple tables to uncover relationships.
- **Reporting:** Generate multi-level summaries and subtotals for reporting purposes.

---

## Acknowledgments
This repository provides a comprehensive guide to advanced SQL techniques, enabling learners to tackle real-world database challenges effectively.

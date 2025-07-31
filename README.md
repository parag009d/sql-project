# layoffs data cleaning and exploratory data anaysis using sql


## Overview

This project presents an end-to-end data analytics solution using **MySQL** on a real-world dataset about company layoffs from 2020 to 2023. The main goal is to analyze global layoff patterns, clean raw and inconsistent data, and extract key business insights using structured SQL queries. The project simulates the role of a data analyst in transforming business data into actionable intelligence — without using Python, R, or visualization tools.

## Objectives

- Clean and standardize raw layoff data using SQL only.
- Analyze total layoffs by year, country, company, industry, and startup stage.
- Identify companies with 100% layoffs and understand their funding impact.
- Calculate rolling monthly layoff trends to find peak layoff periods.
- Rank top companies by layoffs for each year using window functions.

  # Dataset

The dataset for this project was sourced from Kaggle 
- **Dataset Link:** [Layoffs 2022 Dataset – Kaggle](https://www.kaggle.com/datasets/swaptr/layoffs-2022)


## Schema

```sql
CREATE TABLE layoffs (
    company TEXT,
    location TEXT,
    industry TEXT,
    total_laid_off INT,
    percentage_laid_off TEXT,
    date TEXT,
    stage TEXT,
    country TEXT,
    funds_raised_millions INT
);
```
##  Problems and Solutions
### 1. Total Layoffs by Year
   ```sql
SELECT YEAR(date) AS year, SUM(total_laid_off) AS total_layoffs
FROM layoffs_staging2
GROUP BY year
ORDER BY year;
 ```
### 2. Top 3 Companies with Most Layoffs Each Year
 ```sql
WITH Company_Year AS (
  SELECT company, YEAR(date) AS year, SUM(total_laid_off) AS total_layoffs
  FROM layoffs_staging2
  GROUP BY company, YEAR(date)
),
Company_Rank AS (
  SELECT *, DENSE_RANK() OVER (PARTITION BY year ORDER BY total_layoffs DESC) AS rank
  FROM Company_Year
)
SELECT * FROM Company_Rank WHERE rank <= 3;
```
### 3. 100% Layoffs (Company Shut Down)
```sql
SELECT company, total_laid_off, percentage_laid_off
FROM layoffs_staging2
WHERE percentage_laid_off = 1;
```
### 4. Layoffs by Country
```sql
SELECT country, SUM(total_laid_off) AS total
FROM layoffs_staging2
GROUP BY country
ORDER BY total DESC;
```
### 5. Rolling Monthly Layoffs
```sql
WITH Date_CTE AS (
  SELECT DATE_FORMAT(date, '%Y-%m') AS month, SUM(total_laid_off) AS total
  FROM layoffs_staging2
  GROUP BY month
)
SELECT month, SUM(total) OVER (ORDER BY month) AS rolling_total
FROM Date_CTE;
```

## Findings and Conclusion

- Delivered an end-to-end SQL data analysis project focused on global layoffs from 2020 to 2023.
- Cleaned and standardized a raw dataset containing over 2,000 layoff records to enable accurate analysis.
- Applied advanced SQL techniques such as CTEs, window functions, and time-based aggregations to extract trends and insights.
- Identified peak layoff periods, high-impact industries, and companies with complete workforce reductions.
- Demonstrated the ability to transform unstructured business data into actionable insights using only SQL.



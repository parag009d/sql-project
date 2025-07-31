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
    SELECT YEAR(date) AS year, SUM(total_laid_off) AS total_layoffs
FROM layoffs_staging2
GROUP BY year
ORDER BY year;

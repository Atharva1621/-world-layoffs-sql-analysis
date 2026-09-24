World Layoffs – SQL Data Cleaning & Exploratory Analysis

This project looks at a dataset of global company layoffs and works through it in two stages: first cleaning up the raw data, then exploring it to pull out some useful patterns. Everything is written in MySQL.

Dataset

The dataset lists layoffs reported by companies around the world, with details like the company, location, industry, country, how many people were laid off, what percentage of the workforce that was, the company's funding stage, and how much funding they had raised. It's the same dataset used in Alex The Analyst's MySQL portfolio project series, and it's publicly available through his GitHub.

Tools used
MySQL / MySQL Workbench
Window functions (ROW_NUMBER, DENSE_RANK)
CTEs (Common Table Expressions)
1: Data Cleaning

Before analyzing anything, the raw data needed some cleanup. The script Data_Clean.sql covers:

Copying the raw table into a staging table first, so the original data is never touched directly
Finding and removing duplicate rows using ROW_NUMBER, partitioned across all the columns that together make a row unique
Trimming extra whitespace from company names
Standardizing inconsistent values, like collapsing different variations of "Crypto" into one consistent industry label, and removing a trailing period from some of the "United States" entries
Converting the date column from text into an actual DATE type, so it can be sorted and filtered properly
Filling in missing industry values by matching them against other rows from the same company where the industry was already known
Removing rows that had no useful data at all, meaning both the total laid off and percentage laid off were missing
2: Exploratory Data Analysis

Once the data was clean, EDA_Layoffs.sql digs into it to answer some basic questions:

Which single layoff event affected the most people, and which company laid off 100% of its workforce
Which companies had the highest total layoffs overall
Which countries and industries were hit hardest
How layoffs were spread out over time, month by month and year by year
A running (rolling) monthly total of layoffs, using a window function over a CTE, to see how the numbers built up over time
The top 5 companies by layoffs for each year, using DENSE_RANK so ties are handled properly
Files
Data_Clean.sql – cleans and standardizes the raw layoffs table
EDA_Layoffs.sql – explores the cleaned data and answers the questions above
How to use

Run Data_Clean.sql first in MySQL Workbench to create and clean the staging table, then run EDA_Layoffs.sql on top of it to reproduce the analysis.

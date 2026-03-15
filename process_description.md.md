# **Healthcare analytics sheets**  For my spreadsheet portfolio project, I worked with a dataset well-suited for analysis in Google Sheets.

Dataset Source: [Kaggle \- Hospital HMIS Dataset for Healthcare Analytics](https://www.kaggle.com/datasets/shalakagangurde/hospital-hmis-dataset-for-healthcare-analytics) (Shalaka Gangurde).

After downloading the zip file from Kaggle, I selected the main CSV files and imported them into Google Sheets as separate tabs (`patients`, `admissions`, `billing`, `diseases`).

### **Step 1: Building a Master Table with VLOOKUP**

To perform a thorough analysis, I created a new tab called `master`. This consolidated all relevant information into a single table, making it easier to analyze.

First, I brought in key identifiers by referencing the original `admission` tab. For example, in column A of my `master` tab, I used `=admission!A2:A` and dragged the fill handle to populate the entire column with all `admission_id` values. I repeated this for essential columns like `admission_date`, `discharge_date`, and `patient_id`.

Once I had these key columns, I used `VLOOKUP` to enrich the table with patient details. For instance, to add the patient's gender, I used the following formula:

`=VLOOKUP($F2, patients!$A:$F, 2, FALSE)`

This formula searches for the `patient_id` (from column F in `master`) within the `patients` tab and returns the corresponding value from the 2nd column (`gender`). I used similar `VLOOKUP` formulas to add `date_of_birth`, `blood_group`, and `city`.

### **Step 2: Preparing Data for Time-Series Analysis**

To analyze revenue trends over time, I needed to break down the `admission_date` into its year and month components. I added two new columns to my `master` tab for this purpose:

* `year`: `=YEAR(B2)`  
* `month`: `=MONTH(B2)`

### **Step 3: Analyzing Monthly Revenue with Pivot Tables and Charts**

To visualize revenue patterns, I created a pivot table in the `monthly_summary` tab, grouping `total_amount` by year and month. From this pivot table, I generated two line charts:

* Monthly Revenue (2020): A detailed view of revenue fluctuations across each month of 2020, revealing seasonal patterns and identifying the peak month (December) and the lowest (July).  
* Annual Revenue Trend (2020-2025): A view showing year-over-year revenue progression, highlighting growth trends and any significant drops or increases across the six-year period.

### **Step 4: Identifying Top Diagnoses and Analyzing Length of Stay** 

Finally, I wanted to identify the most common diagnoses and analyze their impact on hospital resources.  
To identify the most common health conditions in the dataset, I created a pivot table in the `top_diagnoses` tab, counting admissions by `disease_name`. This revealed the top diagnoses, with Stroke (2,300 admissions), Sepsis (2,297), and COPD (2,288) being the most frequent.

I then wanted to analyze Length of Stay (LOS) for these top conditions. After calculating LOS in my `master` tab (`discharge_date - admission_date`), I created a dropdown list of the top 5 diagnoses and used the following formula to calculate the average LOS dynamically:

`=IFERROR(AVERAGEIF(Master!L:L, D4, Master!T:T), "N/A")`

This allowed me to see, for example, that patients admitted for Stroke had an average hospital stay of 5.17 days.


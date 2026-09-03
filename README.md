## Overview
Perform in-depth exploratory data analysis on ~12,000 completed e-commerce browsing sessions and 18 variables/ characteristics.
Clean and pre-process the data, engineering new features, dropping irrelevant columns, and transforming data for modeling.
Use unsupervised and supervised machine learning models to better understand the underlying patterns in the data. See the final report under results/reports for a detailed breakdown of the findings.

This analysis is not for predicting purchase intent mid-session.
The goal is to identify what *drives* purchase intent, by creating a logistic regression model and evaluating its coefficients to identify key purchase drivers.

## Repo Structure
data/ - Raw and processed datasets
notebooks/ - EDA, preprocessing, and modeling notebooks
results/data - Saved model output data for Tableau
results/images - Charts and dashboard
results/reports - Final report



## Data Dictionary
The dataset consists of 10 numerical and 8 categorical attributes.
The 'Revenue' attribute can be used as the class label.

"Administrative", "Administrative Duration", "Informational", "Informational Duration", "Product Related" and "Product Related Duration" represent the number of different types of pages visited by the visitor in that session and total time spent in each of these page categories. The values of these features are derived from the URL information of the pages visited by the user and updated in real time when a user takes an action, e.g. moving from one page to another. The "Bounce Rate", "Exit Rate" and "Page Value" features represent the metrics measured by "Google Analytics" for each page in the e-commerce site. The value of "Bounce Rate" feature for a web page refers to the percentage of visitors who enter the site from that page and then leave ("bounce") without triggering any other requests to the analytics server during that session. The value of "Exit Rate" feature for a specific web page is calculated as for all pageviews to the page, the percentage that were the last in the session. The "Page Value" feature represents the average value for a web page that a user visited before completing an e-commerce transaction. The "Special Day" feature indicates the closeness of the site visiting time to a specific special day (e.g. Mother’s Day, Valentine's Day) in which the sessions are more likely to be finalized with transaction. The value of this attribute is determined by considering the dynamics of e-commerce such as the duration between the order date and delivery date. For example, for Valentina’s day, this value takes a nonzero value between February 2 and February 12, zero before and after this date unless it is close to another special day, and its maximum value of 1 on February 8. The dataset also includes operating system, browser, region, traffic type, visitor type as returning or new visitor, a Boolean value indicating whether the date of the visit is weekend, and month of the year.

## Setup
### Requirements
pandas, numpy, scikit-learn, matplotlib, seaborn, scipy, statsmodels
Run notebooks in order: anna_eda.ipynb -> preprocessing_v1.ipynb -> model_v1.ipynb -> preprocessing_v2.ipynb -> model_v2.ipynb

## Final Dashboard of the Findings
https://public.tableau.com/app/profile/anna.malaschenko/viz/E-commerceShopperData/Dashboard1#1

## Areas for Further Exploration
These dataset columns are highly right-skewed:

Administrative_Duration
Informational_Duration
ProductRelated_Duration

After EDA, There were no obvious signs of time out or bot sessions. These variables were especially interesting however, as each type of page had ~1,000 or more outliers out of ~12,000 rows, making up a significant part of the dataset. The percent of sessions with a favorable purchase outcome inside of those outliers were *higher* (up to 2x) than that of the full dataset, and therefore were not outright dropped. Exploration and modeling can be done on the outlier sessions alone in order to identify any trends and patterns among them. 

After modeling the data, it was clear that page durations for each type of page were some of the *least* influential variables for the model's outcome, and thus digging deeper into these outliers will be left for a different day.

If I had to update a feature and rerun the model, I would bin more traffic types together. In my EDA I decided on a threshold of 200 when picking the traffic types to keep as their own features. During preprocessing I lowered the threshold to 100 (which in reality binned the same rows as 200 would have), to capture more traffic types' affects. When I ran my model and evaluated it's coefficients, many traffic types won out as some of the biggest influences in the model. However, picking to bin at a lesser threshold included a traffic type with 247 sessions, as well as traffic types with 1000+ rows of data. What I would do if I continued from here on is include 5 distinct traffic types with 700+ sessions, and bin the rest under 'Other_Traffic', to see how the coefficients of the kept traffic types, as well as other variables are affected.

## Citation
Sakar, C. & Kastro, Y. (2018). Online Shoppers Purchasing Intention Dataset [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5F88Q.
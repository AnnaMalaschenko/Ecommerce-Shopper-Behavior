## Overview
Perform in-depth exploratory data analysis on 12.3k completed e-commerce browsing sessions and 18 variables/ characteristics.
Clean and pre-process the data- engineering new features, dropping irrelevant columns, and transforming data for modeling.
Use unsupervised and supervised machine learning models to better understand the underlying patterns in the data. See the final report under results/reports for a detailed breakdown of the findings.

This analysis is not for predicting purchase intent mid-session.
The goal is to identify what *drives* purchase intent, by creating a logistic regression model and evaluating it's coefficients to identify key purchase drivers.

## Final Dashboard of the Findings
https://public.tableau.com/app/profile/anna.malaschenko/viz/E-commerceShopperData/Dashboard1#1

## Data Source: 
https://archive.ics.uci.edu/dataset/468/online+shoppers+purchasing+intention+dataset

## Data Dictionary
"Administrative", "Informational", "Product Related" - Type of page visited in the session
"Administrative Duration", "Informational Duration", "Product Related Duration" - How long each page was visited for in the session.
"Bounce Rate" - percentage of visitors who enter the site from that page and then leave ("bounce") without triggering any other requests to the analytics server during that session. Bounce rates seem to be some aggregation of values for the different pages visited during a session - with the type of aggregation not having been specified (ie. average vs sum).
"Exit Rate" - percentage of time the page was the last in the session.
"Page Value" - represents the average value for a web page that a user visited before completing an e-commerce transaction. 
"Special Day" - indicates the closeness of the site visiting time to a specific special day (e.g. Mother’s Day, Valentine's Day).

## Areas for Further Exploration

The dataset columns are highly right-skewed:

Administrative_Duration
Informational_Duration
ProductRelated_Duration

After EDA, There were no obvious signs of time out or bot sessions. These variables were especially interesting however, as each type of page had ~1,000 or more outliers out of ~12,000 rows, making up a significant part of the dataset. The percent of sessions with a favorable purchase outcome inside of those outliers were *higher* (up to 2x) than that of the full dataset, and therefore were not outright dropped. Exploration and modeling can be done on the outlier sessions alone in order to identify any trends and patterns among them. 

After modeling the data, it was clear that page durations for each type of page were some of the *least* influential variables for the model's outcome, and thus digging deeper into these outliers will be left for a different day.
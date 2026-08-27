## Methodologies
Build a logistic regression model to predict if a purchase will happen during a shopping session.
Identify the biggest contributors to sessions ending in a purchase.

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

They each have almost 1,000 or more outliers out of ~12,000 rows, making up a significant part of the dataset.
The percent of sessions with a favorable outcome inside of those outliers are *higher* than in the full dataset, and therefore were not outright dropped.
The task at hand while exploring these variables was to investigate for obvious signs of sessions that were either inactive for a long time - such as when a user may tab out and not be actively browsing, or bot sessions, which are unrepresentative of a typical user and add noise to the data.
After performing the EDA, only a few rows were dropped, and the rest were kept due to no clear sign of them landing in one of the problem buckets I've described.

If I'm to go back and experiment, a few options are:
1) Winsorizing - which would cap the durations to a max, giving me a more reliable coefficient while not eliminating the rows, and still exposing the model to high durations. I'd compare the model's metrics to one without this implementation.
2) Use PCA on the outlier durations to identify common patterns and behaviors which could help identify atypical sessions.
## Methodologies
Clustering to segment observations, or dimensionality reduction, to understand structure.
Logistic Regression and Random Forest for supervised learning.

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
The task at hand while exploring these variables was to investigate for obvious signs of sessions that were inactive for a long time - such as when a user may tab out and not be actively browsing, which, if unaccounted for, may add noise to the model.
The percent of sessions with a favorable outcome inside of those outliers are *higher* than in the full dataset, and therefore were not all outright dropped.
My concern being that, since I want interpretability from this model via logistic regression, my coefficients for these durations may be noisy due to these outliers.

Variations of Duration Handling That Can Further Be Tried and Compared:
Winsorizing would cap the durations to a max, giving me a more reliable coefficient while not eliminating the rows, and still exposing the model to high durations. Try a model both including this method and not to see if this improves it.
Use PCA on sessions with outlier duration to identify common patterns and behaviors that could indicate bot activity or tabbed out users.


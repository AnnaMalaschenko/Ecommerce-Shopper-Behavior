## Findings

### Groups of Shoppers
K-Means clustered together sessions based on 7 continuous variables, creating 4 distinct group profiles:

• Cluster 3 - Low browsing activity, low page values, slightly elevated exit rates, low conversion (4% made a purchase). This group makes up 62% of sessions in the training data - the typical shopper.
• Cluster 1 - Very low browsing activity, very high ExitRates, the lowest page values, the smallest conversion (0.7% purchase rate). This group makes up 12% of sessions in the training data.
• Cluster 0 - Higher browsing activity, significantly higher PageValue, high conversion (61% purchase rate). This group makes up 17% of sessions in the training data.
• Cluster 2 - Very high browsing activity, higher page values, lower exit rates, and moderate conversion rates (28% purchase rate). This group makes up 9% of the training data.

### What the Groups Show
The groups served as a hint into variables that may be influential in purchase decisions - with the highest page values showing up in the group with the highest purchasing rate, exit rate being highest in the group with the lowest purchase rate, and page count as well as duration showing up in higher values amongst the 2 groups with the highest purchase rates.


## Supervised Model Performance
• The model performance in model_v1 and model_v2 was nearly identical when evaluating its classification report.
• An ROC AUC score of 0.92 means that the model is very good at distinguishing between sessions that'll end in a purchase and those that wont, where a 0.50 score would be the same as taking a random guess.
• For sessions where a purchase was made, the precision score is 0.55, meaning only 55% of sessions the model flagged as purchases did end in a purchase.
• The false discovery rate of 45% (1-precision) is less concerning when looking at the recall score of 0.80, meaning that for all sessions that did end with a purchase, the model caught 80% of them. The model has decent capability to catch sessions that will end in a purchase at the cost of more false positives.
• The bigger role of this model was to extract the coefficients that contributed to its output, the most important of which I describe below.


## The Most Impactful Features for the Logistic Regression Model's Output (Top 10):

                         feature  coefficient  abs_coef
0                 PageValues_log     1.270135  1.270135
1                      Month_May    -1.158958  1.158958
2                 high_exit_rate    -0.984515  0.984515
3                      Month_Mar    -0.957106  0.957106
4                  TrafficType_5     0.806154  0.806154
5                  TrafficType_8     0.761314  0.761314
6                      Month_Nov     0.691225  0.691225
7                      Month_Dec    -0.672690  0.672690
8                 TrafficType_20     0.661370  0.661370
9                 TrafficType_13    -0.552232  0.552232

• PageValues is the single largest contributor to purchase predictions- with higher page values substantially increasing the odds of a purchase in a session. Since PageValues was log-transformed and scaled during preprocessing, its odds ratio (3.56x per scale unit) is less interpretable than the rest of the top 10 predictive features (all binary). However, Its direction is clear (higher PageValues -> higher odds of purchase), and its magnitude of effect is supported by findings via the k-means model (Cluster 0 had exceptionally high Page Values, and a purchase rate of 61%, more than 2x that of the next highest conversion group).
• high_exit_rate (>= 0.18) has the 3rd highest coefficient - which lines up with findings in k-means clustering, showing that the group with the highest exit rates were significantly higher, and the purchase rate of that group was 0.7%.
• November increased the probability of a session leading to a purchase, while May, March, and December significantly decreased it, suggesting seasonality played a big role in purchase intent.
• Sessions originating from Traffic Types 5, 8, 20, and 13 had 2.24x, 2.14x, 1.94x, and 0.58x the odds accordingly of a session leading to a purchase when compared to other traffic types.
• Page related variables such as type of page visited, how many, and how long, had some of the smallest coefficients, being outranked by Months and Traffic Types, contributing little when predicting purchases within sessions.


### Implications and Recommendations from Feature Importance
• The Page Value metric having been derived from a page's historical financial contribution (revenue generated), it makes intuitive sense then that shoppers visiting higher page value sessions will also make a purchase, since the page value implies others have done so in the past. An e-commerce website can test how purchase rates change via an experiment where more sessions recommend or redirect the shopper to higher value pages with the goal of increasing conversion.
• Based on the metadata, exit rates are attributed to individual pages, while each row/session in the dataset has one ExitRates value, implying a possible aggregation across all pages the shopper visited- however the dataset description does not indicate this explicitly, and the type of aggregation is unknown. What we do know is that exit rates are similar to page values in the sense that they're based off of historical activity, and however this metric was calculated, there is a real signal that suggests looking at past page performance as a metric to predict future sales.
• Months were bucketed into an 'other' category, except the 4 that were encoded separately since they appeared in many more sessions. All 4 of the months kept won out on top amongst the highest contributors to the model's outcome. It's possible that other months would also carry signal, but more data needs to be collected for them to make reliable conclusions - which is the recommendation being made here. This is supported by the fact that the odds of a session ending in a purchase drastically vary by month (In November the odds are 2x, while in May they are 0.31x when compared to other months), and the model's predictive capability could improve if it is trained off of a dataset with more session data for those months.
• Traffic types 5, 8, and 20 should be investigated for the reason that they are positively correlated with a session ending in a purchase. They are not labeled in the metadata what this traffic is, but it'd be important to know whether they are paid ads, affiliation links, social media ads, search engine results, or something else. Questions that should be asked when digging further include, "What are people seeing about the product before they enter the website?", "Is most of the traffic for a given type out of the 3 coming from one website or page?". The answers to these questions can give valuable, actionable insight into where a company might want to spend their marketing dollars.


## Analysis Notebooks
### anna_eda
• Explored the distribution of each variable, presence of duplicate rows, null values, investigated outliers, correlation of each variable with the desired outcome 'Revenue', each categorical variables' association with and effect on 'Revenue', and the correlation of all non outcome variables to each other.

### preprocessing_v1
• Loaded the raw dataset dropping impossible sessions, binning variables and encoding variables, log and sqrt transforming columns. Engineer new features, dropping those that are least correlated and/or would make my model's coefficients unstable.

### model_v1
• Split the cleaned data from preprocessing_v1, choosing to use the robust scaler to better handle outliers. Used k-means clustering to segment sessions into 4 distinct groups of online shoppers. Used PCA and t-SNE models to further visualize these groups to confirm they are distinct, separate groups. Fed the full training data into a logistic regression model, outputting a report, confusion matrix, and coefficients for each variable. High coefficient variables were investigated by looking back at the EDA notebook. Identified that low-session Months need to be binned, as their low sample size is less reliable and shouldn't be used in the model when comparing to high-session months.

### preprocessing_v2
• Create a copy of the v1 preprocessing notebook, adding a binning function which was applied to 'Month', grouping low-session months together in an 'Other' category.

### model_v2
• Used the new clean csv created by preprocessing_v2 to split, scale, and model the data on the same logistic regression code, outputting new coefficients. Calculated the odds ratio based off of the coefficients, and interpreted the results.
# Prediction of Customer who will Subscribe to a Term Deposit: To minimize cost of direct marketing campaign
====================================================================================
![GitHub top language](https://img.shields.io/github/languages/top/raulincadet/Bank_marketing?style=plastic)
![GitHub repo size](https://img.shields.io/github/repo-size/raulincadet/Bank_marketing?color=green)
![GitHub language count](https://img.shields.io/github/languages/count/raulincadet/bank_marketing?style=flat-square)
![Lines of code](https://img.shields.io/tokei/lines/github/raulincadet/bank_marketing?color=orange&style=plastic)


## 1) Preamble
A Portuguese banking institution has, in the past, conducted a direct marketing campaign, by phone calls, to increase the number of customers subscribing to a term deposit. The bank should conduct another campaign for the same financial product. However, It costs to conduct such a direct marketing campaign. Indeed, contacting customers who will not subscribe to the term deposit is wasting money. Since the goal of the bank is to maximize profit, it cannot afford to lose money. Then, it is essential that the bank predicts the customers who will subscribe to the term deposit.

This project intends to predict the customers who will buy the term deposit. It should help the bank to minimize the cost of the direct marketing campaign. Minimizing its costs is important for a bank, since it should maximize its profits. To reach this goal


## 2) Methodology

Data used for this project is provided by Moro et al. (2014) to [UCI](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing). To reach the goal of this project, a machine learning model is used, using data from a previous experience of the of the bank. To evaluate the model to predict the customers who will subscribe to the term deposit, the recall rate is used. We expect to reach a recall rate of 95% at least.

### Cleaning and Feature Engineering
After importing the data, I prepare them. Most of the features are string categorical data. I transform them to ordinal integers. This is the case, for examples, of the following features: education, marital, job. About the feature named day, the first day of work is equal 1 and so on until the fifth day which is equal to 5. The same thing is done for the months where their names are replaced by an ordinal integer. Thus, January becomes 1, February becomes 2, and so on. 

A function is created to verify the presence of sparse classes in the features of the training data. If there are sparse classes in a feature, some classes of the feature will be merged. The correlation between the features is calculated. If two features are highly correlated, a correlation superior to 0.6, one of them will be removed from the training and testing data.

The numerical features are scaled, using the MinMaxScaler method. The categorical data transformed as integers are not scaled. The numerical features used in training data are: 'Age', 'IPC', 'Confidence'. The categorical features used are: 'Couple', 'Education', 'Job', 'Loan', 'Housing', 'Default', 'Campaign', 'Day', 'Month'.

### Criteria of model selection
To evaluate the model, the metric *recall* is chosen. Indeed, to minimize the cost of the next marketing campaign, the bank should contact only customers who will buy a term deposit certificate. It should not contact customers who won't buy a term deposit. Since the recall score is the rate of true positive rate, the best model will be the one with the highest recall score. This metric is computed to compare several models and several parameters, to choose the best model that increases the precision of the prediction. The project criterion is to select a model with a recall score which is at least equal to 0.95.

If the recall score is lower than 0.95 for all the models considered, new features will created and an ensemble method will be used to improve the score.  The ensemble method used is the gradient boosting classifier. If the recall score is still lower than 0.95,  a combination of two models will be tried to improve this score.



## 3) Results

### Cleaning data
Results show no sparse classes in the features. There is no need to merge some classes in the features. Regarding correlation of features, the features EmployementRate, IPC and Euribor are highly carrelated. Two of them are removed: EmployementRate and Euribor. The correlations are calculated after removing EmployementRate and Euribor among the features. The heatmap shows that there is not any feature highly correlated with another one.

### Model selection
The scores show that the two models with the best recall score are: logistic regression; kernalized support vector with RBF as kernel. However, it is possible to find best precision score changing the parameters of the kernalized support vector model. GridSearchCV is used look for parameters that produce better recall scores. These parameters are: 'C'= 1, 'gamma'=0.01. The related recall score is 0.89, which is lower than the criteria of the project which is 0.95.

To improve the recall score, two new features are created: Economic and Calendar. The features IPC and Confidence are combining to create the new feature Economic. IPC and Confidence are removed. The new feature Calendar is created multiplying the categorical features Day and Month. The features used to train the models are slightly different from the previous estimations. The features used are: "Age", 'Calendar', 'Economic', 'Couple', 'Education', 'Job', 'Loan', 'Housing', 'Default', 'Campaign', 'Day', 'Month'.

The recall score obtained after the modifications in the features is similar to the previous one. To improve the recall score, an ensemble method is used: the gradient boosting classifier. I look simultaneously for the learning rate, the maximum features and the maximum depth that return the best recall score. The results show that the best recall is obtained when the maximum features is set to 4, the learning rate is 0.05 and the maximum depth is 1. However, this best recall score (0.899) is similar to the previous, and then lower than the criteria of the project which is 0.95.

#### Combining two models
In this subsection the mean of the estimated probabilities from two models is calculated, and several values of the threshold are iterated to find the highest value of the recall score obtained. The combination of two models from these three is used: SVC, logistic regression, MPL classifier. The best recall score is obtained when combining the probabilities estimated by the logistic regression model and the MPL classifier model, with a threshold of 0.05. This threshold is used to transform the probabilities to binary values.

## 4) Conclusion

The recall score obtain is higher than the goal of the project. While the criterion of the project was to obtain a recall score of 0.95 at least, the one obtain when combining two models is 0.98. Thus,  we expect that 98% of the customers the model predicts as buying a term deposit certificate if contacted during the marketing campaign, will truly buy it. In this case, the bank can earn profit upon 98% of the customers contacted during the marketing campaign. Only 2% of the cost of the marketing campaign is expected to be wasted money, since it is expected that 2% of the customers contacted won’t buy a term deposit certificate. 
The prediction of the customers who will buy a term deposit helps the bank managers to minimize the cost of the next marketing campaign. Without this prediction, the bank could waste more than 2% of the cost of the marketing campaign, since it could contact a lot of customers that would not buy a term deposit in spite of the campaign.

## Bibliography
Paper: S. Moro, P. Cortez and P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, June 2014

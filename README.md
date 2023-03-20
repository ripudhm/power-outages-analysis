# power-outages-analysis
Project 5 for DSC 80 


## Problem Identification

We began this project by defining a main goal: to predict the duration of power outages in the United States. Using a provided dataset of info for each corrsponding power outage, we will make use of the 'OUTAGE.DURATION' as our response variable. We are approaching a solution by using linear regression, and we have chosen the R^2 coefficient as our metric of evaluation. The reasoning behind choosing OUTAGE.DURATION as our response variable is due to its versatility and leading role in the dataset. Much of the data in the dataset, i.e. annual number of customers served in the electricity sector and average monthly price in the US state have very different trends depending on what group they were in. Therefore, the OUTAGE.DURATION column is a more encompassing set and can be predicted in a larger scope, without needing to take into account smaller details like state and residential sector. All information provided would be of past outages and should be the most up to date data.

## Baseline Model

The baseline model that we created comprises of 5 features. These are 4 nominal categories: 'MONTH', 'NERC.REGION', 'CAUSE.CATEGORY', and 'CLIMATE.CATEGORY', and 1 quantitative category, 'CUSTOMERS.AFFECTED'. We one-hot encoded every nominal feature and passed through CUSTOMERS.AFFECTED, receiving a returned R^2 of about 0.06. The reason why these features are used is due to us implementing the following logic and prior discoveries: there seem to be higher outage duration during the summer, aka months June through August, which meant that the months must be crucial to predicting duration. Region and climate could have also have an effect, as different regions have different climates per month. The cause is crucial as well, as a storm outage may be more difficult to fix than an accidental or directed outage, as there is no weather preventing repairs from moving smoothly. The final feature, customers affected, should also be important, as the more customers in an area, the greater the urgency to resolve the outage. We believe that the current model may not be enough as, barring the incredibly low R^2 number, the data is not standardized for customers affected, as different regions may have a different population of customers, which is not taken into account when glancing at the unstandardized column. The column also needs more quantitative features, which are lacking in this dataset.

## Final Model 

For the final model, we included all the original features from the baseline model and then, for new features, standardized the 'CUSTOMERS.AFFECTED' feature and added two features: a new 'ANOMALY.LEVEL' feature and a standardized 'POPULATION' features. The reasoning for including the original features from the baseline model still stands, but the reason behind the two new features is thus: we standardize 'CUSTOMERS.AFFECTED' in order to ensure that the different groups of customers affected from different regions are comparable in the same group. 'ANOMALY.LEVEL' was included because the change in the Oceanic Nino Index that the variable is measuring, aka the difference from the average surface temperature of water in the pacific, is often very indicitave of unusually strong weather events, and therefore would theoretically have a large impact on the frequency of weather-related outages. This data is not standardized as it is already standardized by nature. 'POPULATION' however IS standardized in order to take into account outlying large and small populations from different regions. Population would be probable to improve the model as population size may affect urgency of fixing outages, or change the number of available workers. These added features improved the model to an R^2 of 0.24.

The hyperparameter that we decided to perform gridsearch on was 

Describe the model you chose, 
the hyperparameters that ended up performing the best, 
and the method you used to select hyperparameters and your overall model. 
Describe how your Final Model's performance is an improvement over your Baseline Model's performance.
## Fairness Analysis

For our fairness anaylsis, we decided to binarized the state column into states that were not California (0) and states that were California (1). The reason for us defining these two groups is so that we could determine whether or not to fail to reject our null hypothesis, that our model performs better for outages in the state of cali, or fail to reject our alternative hypothesis, that the model performs the same for all outages. Our choice of test statistic was the difference in RMSE values between these two defined groups, and we chose a significance level of 0.05. After performing the permutation testing, we came up with a resultant p-value of 0.13, which means that we reject the null hypothesis that there is a bias toward california in the model.

<iframe src="assets/diff_rmse.html" width=800 height=600 frameBorder=0></iframe>
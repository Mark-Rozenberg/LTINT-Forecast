# LTINT-Forecast
long term interest rates forecast using macro indicators

## Description
in this project i try to forecast future developmnet of long term interest rates. to achieve the goal i collected data from OECD for various countries and a few macro indicators: unemployment, wages, shares prices, consumer price index (cpi) and consumer confidence index (cci). for forecast i use simple linear regression and more complexed Sarimax model, that based on arima principle. to improve models performance i used Sequential Feature Selector (sfs) model to eliminate variables that do not contribute much to explain the dependent variable, long term interest rate.

## Data
### source
the source of the data is OECD. you can find the data in link-1. i used only economic macro indicators that known, by research, to have conectivity with interest rates. i choosed to use only indicators that availible with monthly freqeuncy. OECD data have other variables known to have strong connection to iterest rates, for example gross domestic product (GDP), but this variables collected only with quarterly frequency. at this stage i had 2 options:
a. to use raw data with one type of frequency, monthly or quarterly
b. to normalize / extrapolate the quarterly data into monthly or other way monthly to quarterly
i choose to stick to the raw data because normalization can impact strongly the results and their interpretation. between the monthly and quarterly i choose monthly data due to the fact that it more reach in variables and because the option to catch shorter impacts between the variables.

### subset extraction
the data is not totaly complete and there is gaps, missing data, for some countries and on one or more variables. the biggest problem is in wage data. not for all countries this data collected, because of this i tested my models twice:
1. without wage variable, with much wider range of countries in the sample
2. with wage variable but with much less countries in the sample
for the sample i took Jan-2005 to Dec-2023 time range. the sample concluded from coutries that have complete 228 months of positive, non missing, values for all variables. in version 1, without wage data, the sample consist of 21 countries and in version 2, with wage data, 10 countries.

### adding lag variables
for each independent variable i added 3 lag variables to catch impact that occurs in delay.

### pivot the data
after adding lag variables my main table have a panel data form where month and country are the categories and the macro indicators are the variables. 

### train test split
for training and test i splited the data, the train from Jan-2005 till Jan-2020 and test from Feb-2020 to Dec-2023. 

## Models
our goal is to forecast time series data. for that i used two models: linear regression and SARIMAX model (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors). SARIMAX extends the ARIMA model by incorporating seasonal effects and exogenous variables. with these capabilities, the SARIMAX model can deliver more accurate and reliable forecasts for long-term interest rates in this project.

after first run i used feature elimination technique, Sequential Feature Selector (sfs). the reason for that is some variables can have low impact if at all on the target / dependent variable so actually this variables adding noise to our model and can decrease the accuracy and reliability of coefficients of other more stronger vars. when we use feature elimination we droping out unecessary noise and improve robustness of other features such that the final model is more accurate and robust. i decided that i will keep around 1/3 of the features, explanatory variables.

### Feature Elimination Technique

Feature elimination is a crucial step in building predictive models, especially when dealing with a large number of explanatory variables. The primary goal of feature elimination is to improve the model's performance by removing variables that do not significantly contribute to the prediction of the target variable. Here are some key points on the importance of feature elimination:

1. **Reduces Overfitting**: By eliminating irrelevant or less important features, the model becomes less complex and more generalizable. This helps in reducing overfitting, where the model performs well on training data but poorly on unseen test data.

2. **Improves Model Accuracy**: Removing noise from the dataset by eliminating unimportant features can lead to more accurate predictions. This is because the model focuses on the most relevant information, leading to better performance metrics.

3. **Enhances Interpretability**: A model with fewer features is easier to interpret and understand. This is particularly important in fields where understanding the relationship between variables is crucial, such as economics and finance.

4. **Reduces Computational Cost**: Fewer features mean less computational resources are required for training the model. This can be particularly beneficial when working with large datasets or complex models.

5. **Increases Robustness**: By focusing on the most significant features, the model becomes more robust to changes in the data. This means that the model is more likely to perform well on new, unseen data.

In this project, Sequential Feature Selector (SFS) was used for feature elimination. SFS is a stepwise approach that adds or removes features based on their contribution to the model's performance. By using SFS, the final model retained only the most impactful features, leading to improved accuracy and robustness.

## Results
i tested the model for USA interest rates. the models are compared using 4 measures: mean absolute error (MAE), mean squared error (MSE), explained variance score (EVS), coefficient of determination -> R-squared (COD). the model with the best fit was of version 2, with wage data, after feature elimination.
when comparing the models with complete set of variables to those of reduced form, the reduced form are more accurate. the reason for that is because we use info from other different countries, info that not contribute to the exaplanation power of the model when comparing models with wage data to those without. 
the gaps are significant comparing to the models without wage data. it can indictaes that for USA the wage data is very important in determining the interest rates.

## Summary

## Discussion

## links
1. OECD data-explorer: https://data-explorer.oecd.org/?lc=en&pg=0
2. Sarimax model: https://www.statsmodels.org/dev/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html
3. sfs model: https://scikit-learn.org/1.5/modules/generated/sklearn.feature_selection.SequentialFeatureSelector.html


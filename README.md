# LTINT-Forecast
long term interest rates forecast using macro indicators

## Description
in this project i try to forecast future developmnet of long term interest rates. to achieve the goal i collected data from OECD for various countries and a few macro indicators: unemployment, wages, shares prices, consumer price index (cpi) and consumer confidence index (cci). for forecast i use simple linear regression and more complexed Sarimax model, that based on ARIMA principle. to improve models performance i used Sequential Feature Selector (sfs) model to eliminate variables that do not contribute much to explain the dependent variable, long term interest rate. the results show that the models perform much better after applying feature selection model and also wage are found to be sigfincant factor to explain the target variable. as another source of comparison i used mixed effects linear model, this model produced the worst results.  

## Data
### source
the source of the data is OECD. you can find the data in link-1. simple observation of the data can be found in looker studio workbook, Link-2. i used only economic macro indicators that known, by research, to have conectivity with interest rates. i choosed to use only indicators that availible with monthly freqeuncy. OECD data have other variables known to have strong connection to iterest rates, for example gross domestic product (GDP), but this variables collected only with quarterly frequency. at this stage i had 2 options:
a. to use raw data with one type of frequency, monthly or quarterly
b. to normalize / extrapolate the quarterly data into monthly or other way monthly to quarterly
i choose to stick to the raw data because normalization can impact strongly the results and their interpretation. between the monthly and quarterly i choose monthly data due to the fact that it more reach in variables and because of the option to catch shorter impacts between the variables.

### subset extraction
the data is not totaly complete and there is gaps, missing data, for some countries and on one or more variables. the biggest problem is in wage data. not for all countries this data collected, because of this i tested my models twice:
1. without wage variable, with much wider range of countries in the sample
2. with wage variable but with much less countries in the sample
for the sample i took Jan-2005 to Dec-2023 time range. the sample concluded from countries that have complete 228 months of positive, non missing, values for all variables. in version 1, without wage data, the sample consist of 21 countries and in version 2, with wage data, 10 countries.

### adding lag variables
for each independent variable i added 3 lag variables to catch impact that occurs in delay.

### pivot the data
after adding lag variables my main table have a panel data form where month and country are the categories and the macro indicators are the variables. i pivoted the data such that i left only with one category, month, and the rest of the columns are are the macro indicators with additional 3 lag variables for each indicator for each and every one of the countries in the sample.

### train test split
for training and test i splited the data, the train from Jan-2005 till Jan-2020 and test from Feb-2020 to Dec-2023. 

## Models
our goal is to forecast time series data. for that i used two models: linear regression and SARIMAX model (Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors). SARIMAX extends the ARIMA model by incorporating seasonal effects and exogenous variables. with these capabilities, the SARIMAX model can deliver more accurate and reliable forecasts for long-term interest rates in this project.
In addition to the linear regression and SARIMAX models, I also explored the use of the Mixed Linear Model (MixedLM). MixedLM is particularly useful when dealing with hierarchical or grouped data, which is common in panel data structures. This model accounts for both fixed effects, which are consistent across countries, for example macroeconomic indicators that have a uniform impact on interest rates across all countries and random effects, which vary across countries.

### Feature Elimination Technique

after first run i used feature elimination technique, Sequential Feature Selector (SFS). The primary goal of feature elimination is to improve the model's performance by removing variables that do not significantly contribute to the prediction of the target variable. i decided that i will keep around 1/3 of the features, explanatory variables.

## Results
i tested the model for USA interest rates. the models are compared using 4 measures: mean absolute error (MAE), mean squared error (MSE), explained variance score (EVS) and coefficient of determination -> R-squared (COD). the model with the best fit to the test time range data was of version 2, with wage data, after feature elimination.
when comparing the models with complete set of variables to those of reduced form, the reduced form are more accurate. the reason for that is because we use as explanatory variables macro indicators from various countries, not all this info necessarily contribute to the exaplanation power of the model. the mixed-effects model results were pure, its efficiency paramters were close to models without feature elimination.

## Discussion
there is plentty of other optional models that can be tested here. for example simple time-series forecast models, without explanatory variables, like Prophet, LSTM (Long Short-Term Memory) networks, VAR (Vector AutoRegression) and more. also i would test even smaller amount of explanatiry variable, i.e i used 1/3 of explanatory variables, maybe 1/4 or 1/5 of all variables will produce even better results.

## links
1. OECD data-explorer: https://data-explorer.oecd.org/?lc=en&pg=0
2. Looker Studio Workbook: https://lookerstudio.google.com/reporting/a49941c3-c618-4be3-b676-857fd7ae2761
2. Sarimax model: https://www.statsmodels.org/dev/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html
3. sfs model: https://scikit-learn.org/1.5/modules/generated/sklearn.feature_selection.SequentialFeatureSelector.html
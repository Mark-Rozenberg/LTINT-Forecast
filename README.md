# LTINT-Forecast
long term interest rates forecast using macro indicators

## Description
in this project i try to forecast future developmnet of long term interest rates. to achieve the goal i collected data from OECD for various countries and a few macro indicators: unemployment, wages, shares prices, consumer price index (cpi) and consumer confidence index (cci). for forecast i use simple linear regression and more complexed Sarimax model, that based on arima principle. to improve models performance i used Sequential Feature Selector (sfs) model to eliminate variables that do not contribute much to explain the dependent variable, long term interest rate.

## Data
### source
the source of the data is OECD. you can find all data in link-1. i used only economic macro indicators that known, by research, to have conectivity with interest rates. i choosed to use only indicators that availible with monthly freqeuncy. OECD data have other variables known to have strong connection to iterest rates, for example gross domestic product (GDP), but this variables collected only with quarterly frequency. at this stage i had 2 options:
a. to use raw data with one type of frequency, monthly or quarterly
b. to normalize / extrapolate the quarterly data into monthly or other way monthly to quarterly
i choose to stick to the raw data because normalization can impact strongly the results and their interpretation. between the monthly and quarterly i choose monthly data due to the fact that it more reach in variables and because we will have the option to catch more shorter impacts between the variables.

### subset extraction
the data is not totaly complete and there is gaps, missing data, for some countries and one or more variables. the biggest problem is in wage data. not for all countries this data collected, because of this i tested my models twice:
1. without wage variable, with much wider range of countries in the sample
2. with wage variable but with much less countries in the sample
for the sample i took Jan-2005 to Dec-2023 time range. the sample concluded from coutries that have complete 228 months of positive, non missing, values for all variables. in version 1, without wage data the sample consist of 21 and in version 2, with wage, 10 countries.

### adding lag variables
for each independent variable i added 3 lag variables to catch impact that ocuurs in delay.

### train test split
for training and test i splited the data, the train from Jan-2025 till Jan-2020 and test from Feb-2020 to Dec-2023. 

## Models
our goal is forecasting time series data. for that i used two models: linear regression and Sarimax, which is based on ARIMA model, documentation of Sarimax in links below.
after first run i used feature elimination technique Sequential Feature Selector (sfs). the reason for that is some variables can have low impact if at all on the target / dependent variable so actually this variables adding noise to our model and can impact the reliability of the more stronger vars. when we use feature elimination we droping out unecessary noise and improve robustness of other features such that the final model is more accurate and robust. i decided that i will keep around 1/3 of the features, explanatory variables.

## Results
i tested the model for USA interest rates. the model with the best fit was of version 2, with wage data, after feature elimination. the gaps are significant comparing to the models without wage data. it can indictaes that for USA the wage data is very important in determining the interest rates.

## Summary

## Discussion

## links
1. OECD data-explorer: https://data-explorer.oecd.org/?lc=en&pg=0
2. Sarimax model: https://www.statsmodels.org/dev/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html
3. sfs model: https://scikit-learn.org/1.5/modules/generated/sklearn.feature_selection.SequentialFeatureSelector.html


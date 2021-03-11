# real-estate-investment-analysis-and-projection
## Maximizing return via timeseries modeling 

# Scenario:
### As an analyst for a real estate investment company I am tasked with finding US zipcodes to invest in using time series modeling to predict future prices.

# Data:
### CSV table of almost 15,000 US zip codes and their median home prices from 1996-2018.
### Each zip code contains, in addition to historical prices: it's city, state, metro area, county, and size rank. 
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/data_over.png" width="600" height="220">

# EDA
### I started by focusing my analysis on less risky investments. I decided to use performance during the Great Recession as a proxy for risk. I created a variable called recession_return, sorted the data by recession_return, and made the top 5 results into a new data frame.
### Next, I cleaned up this new data frame:
* I dropped all the variable (columns) asside from date prices and zip code
* Set zipcode to the index
* Transposed the data so dates were the index
* Converted dates to datetime
* Changed the zip codes (columns) to strings
### With the data in the proper form I took a look...
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/price_graph.png" width="600" height="400">

### All of the zip codes appear to trend upwards so I tested for stationarity.
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/stat_test_origin.png" width="600" height="600">
### All of my zip codes have dickey-fuller test values over 0.05 so I tested again afer differencing once.
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/stat_test_differ1.png" width="600" height="600">
### After differencing once they all passed. 

### Next I ran autocorrelation and partial autocorrelation functions on my zip codes.
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/exemple_acf:pacf.png" width="500" height="800">
### All of the zipcodes look to have ACFs that are significant upto around 25 and PACFs upto 2.

### For my final EDA step I ran a seasonal decomposition function to look at trends and seasonality.
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/example-seasonal_decomp.png" width="500" height="800">
### There appears to be a clear seasonal trend within each year. And, again, we see an upward trend.

# Initial Models
### I decided to use an autoregressive model (ar) with 1 term as my baseline. I then compared it to a moving average model with 1 term, an ARMA model, and an ARIMA model (an ARMA model diferenced once). Additionally, while all the prior models only used one term I used grid searching to automaticlly find the best terms for another ARIMA model and SARIMA model.
### I compared these models by thier AIC scores and RMSE's. Instead of using traditional RMSE numbers I converted them to percentages, by dividing the RMSE by the average price of homes in each zip code, to more easily compare. 
### To calculate this I split my data into testing data, my last 18 months of data (since my projection will be 18 months), and my training data, all but the last 18 months. I formatted the results into tables to compare.   
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/test_rmse.png" width="600" height="600">
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/train_rmse.png" width="600" height="600">
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/aics.png" width="600" height="600">
### In the AIC and test RMSE results the autoSARIMA did best, with the exeption of 10128 in test RMSE. Becuase of this I selected the autoSARIMA model for my final price predictions.

# Final Models 
### Using the autoSARIMA model I predicted the price of homes in each zip code 18 months out.
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/73128-fin.png" width="800" height="600">
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/47220-fin.png" width="800" height="600">
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/47281-fin.png" width="800" height="600">
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/73064-fin.png" width="800" height="600">
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/10128-fin.png" width="800" height="600">
# Above you can see results of each model's preditcion. All but one show variuos positive values. And they all appear to have predicted values that are fairly close to their actual values.

# Conclusion
### My final models predict 47281 (Vallonia, Indiana) and 73064 (Mustang, Oklahoma) to have the best returns around 4%. Given the national average 18 month return of 5.2% this is a bit low but given my focus on lower risk investments this could make sense for more risk averse investors. Additionally, 10128 (New York City, New York) is expected to show a loss in this period so investors should avoid investing there. On a final note, risk averse investors looking to invest across multiple zip codes might consider investing in 47220 (Brownstown, Indiana) given it's lower but still positve return.

# Future work
### Given my focus on lower risk investing it is worth analyzing how these zipcodes did during of economic down turns to confirm that they are low risk. I would also like to look at other zipcodes that did well during the Great Recession to see if they potentially offer better returns, lower risk, or simply to diversify. It is also worth looking into how using leverage/debt might affect expected returns for the less risk averse. And finnally, adding exogenous variables could help imporove my model's fit and make better predictions.
   

# real-estat-investment-analysis-and-projection
## Maximizing return via timeseries modeling 

# Scenario:
### As an analyst for a real estate investment company I am tasked with coming up with finding US zipcodes to invest in using time series modeling to predict future prices.

# Data:
### CSV table of almost 15,000 us zipcodes and their median home prices from 1996-2018
### Each zipcode contains, in addition to historical prices: it's city, state, metro area, county, and size rank 
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/data_over.png" width="600" height="220">

# EDA
### I started by focusing my analysis on less risky investments. I decided to use performance during the Great Recession as a proxy for low risk. I created a variable called recession_return, sorted the data by recession_return, and made the top 5 results into a new datat frame.
### Next, I cleaned up this new data frame:
* I dropped all the variable (columns) asside from price and zipcode
* Set zipcode to the index
* Transposed the data so dates were the index
* Converted dates to datetime
* And changed the zip codes (columns) to strings
### With the data in the proper form I took a look...
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/price_graph.png" width="600" height="400">

### All of the zip codes appear to trend upwards so next I tested for stationarity.
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/stat_test_origin.png" width="400" height="600">
### All of my zip codes have dickey-fuller test values over 0.05 so I tested again afer differencing once.
<img src="https://github.com/s-shader/real-estat-investment-analysis-and-projection/blob/main/mod-pictures/stat_test_differ1.png" width="400" height="600">


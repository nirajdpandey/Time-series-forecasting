### Time-series-forecasting
This repository is dedicated for time-series forecasting 

#### Content
```
    1. checking stationarity in the time-series
    2. if it doesn't have stationarity property, make it stationary by using rolling mean
    3. Arima forcasting by using AIC to find best parameter 
    4. MSE and MAPE loss analization
    
```    
### checking stationarity

A common assumption in many time series techniques is that the data are stationary.
A stationary process has the property that the mean, variance and autocorrelation structure do not change over time. Stationarity can be defined in precise mathematical terms, but for our purpose we mean a flat looking series, without trend, constant variance over time, a constant autocorrelation structure over time and no periodic fluctuations (seasonality).

For practical purposes, stationarity can usually be determined from a run sequence plot.

### DATA
The data we are dealing with look as follows 
![cb](https://github.com/nirajdevpandey/Time-series-forecasting/blob/master/data/original_data.jpg)
***
### Trend 
Let's check the trend in the data 
![dd](https://github.com/nirajdevpandey/Time-series-forecasting/blob/master/data/trend.jpg)
***
### Seasonality 
![dd](https://github.com/nirajdevpandey/Time-series-forecasting/blob/master/data/seasonality.jpg)

***
## checking stationarity
```python
def TestStationaryAdfuller(ts, cutoff = 0.01):
    ts_test = adfuller(ts, autolag = 'AIC')
    ts_test_output = pd.Series(ts_test[0:4], index=['Test Statistic','p-value','#Lags Used','Number of Observations Used'])
    
    for key,value in ts_test[4].items():
        ts_test_output['Critical Value (%s)'%key] = value
    print(ts_test_output)
    
    if ts_test[1] <= cutoff:
        print("Strong evidence against the null hypothesis, reject the null hypothesis. Data has no unit root, hence it is stationary")
    else:
        print("Weak evidence against null hypothesis, time series has a unit root, indicating it is non-stationary ")
```
***
This function will return one of the above two statement which will represent whether the time-series is staionary or not.

## Arima

```python
mod = sm.tsa.statespace.SARIMAX(train_data,
                                order=SARIMAX_model[AIC.index(min(AIC))][0],
                                seasonal_order=SARIMAX_model[AIC.index(min(AIC))][1],
                                enforce_stationarity=False,
                                enforce_invertibility=False)

results = mod.fit()
```
***
Above one can see the script to fit `SARIMA` model after getting best parameter through `AIC`

```python
plt.style.use('ggplot')
results.plot_diagnostics(figsize=(15, 10))
plt.savefig('Arima1.jpg')
plt.show()
```
![alt text](https://github.com/nirajdevpandey/Time-series-forecasting/blob/master/data/Arima1.jpg)
***
In the plots above, we can observe that the residuals are uncorrelated (bottom right plot) and do not exhibit any obvious seasonality (the top left plot). Also, the residuals and roughly normally distributed with zero mean (top right plot). The qq-plot on the bottom left shows that the ordered distribution of residuals (blue dots) roghly follows the linear trend of samples taken from a standard normal distribution with N(0, 1). Again, this is a strong indication that the residuals are normally distributed.

![Arima_prediction](https://github.com/nirajdevpandey/Time-series-forecasting/blob/master/data/prediction.jpg)
***
Looking at the figure, the model seems to do a pretty good job at modeling the time series. The blue and purple lines are, as expected, very close to the red ground truth. What is more interesting is the gray line, the out of sample predinction. For such a simple time series, the ARIMA model is able to forecast the 1960 values accurately.

> Thanks a lot

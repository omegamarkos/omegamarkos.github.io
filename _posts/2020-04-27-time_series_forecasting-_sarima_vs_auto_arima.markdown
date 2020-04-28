---
layout: post
title:      "Time series forecasting- SARIMA vs Auto ARIMA !"
date:       2020-04-27 16:35:02 -0400
permalink:  time_series_forecasting-_sarima_vs_auto_arima
---

Time series is a series of data points measured at consistent time intervals such as yearly, daily, monthly, hourly and so on. It is time-dependent & the progress of time is an important aspect of the data set. One of the most common methods used in time series forecasting is known as the ARIMA model, which stands for Auto Regressive Integrated Moving Average. ARIMA is a model that can be fitted to time series data to predict future points in the series.<br>
We can split the Arima term into three terms, AR, I, MA:<br><br>
**AR(p**) stands for the autoregressive model, the p parameter is an integer that confirms how many lagged series are going to be used to forecast periods ahead.<br><br>
**I(d**) is the differencing part, the d parameter tells how many differencing orders are going to be used to make the series stationary.<br><br>
**MA(q)** stands for moving average model, the q is the number of lagged forecast error terms in the prediction equation. SARIMA is seasonal ARIMA and it is used with time series with seasonality.<br><br>
There are a few steps to implement an ARIMA model:<br><br>
** Import the necessary libraries & Load the data:** The first step for model building is to load the data set & import libraries.<br><br>
![](https://miro.medium.com/max/875/0*HJd2VNx9PrZx77Go)<br>

![](![image](https://user-images.githubusercontent.com/23279623/80418789-ee775200-88a5-11ea-81ea-c3a0ca94e2f1.png))
We will be working on Zillow median house data for a specific zip code.<br><br>
**2. Data Preprocessing:** While working with time series data in Python, it’s important to always ensure that dates are used as index values and are understood by Python as a true “date” object. We can do this by using pandas datestamp or to_datetime method.<br><br>
![image](https://user-images.githubusercontent.com/23279623/80419515-48c4e280-88a7-11ea-8612-a669c64eca64.png)<br>
![](![image](https://user-images.githubusercontent.com/23279623/80501109-0b129900-893d-11ea-80b0-6bc122322c66.png))
**3. Check for stationarity:** Most time series models require the data to be stationary. A time series is said to be stationary if its statistical properties such as mean, variance & covariance remain constant over time. The formal ways to check for this are plotting the data and do a visual analysis and use a statistical test.<br><br>
**Visual:** we can use the decomposition method which allows us to separately view seasonality (which could be daily, weekly, annual, etc), trend and random which is the variability in the data set after removing the effects of the seasonality and trend.
![](![image](https://user-images.githubusercontent.com/23279623/80501948-0e5a5480-893e-11ea-8f66-0bfdb9f3d4d7.png))
![](![image](https://user-images.githubusercontent.com/23279623/80502086-377ae500-893e-11ea-84f8-61904f49b3c8.png))
The plot shows that the data has both trend & seasonality. That means it is not stationary.<br>
**Statistical test:** To confirm our visual observation on the above plot, we will use the Dickey-Fuller Hypothesis testing.<br>
**Null Hypothesis:** The series is not stationary.<br>
**Alternate Hypothesis:** The series is stationary.
![](![image](https://user-images.githubusercontent.com/23279623/80502792-0f3fb600-893f-11ea-800a-52d11b5fdcfd.png))
![](![image](https://user-images.githubusercontent.com/23279623/80502906-31d1cf00-893f-11ea-889d-091c28134f31.png))
With the p-value 1 which is greater than 0.05, we fail to reject the null hypothesis & it confirms that the series is not stationary.<br>
**4. Make series stationery & determine the d value:**<br>
 After the statistical test confirmed that the series is not stationary, the next step is to remove the trend and make the series stationary. One of the most common methods of dealing with removing both the trend and seasonality is differencing and the number of times the differencing was performed to make the series stationary is the d value.<br>
 ![](![image](https://user-images.githubusercontent.com/23279623/80503180-84ab8680-893f-11ea-9dc8-7856d32d8bde.png))
 After a couple of differencing the test confirms that the data is stationary. That means the d value is 2.<br>
**5. Create ACF and PACF plots & determine the p and q values:** <br>
The Partial Autocorrelation Function ( PACF) gives the partial correlation of a time series with its own lagged values, controlling for the values of the time series at all shorter lags. The Autocorrelation Function gives the correlation of a time series with its own lagged values but without controlling the other lags.<br>
The ACF plot for the AR(p) time series would be strong to a lag of p and remain stagnant for subsequent lag values, trailing off at some point as the effect is weakened. The PACF, on the other hand, describes the direct relationship between an observation and its lag. This generally leads to no correlation for lag values beyond p.<br>
The ACF for the MA(q) process would show a strong correlation with recent values up to the lag of q, then an immediate decline to minimal or no correlation. For the PACF, the plot shows a strong relationship to the lag and then a tailing off to no correlation from the lag onwards. Below is the ACF & PACFplot for our stationary data.<br>
![](![image](https://user-images.githubusercontent.com/23279623/80503444-dc49f200-893f-11ea-8308-d60f1104c7b5.png))
![](![image](https://user-images.githubusercontent.com/23279623/80503520-faafed80-893f-11ea-8426-21033ca2c2e4.png))
PACF & ACF suggested that AR(2) & MA(2), the next step is to run the ARIMA model using the range of values estimated by the ACF & PACF. Information criterion like AIC (Akaike Information Criterion) or BIC(Bayesian Information Criterion) will be used to choose among correctly fitted models.
**6. Fit ARIMA model:**


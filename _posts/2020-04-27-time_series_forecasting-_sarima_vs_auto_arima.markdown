---
layout: post
title:      "Time series forecasting- SARIMA vs Auto ARIMA !"
date:       2020-04-27 16:35:02 -0400
permalink:  time_series_forecasting-_sarima_vs_auto_arima
---

Time series is a series of data points measured at consistent time intervals such as yearly, daily, monthly, hourly and so on. It is time-dependent & the progress of time is an important aspect of the data set. One of the most common methods used in time series forecasting is known as the ARIMA model, which stands for Auto Regressive Integrated Moving Average. ARIMA is a model that can be fitted to time series data to predict future points in the series.<br>
We can split the Arima term into three terms, AR, I, MA:<br><br>
AR(p) stands for the autoregressive model, the p parameter is an integer that confirms how many lagged series are going to be used to forecast periods ahead.<br><br>
I(d) is the differencing part, the d parameter tells how many differencing orders are going to be used to make the series stationary.<br><br>
MA(q) stands for moving average model, the q is the number of lagged forecast error terms in the prediction equation. SARIMA is seasonal ARIMA and it is used with time series with seasonality.<br><br>
There are a few steps to implement an ARIMA model:<br><br>
**Load the data & Import the necessary libraries & Load the data:** The first step for model building is to load the data set & import libraries.<br><br>
![](https://miro.medium.com/max/875/0*HJd2VNx9PrZx77Go)<br>

![](![image](https://user-images.githubusercontent.com/23279623/80418789-ee775200-88a5-11ea-81ea-c3a0ca94e2f1.png))
We will be working on Zillow median house data for a specific zip code.<br><br>
**2. Data Preprocessing:** While working with time series data in Python, it’s important to always ensure that dates are used as index values and are understood by Python as a true “date” object. We can do this by using pandas datestamp or to_datetime method.<br><br>
![image](https://user-images.githubusercontent.com/23279623/80419515-48c4e280-88a7-11ea-8612-a669c64eca64.png)<br>
![](![image](https://user-images.githubusercontent.com/23279623/80419515-48c4e280-88a7-11ea-8612-a669c64eca64.png)

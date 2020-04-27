---
layout: post
title:      "Time series forecasting- SARIMA vs Auto ARIMA !"
date:       2020-04-27 20:35:01 +0000
permalink:  time_series_forecasting-_sarima_vs_auto_arima
---

Time series is a series of data points measured at consistent time intervals such as yearly, daily, monthly, hourly and so on. It is time-dependent & the progress of time is an important aspect of the data set. One of the most common methods used in time series forecasting is known as the ARIMA model, which stands for Auto Regressive Integrated Moving Average. ARIMA is a model that can be fitted to time series data to predict future points in the series.
We can split the Arima term into three terms, AR, I, MA:
AR(p) stands for the autoregressive model, the p parameter is an integer that confirms how many lagged series are going to be used to forecast periods ahead.
I(d) is the differencing part, the d parameter tells how many differencing orders are going to be used to make the series stationary.
MA(q) stands for moving average model, the q is the number of lagged forecast error terms in the prediction equation. SARIMA is seasonal ARIMA and it is used with time series with seasonality.
There are a few steps to implement an ARIMA model:
Load the data & Import the necessary libraries & Load the data: The first step for model building is to load the data set & import libraries.
![](https://miro.medium.com/max/875/0*HJd2VNx9PrZx77Go)



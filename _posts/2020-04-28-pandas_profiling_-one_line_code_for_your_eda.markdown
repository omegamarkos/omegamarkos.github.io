---
layout: post
title:      "Pandas Profiling -One line code for your EDA"
date:       2020-04-28 15:48:02 -0400
permalink:  pandas_profiling_-one_line_code_for_your_eda
---

A link for this blog on Analytics Vidhya:<br>
[https://medium.com/analytics-vidhya/pandas-profiling-one-line-code-for-your-eda-ee35d2020ea1?source=friends_link&sk=a8a00511ce597ad0d35e616574baf5b2](http://)

Exploratory data analysis (EDA) is the most important part of the data science pipeline. It helps us to fully understand our data and discover patterns, anomalies & test underlying assumptions. It also helps us define the insights we want to get from our data. There are lots of ways to perform our EDA and they all involve a lot of repetitive processes to all the variables involved. This task gets more complicated and is time-consuming, especially when we are dealing with a high dimensional dataset.<br>

The simplest & most commonly used method is the pandas describe() function. It gives us a few important statistics but we still need to perform other tedious tasks to complete our EDA. There is another powerful alternative called pandas profiling.<br>
Pandas profiling is an open-source python module that generates EDA profile reports such as descriptive statistics, quantile statistics, most frequent values, histograms, Correlations, and missing values. All this is done with one line of code.<br>
To get started, first, we need to install pandas_profiling here.<br>
pip install pandas-profiling<br>
Or<br>
conda install -c conda-forge pandas-profiling<br>
The next step is to import pandas profiling & write the one line code. For this blog, I will be using the Forest Fires data set from the UCI Machine Learning Repository.<br>
![](https://user-images.githubusercontent.com/23279623/80527587-95b9bf00-8962-11ea-849f-0a82015cd3e9.png)<br>
This code displays a one-page report. But to make it simple, I will try to show the four major sections of the report, which are Overview, Variable, Correlation & Sample.<br>
**1.Overview**<br>
Dataset info: This section shows general information about our data, such as the number of variables/columns, missing value and the number of observations. It is very similar to the pandas .info() function.<br>
Variable types: In addition to the common numeric & categorical types, other variables such as boolean and date are also recognized.<br>
![](https://user-images.githubusercontent.com/23279623/80527634-ac601600-8962-11ea-8432-246c53b7b2d9.png)<br>
**Warnings:** <br>This shows valuable information such as zero values, duplicates, and rejected variables. To show more info on the Warnings section, I will rerun the report by adjusting the correlation threshold to 0.4 which was by default 0.9.<br>
![](https://user-images.githubusercontent.com/23279623/80527712-cef22f00-8962-11ea-9ad5-5bfe1cbfb542.png)<br>
Now our warnings section shows the correlation coefficient and the rejected variables due to high correlation.<br>
![](https://user-images.githubusercontent.com/23279623/80527797-ee895780-8962-11ea-92d3-1a772e752699.png)<br>
**2. Variables**<br>
The following statistics are generated for each column:<br>
**Quantile statistics:** minimum value, Q1, median, Q3, maximum, range, interquartile range<br>
**Descriptive statistics:** mean, mode, standard deviation, sum, median absolute deviation, coefficient of variation, kurtosis, skewness.<br>
**Histogram:** The frequency distribution for continuous variables and the counts for the categorical variables.<br>
**Common Values:**  The common values and their frequency<br>
**Extreme Values:**   The five minimum and maximum values and their frequencies.<br>

![](https://user-images.githubusercontent.com/23279623/80527879-124c9d80-8963-11ea-849b-f32568237fea.png)<br>
The highly correlated variables will be ignored and excluded from the report. In our example, ‘DC’ is rejected due to a high correlation with ‘DMC’ and it is ignored. But you can include those variables by adjusting the correlation_overrides and supply a list with the rejected columns you want to show.<br>
![](https://user-images.githubusercontent.com/23279623/80527972-34462000-8963-11ea-9e56-184516f719b6.png)<br>
![](https://user-images.githubusercontent.com/23279623/80528051-4f189480-8963-11ea-84ff-01eb8575a702.png)
**3.Correlations**<br>
This shows the heatmap of highly correlated variables, Spearman, Pearson and Kendall matrices<br>
![](https://user-images.githubusercontent.com/23279623/80528203-90a93f80-8963-11ea-8342-50e898ab0fc7.png)<br>
**4.Sample**<br>
This is just like pandas head() function and it shows the first five samples of the data.<br>
![](https://user-images.githubusercontent.com/23279623/80528275-acace100-8963-11ea-82c6-f94da3e7301f.png)
Finally, you can export the report in HTML format by including this in your code.<br>
![](https://user-images.githubusercontent.com/23279623/80528376-d403ae00-8963-11ea-8e50-d2753490fcf7.png)
I hope this helps to speed up your EDA.<br>
Thanks for reading!<br>
References:<br>
[https://pypi.org/project/pandas-profiling/#types](http://)<br>
[https://medium.com/@InDataLabs/why-start-a-data-science-project-with-exploratory-data-analysis-f90c0efcbe49
](http://)




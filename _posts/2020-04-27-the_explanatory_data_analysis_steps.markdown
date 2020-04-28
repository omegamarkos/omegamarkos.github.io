---
layout: post
title:      "The Explanatory Data Analysis steps"
date:       2020-04-27 16:08:15 -0400
permalink:  the_explanatory_data_analysis_steps
---




Explanatory Data Analysis is a very important part of data science. It is a process in which you develop a deeper understanding of the data set by performing initial investigations to discover patterns, spot anomalies, and check assumptions. It is a good practice to understand the data first and try to gather insight from it before you start to develop a model. It can also help to start formalizing the right questions for the data analysis. I approached the explanatory data analysis process in the following steps:<br>
1. Importing important libraries and read data
2. Data Cleaning
3. Create tables and plots
4. Explore data correlation
5. Identification of important features
**1.Importing important libraries and read data**<br>
At this step, I imported the major libraries and read the data set. Using the .info() method I was able to learn that the dataset has 21597 houses with 21 different variables for the size, date, location, condition & features of the houses. This method also shows the column names and their corresponding data type.<br>
**2.Data Cleaning**<br>
**Missing values:**<br>
Running the .isna() method I learned that the data has substantial missing values in the ‘waterfront’ & ‘renovated’ columns and a few missing values in the “view’ column. From my experience as a coordinator for a very challenging data collection process, I am a strong believer of “Do your best to keep your data!” Throwing data is both costly & will affect the accuracy of the data analysis especially when you have small data. I also believe that it is very important to know the right imputation method to impute the missing values. Otherwise, the wrong values will affect the result as well. I replaced the missing values using the mode & median of the columns which are all 0.<br>
**Extraneous values:**<br>

Next, I checked for extraneous values in each column.
for col in kchouse.columns:
print (col,’\n’, kchouse[col]. value_counts(normalize= True).head(), ‘\n\n’)
2.1% The sqft_basement has a value “?”. Since the majority of the houses has no basement the value was replaced by 0<br>
***Duplicates:***<br>
Running the duplicate method result in no duplicate.<br>
**3.Build tables and plots:**<br>
I bought a new house recently and I have a basic background on what matters when it comes to housing prices. But I was so excited to proof two factors I heard from my real estate agent repeatedly, “The season you sell your house matters. But the age of the house doesn’t”.<br>
Does selling your house in the spring season really matter?
I grouped the data by month, number of houses sold & their median price and create a bar plot. I also created a new column for the month the house of sold and created a dummy variable for the seasons.

```
kchouse[‘date’]= pd.to_datetime(kchouse[‘date’])
sale_monthp=kchouse.groupby(kchouse.date.dt.month).agg({‘price’:np.median})
sale_monthp.reset_index().rename(columns= {‘price’:’median price’, ‘date’:’month’})
```


It was very interesting to find out that more houses are sold between April & August and the median price is higher in those months as well. One of the reasons is that it is hard to move in winter months and for families with kids it easier to move after school is closed.
What about the age of the house?
For the sake of better analysis, I changed the year built to a continuous variable by calculating the age of the house and created a new column age_house.
```
kchouse[‘house_age’] =(kchouse.date.dt.year)-(kchouse[‘yr_built’])
```
Price is not affected by the age of the house between the old houses(> 80 yrs old) and the new houses(<20 yrs. old). But the prices for the houses between age 20 to 40 is lower. The data doesn’t have that much info to investigate this. The important variable would be the year the house was renovated but is has 0 value for most of the houses.
I also wanted to answer questions like, is staging the house increase the price, what was the price when the house last sold and how long did the house stay on the market? unfortunately, the data doesn’t provide much info and it was impossible to<br>
**4. Explore data relationships:**<br>
When you build a regression model, one of the requirements is the linearity between the feature and target variable. We can explore this by using the scatter_matrix method. Which plots the distribution of the features and the relationship between the target variable and between each other. We can also identify the categorical variable using this plot. Identification of the categorical variable is very important since we need to create a dummy variable before we to include them as feature variables.
```
kchouse_corr[‘price’].sort_values(ascending = False)
```
The correlation matrix shows that price has a strong positive correlation with sqft of living area and grade.
**5. Identification of important features:**
Other than checking for linearity. It is important to check for multipolarity. If there is a linear relationship between the feature variables that means we are double-counting the effect of a feature on a target variable. Also, when we measure the effect of a change on one feature variable, we assume that the other features are constant. but if they are multicollinear that breaks the assumption.
This is my first data science project & I enjoyed applying what I learn so far. I am excited and looking forward to learning more.
Resources:
https://www.datasciencecentral.com/profiles/blogs/quick-eda-article

---
layout: post
title:      "Feature selection using Scikit-learn"
date:       2020-04-28 12:59:09 -0400
permalink:  feature_selection_using_scikit-learn
---



![](https://user-images.githubusercontent.com/23279623/80513239-c55dcc80-894c-11ea-9a78-f8e14ddf606b.png)
Feature selection one of the most important steps in machine learning. It is the process of narrowing down a subset of features to be used in predictive modeling without losing the total information. Sometimes, feature selection is mistaken with dimensionality reduction. Both methods tend to reduce the number of features in the dataset but in a different way. Dimensionality reduction reduces the number of features by creating new features as combinations of existing ones. All the features are combined to create a few unique features. Feature selection, on the other hand, works by eliminating the irrelevant features and only keeping the relevant ones.<br>
Here are the main advantages of feature selection:<br>
* It improves model performance: when you have irrelevant features in your data, these features act as a noise, which makes the machine learning models perform poorly.<br>
* It leads to faster machine learning models.<br>
* It avoids overfitting, which increases model generalisability.<br>
Feature selection can be done in multiple ways and we will see some of the Scikit-learn feature selection methods here.<br>
**Dataset**<br>
For this blog, I will use the Breast Cancer Wisconsin (Diagnostic) Data Set. It has a total of 32 features including the id & response variable(diagnosis).<br>

```
df = pd.read_csv (‘breast cancer data.csv’)
y =df['diagnosis'].map({'B':0,'M':1})# target variable
X= df.drop((['diagnosis','id']), axis=1) # features after dropping the  target (diagnosis) & ID
```

**Multicollinearity**<br>
Checking for multicollinearity is a very important step during the feature selection process. Multicollinearity can significantly reduce the model’s performance. Removing multicollinear features will both reduce the number of features and improve the model’s performance.<br>
After running the data to pandas profiling, the report shows that 10 features are eliminated from the features list due to multicollinearity.<br>

`pandas_profiling.ProfileReport(df)`

![](https://user-images.githubusercontent.com/23279623/80513601-487f2280-894d-11ea-965a-9847af440d3f.png)
Let’s drop these features from the previous list and move on to the next feature selection method.<br>

```
# variables rejected due to high correlation
rejected_features= list(profile.get_rejected_variables()) 
X_drop= X.drop(rejected_features,axis=1)
X_drop.shape
```

**1.Univariate feature selection**<br>
Univariate feature selection works by selecting the best features using univariate statistical tests such as chi-square. It examines each feature individually to determine the strength of the relationship of the feature with the response variable. SelectKBest is one of the univariate methods which removes all but the specified number of highest scoring features.<br>
**Split the data**<br>

```
# split data train 70 % and test 30 %
from sklearn.model_selection import train_test_split
x_train, x_test, y_train, y_test = train_test_split(X_drop, y, test_size=0.3, random_state=42)`
Then run SelectKbest to select the 5 best features
from sklearn.feature_selection import SelectKBest, chi2
X_5_best= SelectKBest(chi2, k=5).fit(x_train, y_train)
mask = X_5_best.get_support() #list of booleans for selected features
new_feat = [] 
for bool, feature in zip(mask, x_train.columns):
 if bool:
 new_feat.append(feature)
print(‘The best features are:{}’.format(new_feat)) # The list of your 5 best features
```




![](https://user-images.githubusercontent.com/23279623/80513970-d955fe00-894d-11ea-8d62-2c3a2e26c65d.png)
**2.Recursive feature elimination (RFE)**<br>
Unlike the univariate method, RFE starts by fitting a model on the entire set of features and computing an importance score for each predictor. The weakest features are then removed, the model is re-fitted, and importance scores are computed again until the specified number of features are used. Features important score are ranked by the model’s coef_ or feature_importances_ attributes, and by recursively eliminating a small number of features per loop.<br>

```
from sklearn.feature_selection import RFE
estimator = RandomForestClassifier(random_state = 42)
selector = RFE(estimator, 5, step=1)
selector = selector.fit(x_train, y_train)
rfe_mask = selector.get_support() #list of booleans for selected features
new_features = [] 
for bool, feature in zip(rfe_mask, x_train.columns):
 if bool:
 new_features.append(feature)
new_features # The list of your 5 best features

```

**3.Recursive feature elimination with cross-validation (RFECV)**<br>
RFE requires a specified number of features to keep, however it is usually not known in advance how many features are valid. To find the optimal number of features, cross-validation is used with RFE to score different feature subsets and select the best scoring collection of features.<br>

```
from sklearn.feature_selection import RFECV
cv_estimator = RandomForestClassifier(random_state =42)
X_train,X_test,Y_train,Y_test = train_test_split(X, y, test_size=0.3, random_state=42)
cv_estimator.fit(X_train, Y_train)
cv_selector = RFECV(cv_estimator,cv= 5, step=1,scoring=’accuracy’)
cv_selector = cv_selector.fit(X_train, Y_train)
rfecv_mask = cv_selector.get_support() #list of booleans
rfecv_features = [] 
for bool, feature in zip(rfecv_mask, X_train.columns):
 if bool:
 rfecv_features.append(feature)
print(‘Optimal number of features :’, cv_selector.n_features_)
print(‘Best features :’, rfecv_features)
n_features = X_train.shape[1]
plt.figure(figsize=(8,8))
plt.barh(range(n_features), cv_estimator.feature_importances_, align='center') 
plt.yticks(np.arange(n_features), X_train.columns.values) 
plt.xlabel('Feature importance')
plt.ylabel('Feature')
plt.show()
```


![](https://user-images.githubusercontent.com/23279623/80514196-2f2aa600-894e-11ea-8700-221c25aa215b.png)
![](https://user-images.githubusercontent.com/23279623/80514253-3ea9ef00-894e-11ea-80d6-5104f23dee81.png)

Finally, the number of features is reduced to 13 and we have the name of those features. The next step is to fit the models using these features and check the performance.<br>
Thanks for reading!<br>
References:<br>
[11.3 Recursive Feature Elimination | Feature Engineering and Selection: A Practical Approach for Predictive Models](http://)
[https://towardsdatascience.com/feature-selection-techniques-in-machine-learning-with-python-f24e7da3f36e](http://)
[https://medium.com/@contactsunny/what-is-feature-selection-and-why-do-we-need-it-in-machine-learning-28a28520607c](http://)
[https://towardsdatascience.com/feature-selection-using-python-for-classification-problem-b5f00a1c7028](http://)






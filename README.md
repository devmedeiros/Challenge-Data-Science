<p align="center">
  <img src="https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/aluravoz.png" alt="heart with an A inside and you can read 'Alura Voz telecommunication company'">
</p>

![status - completed](https://img.shields.io/badge/status-completed-2ea44f)

# Table of Contents
- [Challenge Data Science](#challenge-data-science)
- [First Week](#first-week)
  * [Reading the dataset](#reading-the-dataset)
  * [Missing data](#missing-data)
  * [Feature Encoding](#feature-encoding)
- [Second Week](#second-week)
  * [Data Analysis](#data-analysis)
  * [The Churn Profile](#the-churn-profile)
- [Third Week](#third-week)
  * [Preparing the dataset](#preparing-the-dataset)
  * [Baseline](#baseline)
  * [Model 1 - Random Forest](#model-1---random-forest)
  * [Model 2 - Linear SVC](#model-2---linear-svc)
  * [Model 3 - Multi-layer Perceptron](#model-3---multi-layer-perceptron)
  * [Conclusion](#conclusion)

# Challenge Data Science

I was challenged to take the role of a new data scientist hired at Alura Voz. This made-up company is in the phone industry and it needs to reduce the Churn Rate.

For the first week, the goal is to clean the dataset provided by an API. Next, we need to identify clients who are more likely to leave the company, using data analysis, afterwards the idea is to use machine learning models to predict the churn rate and guide the decision-making process for Alura Voz.

# First Week

## Reading the dataset

The dataset is available in a JSON file, at first glance it looked like a normal table.

|  | customerID | Churn | customer | phone | internet | account |
|---|---|---|---|---|---|---|
| 0 | 0002-ORFBO | No | {'gender': 'Female', 'SeniorCitizen': 0, 'Part... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'DSL', 'OnlineSecurity': '... | {'Contract': 'One year', 'PaperlessBilling': '... |
| 1 | 0003-MKNFE | No | {'gender': 'Male', 'SeniorCitizen': 0, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'Yes'} | {'InternetService': 'DSL', 'OnlineSecurity': '... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 2 | 0004-TLHLJ | Yes | {'gender': 'Male', 'SeniorCitizen': 0, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 3 | 0011-IGKFF | Yes | {'gender': 'Male', 'SeniorCitizen': 1, 'Partne... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |
| 4 | 0013-EXCHZ | Yes | {'gender': 'Female', 'SeniorCitizen': 1, 'Part... | {'PhoneService': 'Yes', 'MultipleLines': 'No'} | {'InternetService': 'Fiber optic', 'OnlineSecu... | {'Contract': 'Month-to-month', 'PaperlessBilli... |

But, as we can see, `customer`, `phone`, `internet`, and `account` are their separate tables. So I had to normalize them separately and then I just concatenated all these tables into one.

## Missing data

The first time I looked for missing data in our dataset I notice that apparently, that wasn't anything missing, but later on, I noticed that there was empty space and just space not being counted as `NaN`. So I corrected this, and now the dataset had 224 missing values for `Churn` and 11 missing for `Charges.Total`.

```
customerID            0
Churn               224
gender                0
SeniorCitizen         0
Partner               0
Dependents            0
tenure                0
PhoneService          0
MultipleLines         0
InternetService       0
OnlineSecurity        0
OnlineBackup          0
DeviceProtection      0
TechSupport           0
StreamingTV           0
StreamingMovies       0
Contract              0
PaperlessBilling      0
PaymentMethod         0
Charges.Monthly       0
Charges.Total        11
dtype: int64
```

I decided to drop the missing `Churn` because this is going to be the object of our study and there isn't a point in studying something that doesn't exist. For the missing `Charges.Total`, I think it represents a customer that hasn't paid anything yet, so I just replaced the missing value for 0.

## Feature Encoding

The feature `SeniorCitizen` was the only one that came with `0` and `1` instead of `Yes` and `No`. For now, I'm changing it to yes and no, because it'll make the analysis simpler to read.

`Charges.Monthly` and `Charges.Total` were renamed to lose the dot because the dot gets in the way when calling the feature.

# Second Week

## Data Analysis

In the first plot, we can see how much unbalanced our data set is. There're over 5000 clients that didn't leave the company and a little less than 2000 that left.

<p align="center">
  <img src="https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/churn_count.jpg?raw=true" alt="bar plot with two bars, the first one is for 'no' and the second is for 'yes', the first bar is over 5000 count and the second one is around 2000">
</p>

I also generated 16 plots for all the discrete data. I wanted to see if there was any behavior that made some clients more likely to leave the company.

Is easy to see that all, except for `gender`, seems to play a role in determining if a client will leave the company or not. More specifically payment methods, contracts, online backup, tech support, and internet service.

<p align="center">
  <img src="https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/categorical_churn.jpg?raw=true" alt="there are 16 bar plots in the image showing how each feature is correlated with the churn rate">
</p>

In the `tenure` plot, I decided to make a distribution plot for the tenure, one plot for clients that didn't churn and clients that did churn. We can see that clients that left the company tend to do so at the beginning of their time in the company.

<p align="center">
  <img src="https://github.com/devmedeiros/Challenge-Data-Science/blob/main/2%20-%20Second%20Week/tenure_churn.jpg?raw=true" alt="there two plots side-by-side, in the first one the title is 'Churn = No' the data is along the tenure line and is in a U shape. the second plot has the title 'Churn = Yes' and starts high and drops fast along the tenure line">
</p>

The average monthly charge for clients that didn't churn is 61.27 monetary units, while clients that churn were paying 74.44. This is probably because of the type of contract they prefer, but either way, is known that higher prices drive the customer away.

## The Churn Profile

Considering everything that I could see through plots and measures I came up with a profile for clients that are more likely to churn.

- New clients are more likely to churn than older clients.

- Customers that use fewer services and products tend to leave the company. Also, when they aren't tied down to a longer contract they seem to be more likely to quit.

- Regarding the payment method, clients that churn have a **strong** preference for electronic checks and usually are spending 13.17 monetary units more than the average client that didn't leave.

# Third Week

## Preparing the dataset

We start by making dummies variables dropping the first, so we would have n-1 dummies for n categories. Then we move on to look at features correlation.

<p align="center">
  <img src="https://github.com/devmedeiros/Challenge-Data-Science/blob/main/3%20-%20Third%20Week/corr_matrix.png?raw=true" alt="correlation matrix with all the features">
</p>

We can see that the `InternetService_No` feature has a lot of strong correlations with many other features, this is because these other features depend on the client having internet service. So I'll drop all features that are dependent on this one. The same thing happens with `PhoneService_Yes`.

`tenure` and `ChargesTotal` also have a strong correlation, but I'll run the models with these two, as I think they are both relevant.

After dropping these features I move on to normalize the numeric data, `ChargesTotal` and `tenure`.

## Baseline

I made the baseline model using a dummy classifier that guessed that every client behaved the same. It is always guessed that no client will leave the company. By using this approach we got a baseline accuracy score of `0.73456`. I'll use this to compare the following models.

All models moving forward will have the same random state.

## Model 1 - Random Forest

I start by using a grid search with cross-validation to find the best parameters within a given pool of options. The best model was:

```python
RandomForestClassifier(max_depth=15, max_leaf_nodes=75, random_state=22)
```

After fitting this model, the accuracy score was `0.78282`.

## Model 2 - Linear SVC

For this model, I just used the default parameters.

```python
LinearSVC(random_state=22)
```

After fitting, it got an accuracy score of `0.78992`. 

## Model 3 - Multi-layer Perceptron

Here I fixed the solver to LBFGS and used grid search with cross-validation to find a hidden layer size that would be the best. The best model was:

```python
MLPClassifier(hidden_layer_sizes=(1,), max_iter=9999, random_state=22, solver='lbfgs')
```

After fitting its accuracy score was `0.79489`.

## Conclusion

| **Model** | **Acc Score** | **Improvement** |
|-----------|:-------------:|:---------------:|
| Baseline  |    0.73456    |        -        |
| Model 1   |    0.78282    |      6.57%      |
| Model 2   |    0.78992    |      7.54%      |
| Model 3   |    0.79489    |      8.21%      |

I ran three models, all of them using the same `random_state`. The first model was a random forest, with `max_depth = 15` and`max_leaf_nodes = 75`. I used grid search with cross-validation on some random parameters to find this combination. The second model is a simple linear SVC. And lastly, the third model is a neural network with a multi-layer perceptron, in this case, I also used a grid search with cross-validation with LBFGS solver (because of the small dataset) and some random hidden layer sizes. In the end, the MLP had a `hidden_layer_sizes=(1,)`, `solver='lbfgs'` and I set the `max_iter = 9999`.

Even though all the models outperformed the baseline, the best model was the MLP with an 8.21% accuracy increase, compared to the baseline.

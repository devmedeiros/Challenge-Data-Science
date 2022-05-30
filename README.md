<p align="center">
  <img src="https://raw.githubusercontent.com/devmedeiros/Challenge-Data-Science/main/aluravoz.png" alt="heart with an A inside and you can read 'Alura Voz telecommunication company'">
</p>

![status - under development](https://img.shields.io/badge/status-under_development-2ea44f)

# Table of Contents
- [Challenge Data Science](#challenge-data-science)
- [First Week](#first-week)
  * [Reading the dataset](#reading-the-dataset)
  * [Missing data](#missing-data)
  * [Feature Enconding](#feature-enconding)
- [Second Week](#second-week)
  * [Data Analysis](#data-analysis)
  * [The Churn Profile](#the-churn-profile)
- [Third Week](#third-week)
  * [Preparing the dataset](#preparing-the-dataset)
  * [Baseline](#baseline)
  * [Model 1](#model-1)
  * [Model 2](#model-2)
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

In the first plot we can see how much unbalanced our data set is. There're over 5000 clients that didn't leave the company and a little less than 2000 that left.

<!---
![bar plot with two bars, the first one is for 'no' and the second is for 'yes', the first bar is over 5000 count and the second one is around 2000]()
--->

I also generated 16 plots for all the discrete data. I wanted to see if there was any behavior that made some clients more likely to leave the company.

Is easy to see that all, except for `gender`, seems to play a role in determining if a client will leave the company or not. More especifically payment methods, contract, online backup, tech support, and internet service.

<!---
![there are 16 bar plots in the image showing how each feature is correlated with the churn rate]()
--->

In the `tenure` plot, I decided to make a distribution plot for the tenure, one plot for clients that didn't churn and clients that did churn. We can see that clients that left the company tend do so at the beginning of their time in te company.

<!---
![there two plots side-by-side, in the first one the title is 'Churn = No' the data is along the tenure line and is in a U shape. the second plot has the title 'Churn = Yes' and starts high and drops fast along the tenure line]()
---> 

The average monthly charges for clients that didn't churn is 61.27 monetary units, while clients that churn were paying 74.44. This is probably because of the type of contract they prefer, but either way, is known that higher prices drive customer away.

## The Churn Profile

Condensating everything that I could see throu plots and measures I came up with a profile for clients that are more likely to churn.

- New clients are more likely to churn than older clients.

- Customers that use fewer services and products tend to leave the company. Also, when they aren't tied down to a longer contract they seem to be more likely to quit.

- Regarding the payment method, clients that churn have a **strong** preference for electronic checks and usually are spending 13.17 monetary units more than the average client that didn't leave.

# Third Week

## Preparing the dataset

## Baseline

## Model 1

## Model 2

## Conclusion

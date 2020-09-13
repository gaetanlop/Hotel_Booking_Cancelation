# Project Overview

# Final Product

## Data Cleaning
Done with feature Selection

## Exploratory Data Analysis
Done

## Feature engineering

## Model Building
First, I transformed the categorical variables into dummy variables. I also split the data into train and tests sets with a test size of 20%. I tried three different models and evaluated them using Recall. Why did I used Recall over Accuracy/Precision/F Beta score?

In this classification task, we want to reduce the number of false negative, that is to say the number of bookings that we will predict canceled but that will not be. It is also important to reduce the number of false positive, but focusing on false negative is more important since if the hotel choose to allocate another booking to the room, it will be a problem if the first person that have booked the room come (2 families for only one room). It may be expensive for the hotel to deal with it.

Below the different models that I used:

* **Logistic Regression**
* **Random Forest**
* **XGBoost**
* **Voting Classifier hard and soft**

## Model Performance

The Stack Model outperformed the other approaches on the validation set. Here are the results:
* **Logistic Regression**: -Recall= 80.13%	
* **Random Forest**: -Recall= 85.99%
* **XGBoost**: -Recall= 84.70%
* **Voting Classifier hard**: -Recall= 86.60%
* **Voting Classifier soft**: -Recall= 86.50%


## Productionization and Deployment

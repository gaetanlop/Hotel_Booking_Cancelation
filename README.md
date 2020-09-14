# Hotel Booking Cancelation: Project Overview

'Overbooking is a situation when the total number of rooms reserved for a certain period of time exceeds the total number of rooms available for sale for the same period' (https://setupmyhotel.com/train-my-hotel-staff/front-office-training/439-overbooking.html). In this project we will try to create a tool that will help hotels in their Overbooking Strategy. The goal is to find out whether or not a booking will be cancelled. If it is possible to create such a tool, hotels could use it to predict with a high accuracy the number of rooms that will be cancelled and therefore put the room for rent again. Such a strategy can help to maximize the total capacity and increase the overall revenue of the hotel.

* Created a tool that predict whether or not a Booking will be cancelled (Recall ~ 86.60%) to help hotel (city hotel or resort hotel) maximize their profits.
* Data collection on Kaggle Website: https://www.kaggle.com/jessemostipak/hotel-booking-demand
* Cleaned the Data and Removed sources of Data Leakage (four features where present in the dataset, but could not be used to make predictions since we would not have these informations at the time we make the prediction).
* Generated one other feature and Used SelectKMean for Feature Selection.
* Choose Recall over the different metrics of classification problems because I wanted to minimize False Negative Rate (the number of bookings that we will predict canceled but that will not be).
* Optimized Logistic, Random Forest Classifier and XGBoost Classifier using GridSearchCV to reach the best model.
* Stacked three different models using Voting Classifier of Scikit Learn.
* Built a client facing API using flask.

## About the Dataset

The dataset is from kaggle: https://www.kaggle.com/jessemostipak/hotel-booking-demand

It contains 30 columns and approximately 120 000 rows. It contains information for a city hotel and a resort hotel in Portugal such as : lead_time, number of adults, deposit type, price, the number of special requests... The dataset comprehend bookings due to arrive between the 1st of July of 2015 and the 31st of August 2017, including bookings that effectively arrived and bookings that were canceled. In this analysis we will try to predict whether or not a booking will be canceled.

An explanation of what the different columns mean is given in the Exploratory Data Analysis Notebook

## Code and Resources Used

**Python Version:** 3.7

**For Web Framework Requirements:** ```pip install -r requirements.txt```

**For stacking:** https://www.kaggle.com/kenjee/titanic-project-example

**Flask Productionization:** https://towardsdatascience.com/productionize-a-machine-learning-model-with-flask-and-heroku-8201260503d2

## Exploratory Data Analysis

Before starting the EDA I made a few assumptions, I tried to find out if they were valid or not.
 * Repeated guest are less likely to cancel their reservations.
 * The higher the lead time, the higher the chance of cancellation.
 * May be families are les likely to cancel.
 * The number of booking that were cancelled by the customer increases the chance of cancellation.
 * I think that the more number of total_of_special_requests, the less likely the cancellation.
 
After finding out if my assumptions was valid or not, I explored how bookings price for each hotels evolve during the year, checked out the number of nights people stay at the hotel, explored the correlation between features, and finally, I checked if the data was imbalanced or not.
Below are a few highlights from the things I looked at: 

![alt text](https://github.com/gaetanlop/Hotel_Booking_Cancelation/blob/master/price%20evolution.PNG)
![alt text](https://github.com/gaetanlop/Hotel_Booking_Cancelation/blob/master/balance%20data.PNG)
![alt text](https://github.com/gaetanlop/Hotel_Booking_Cancelation/blob/master/Corr.PNG)

## Data Cleaning

After the EDA I needed to clean up the data so that it can be used by a model. I made the following changes:
* Deal with missing values
* Feature Engineering:
  * adding another column 'home_country'
  * deal with data leakage
  * Feature Selecgion using SelectKBest
* Deal with categorical variables

The most important thing that I had to deal with during the Data Cleaning is removing sources of Data Leakage.

## Model Building
First, I transformed the categorical variables into dummy variables. I also split the data into train and tests sets with a test size of 20%.  I tuned hyperparameters using GridSearchCV. I tried different models and evaluated them using Recall. I tuned hyperparameters using GridSearchCV. Why did I used Recall over Accuracy/Precision/F Beta score?

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

![alt text](https://github.com/gaetanlop/Hotel_Booking_Cancelation/blob/master/model%20perf.PNG)


## Productionization and Deployment

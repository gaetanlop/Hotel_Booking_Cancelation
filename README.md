# Hotel Booking Cancelation: Project Overview

Recently, my family and I decided to buy a new apartment in Paris. It gave me the idea to try to understand how the price of an apartment is calculated and what are the main drivers that impact the sale price of an apartment in Paris.

* Created a tool that estimates the price of an apartment in Paris in 2020 (MAE ~ € 74 K) to help people finding the best price to sold or buy an apartment.
*   Scraped more than 4000 sell announcements of apartments in Paris from LogicImmo website using python and selenium.
*   Engineered features from the text of each announcements description to get the different caracteristics of an apartment in Paris such as balcony, patio, cellar, parking, floor of the apartment, floor of the building.
*   Optimized Linear, Lasso, Ridge, Random Forest Regressors and XGBoost Regressors using RandomizedSearchCV to reach the best model.
*   Built a client facing API using flask.
*   Used HTML, CSS, JS for the UI.
*   Used AWS to put the model in production.

## About the Dataset

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

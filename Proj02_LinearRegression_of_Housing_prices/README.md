# Prediction of HDB Resale Flat Prices
**1) Problem Statement**<br>
Objective of the Housing Development Board (HDB) to provide affordable and quality housing to the general Singapore population. Recent events of rising housing prices (worsen with pandemic regulations that disrupt BTO flats construction) have been a concern for HDB and attempt to analyse factors that influence HDB resale prices.

As an analyst in HDB, it is tasked to develop a predictive model to predict resale flat prices based on historical data.

**Notebooks**<br>

01_DataCleaning&EDA<br>
02_Modeling<br>


**README Overview**<br>
[1. Data](#ID1)<br>
[2. Approach](#ID2)<br>
[3. Models Performance](#ID3)<br>
[4. Discussion & Conclusion](#ID4)<br>

# 1. Data:<a class="anchor" id="ID1"></a>
The data used in this project and its details can be found from [DSI-SG-Project-2 Regression Challenge](https://www.kaggle.com/competitions/dsi-sg-project-2-regression-challenge-hdb-price/overview).

The data consists of 77 features which spans from geographical indicators (eg. address, town, street names), amenities indicators (eg. availability of multistorey carpak, proximity of malls, hawkers) and HDB models/types etc.<br>


# 2. Approach:<a class="anchor" id="ID2"></a>

|Approach|Description|
|:---|:---|
|Data Original size|77 columns, 150634 rows|
|Data Cleaning & Processing|<ul><li>Overall duplications check</li><li> Individual feature Data Values check</li><li>Drop redundant and irrelevant features</li><li>Checking for null values and managing them</li><li>Dataframe has 149805 rows and 57 features after cleaning|
|EDA|<ul><li>Trend of resale prices</li><li>Distribution of features</li><li>Correlation between features<ul><li>Correlation of features to resale price</li><li>Correlation between each features|
|Modelling|<ul><li>Preprocessing<ul><li>StandardScaling on numeric feature values</li><li>OneHotEncoding on categorical features</ul><li>Approach 1: Fit cleaned data<ul><li>Linear Regression, Ridge Regression, Lasso Regression</li><li>Model evaluation: R2, RMSE</ul><li>Approach 2: Test for individual and overall significance of certain geographical indicators</li><ul><li>Individual (t-test) and Overall (F-test) test significance</li><li>Ridge Regression, Lasso Regression</li><li>Model evaluation: R2, RMSE</ul><li>Approach 3: Drop highly correlated features and feature engineering</li><ul><li>Ridge Regression, Lasso Regression</li><li>Model evaluation: R2, RMSE|

# 3. Models Performance:<a class="anchor" id="ID3"></a>

**Overview of Models explored:**
|Model|Description|Performance|Conclusion|
|--|--|--|--|
|lr_base|OLS regression model <ul><li>based on dataset (Xtrain_pro_df)|<ul><li>cross_val train score: -4.4788e+16</li><li>test score: 0.93132</li><li>RMSE: 37517.28</ul>|Overfits, not applicable|
|ridge_CV|Ridge regularization with <ul><li>CV of k-folds=5 </li><li>based on dataset (Xtrain_pro_df)|<ul><li>train score: 0.93568</li><li>test score: 0.93136</li><li>RMSE: 37504.41|Best score out of all models|
|lasso_CV|Lasso regularization with <ul><li>CV of k-folds=5 </li><li>based on dataset (Xtrain_pro_df)|<ul><li>train score: 0.90017</li><li>test score: 0.89868</li><li>RMSE: 45566.63</ul>|Poorer score than ridge_CV|
|ridge_CV01|Ridge regularization with <ul><li>CV of k-folds=5 </li><li>based on dataset (Xtrain_pro_df01)<ul><li> "block" and "planning_area" features removed|<ul><li>train score: 0.92745</li><li>test score: 0.92537</li><li>RMSE: 39094.81|2nd best score out of all models|
|lasso_CV01|Lasso regularization with <ul><li>CV of k-folds=5 </li><li>based on dataset (Xtrain_pro_df01)<ul><li> "block" and "planning_area" features removed|<ul><li>train score: 0.90007</li><li>test score: 0.89860</li><li>RMSE: 45566.63|Poorer score than ridge_CV01|
|lr_base02|OLS regression <ul><li>based on dataset (Xtrain_pro_df02) <ul><li>'block', 'mid_storey','lb_storey', 'ub_storey','lease_commence_year', 'year_completed' features removed|<ul><li>cross_val train score: -2.6746e+16</li><li>test score: 0.93132</li><li>RMSE: 40719.38|Overfits, not ideal for prediction|
|ridge_CV02|Ridge regularization with <ul><li>CV of k-folds=5 </li><li>based on dataset (Xtrain_pro_df02) <ul><li>'block', 'mid_storey','lb_storey', 'ub_storey','lease_commence_year', 'year_completed' features removed|<ul><li>train score: 0.92383</li><li>test score: 0.91917</li><li>RMSE: 40698.99|3rd best score out of all models|
|lasso_CV02|Lasso regularization with <ul><li>CV of k-folds=5 </li><li>based on dataset (Xtrain_pro_df02) <ul><li>'block', 'mid_storey','lb_storey', 'ub_storey','lease_commence_year', 'year_completed' features removed|<ul><li>train score: 0.88759</li><li>test score: 0.88650</li><li>RMSE: 48228.8|Poorer score than ridge_CV02|

**Best Performing Model: RidgeCV (approach 1 (data:Xtrain_pro_df))**<br>
*Comment*<br>
- It has an RMSE score of 37504.(i.e resale price predictions +/- $37504). Thus recommended to be using this model for future predictions.
- Top 5 features are: 
    - **floor_area_sqm**: capturing size of flat
    - **flat_model**: HDB model capturing structure of flat layout
    - **multistorey_carpark**: Availability of multi carpark in the same block
    - **lb_storey**: Lower bound of range of storey that the resale flat is sold captures preference of storey in purchase of resale flat
    - **flat_type**: Amount of rooms in the resale flat captures preferences of numbers of room
- Bottom 5 features are: 
    - **other_room_rental**: captures the proportion of other room type rental units in the resale flat block in affecting resale price
    - **multigen_sold**: captures the proportion of multi-gen units in the resale flat block in affecting resale price
    - **3room_rental**: captures the proportion of 3 room rental units in the resale flat block in affecting resale price
    - **bus_stop_nearest_distance**: captures importance of bus as public transport in resale price
    - **studio_apartment_sold**: captures the proportion of studio apartment residential units in the resale flat block in affecting resale price
    
HDB can analyse the coefficients of these factors on both their significance as well as impact on resale price. To keep resale flat price affordable:
    - HDB have to build taller flats since resale prices are sensitive to floor area which can provide more housing space per area of land space.
    - HDB can impose tiered tax/duties on more luxurious flat models and subsidies lower quality flat models to balance demand which could be a contributing factor to higher priced luxurious flat models.
    - Having supporting amenities such as carparks might contributed to higher prices whereas with more public transport ,such as buses, in the proximity can help reduce prices.
    - Looking beyond the top 5 factors, there are impactful geographical factors as well, HDB can impose location based policies, such as raising minimum occupation period in high demand locations or construct larger flats in less popular locations, to balance demand.

# 4. Concluding statements:<a class="anchor" id="ID4"></a>

**Limitation and Furture work**
- Time/technical constraint on more test for significance of coefficients to improve model prediction performance due to long run times of modeling codes.
    
- Linear regression models might be too simplistic to fully understand how significant factors can impact resale flat prices as it attempts to draw a linear relationship between features and price. This neglects non-linear relationships, for example, a marginal increase of 10sqm from 30 to 40 and 70 to 80 do not increase prices at the same rate. Thus, with a more accurate model, HDB is able to better pinpoint and balance policies to effectively improve affordability of public housing.

- Casting a wider net to capture more and significant features, such as financial and demographic information which logically can have heavy impact on purchasing power of buyers and needs of resale flats.
    
- Explore and investigate underlying reasons on why certain impactful and significant features affects resale flat prices. This could lead to understanding new features as well as more effective measures to control resale prices.

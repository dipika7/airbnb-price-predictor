# Project:Price Analysis of Airbnb Listings 
### Collaborators: Dipika Jiandani, Manshi Shah 
 
 
## Background:  
Airbnb is an online market that offers homestays and lodging. Airbnb does not own these lodgings; they just bridge the gap between the guests and the hosts. We chose the Airbnb NYC dataset because it generates tons of data about density of rentals across regions (cities and neighborhoods), price variations across rentals, host-guest interactions in the form of reviews, and so forth. Also, NYC is the highest revenue generating place in the entire US with more than 48000 listings as of December 2019.  
 
## Dataset:  NYC data of Airbnb Listings [http://insideairbnb.com/get-the-data.html] 
Calendar.csv: Each row of this dataset provides us with the bookings for the year 2020 (future booking dates). The major fields of this CSV file are listing id, date, price, adjusted price, minimum nights and maximum nights. It has 7 columns and 1865068 6rows.  Listings.csv: Each row contains a unique listing of Airbnb in NYC. There are 106 attributes and 51097 observations. Some of the major variables are description, neighborhood, transit, access, host location, latitude, longitude, review score, listing type, neighborhood.  
 
## Research questions:  
Primary Research Question: How can the host maximize profits on its listings?  Airbnb generates revenue by charging a 3% of the rent value from the hosts and 6-10% of the rent value from the guests. There are multiple factors that contribute to the price of AirBnB listings. However, in order to maximize the profits for the hosts, we need to identify the most prominent predictors of the price. Hence, we require statistical learning methods to obtain accurate values and the right guidance for the hosts. 
 
Based on the recent trends and research papers, we identified that amenities, host_is_superhost, review_score_rating contribute largely to an increase/decrease in price. However, we need to identify which specify amenities, which particular review score and which specific factor causes an increase. Below is the detailed description of each of our secondary research questions: 
 
## Secondary Research Questions: 
### 1. Which amenities affect the price of listings for the host? 
Amenities play an integral part to attract the tourists to rent an AirBnB listing. However, amenities differ in each listing. Some listings have hot tubs whereas others have swimming pools. As part of our research question, we identify the amenities that have attracted the maximum number of tourists. This analysis will guide the hosts to incorporate these predicted amenities as part of their listings to gain higher profits. 
 
### 2. Which factors contribute to the host being a super host?
Airbnb launched a super host program to reward the hosts with a VIP status. As a super host, an AirBnb host will have more viability and thus leading to higher earning potential. However, there are multiple factors like cancellation policy, response rate etc. that make a host a super host. The goal of this analysis is to convey all the factors and statistics to these hosts. 
 
### 3. Which review rating scores impact the overall review rating?  
Airbnb claims that at least 50% users rate their stay. Airbnb provides users to rate their stay based on 6 factors: cleanliness, location, communication, accuracy, check in and value. Review rating of each factor is represented on a scale of 1 - 10 which 1 being an upset customer and 10 being a happy customer. Also, it is universally known that an interested customer would often look at the reviews before booking the listing. Thus, reviews largely drive an increase/decrease in the price of listings. We are performing this analysis to intimate the host about the most preferred contributor of a review score. 
 
## Plan for data analysis:  
 
### 1. Data Cleaning 
○ Imputed missing values with mean, medians or zeros depending on the research question and the variables in question. 
○ Dropped columns with maximum N/A values 
○ Removed $, % signs from numerical fields. 
○ Eliminated redundant values like {, / and [] from the amenities column to create distinct amenities columns. 
○ Converted important categorical variables using one hot encoding to perform model building.  
○ The Amenities Column had values in the form of a list. Example: {Oven, Car Parking, TV, Elevator}. We converted this column into a pivot table containing binary digits for the presence or absence of the amenity. (145 columns) 
○ Identified & eliminated columns with multicollinearity using MCTest. 
○ We deleted the amenity columns that had a sum of less than 1000 since the mean was 7540. 
 
### 2. Exploratory Data Analysis 
○ Depiction of the frequency distribution of the property types in New York: We observed that ‘apartment’ is the most widely available property type and ‘house’ is the second most widely available property type in NYC. 
 

<img src="image/proptypes.PNG">

Fig. 1.1 Frequency vs Property type in NYC 
 
○ Depiction of the average price per rental property in New York: Resorts are the most expensive property type followed by ‘lighthouse’ and ‘timeshare’.  
 
 
Fig. 1.2  Average Rental Price vs Property type in NYC 
 
○ Depiction of the total number of rental listings per neighborhood: Williamsburg and Bedford are the top neighborhoods that have the maximum number of listings. 
 

 
Fig. 1.3 Number of listings vs Neighbourhood in NYC 
 
○ Calculate average rental price of listings in each neighborhood: Fort Wadsworth and Woodrow are the neighborhoods with the highest average listing prices. 
 
 
Fig. 1.4 Average listing price vs neighbourhoods  in NYC 
 
○ Identify the price trends over different weeks, months and years: We observed that the number of bookings showed a trend yearly. January, February and March are the months that had a dip in the number of bookings. On the other hand, we did not see any variation in the number of bookings as compared to the days of the week. 
 
 

 
Fig. 1.5 Number of Bookings vs Weekdays and Months 
 
### 3. Feature Selection 
In this step, we used three preliminary methods to narrow down the variables for our model building process. ● Random Forest ● P-value and residual graphs ● Lasso regression with cross validation  Out of these 3 methods, the lasso regression helped us the most in directly eliminating the  variables with a zero-coefficient value. Listings’ dataset had 106 attributes. However, based on domain knowledge and redundant values we reduced the number of columns to 56. 
 
### 4.  Model Fitting and Evaluation:
• In order to evaluate which model works the best for different research questions, we used the below accuracy metrics: 
 
• Primary Question: How can the host maximize profits on its listings?  Target Variable: Listing Price ($) Description: Based on our research in the above three sub-research questions, we narrowed down our variables to the most influential and important ones namely-TV, Gym, Elevator, Family_kid_friendly, Private Entrance, Luggage_dropoff_allowed, Indoor_fireplace, Host_acceptance_rate, Host_response_rate, Host_listings_count, Beds, Maximum_nights and minimum_nights, Number_of_reviews,review_scores_ratings. We combined all these selected variables to predict our target variable price. 
 

 
## Metrics Random Forest Gradient Boosting Elastic Net 
RMSE 79.062 82.719 89.127 
• Secondary Question 1: Which amenities affect the price of listings for the host? Target Variable: Listing Price ($) Description:  We first ran the random forest method in order to get an idea of the Mean Decrease Accuracy and the Mean Gini Index values. This helped us in directly eliminating the ones that had very low values. Next, we fit a linear regression model to understand the relationship of the variables with the target variables. This gave us a list of variables that were significant based on their p-values. Using these variables, we then used the XGBoost, ElasticNet, Lasso and random forest with specific ntree and mtry values. 
 
 
• Secondary Question 2: Which factors contribute to the host being a superhost? Target Variable: is_host_superhost[0 or 1] Description: Usually, to predict binomial categorical variables, logistic regression performs well and since XGBoost is one of the best performing methods under the treebased models, we chose to use these for our research question. Also, we selected limited predictors based on our domain knowledge to fit the model. Stratified sampling was used as one the methods to handle large dataset. 
 
 
 
 
 
 
• Secondary Question 3: Which review rating scores impact the overall review rating? Target Variable: Review Rating Score Description: Different models are evaluated using RMSE and R-Squared values. We tried with linear regression at the start. However, the computation time was really high. Then, we tried out XGBoost, Elastic Net and Random Forest which showed marginal differences in terms of their RMSE values. Since, there were only 6 predictors with each predictor holding similar values, each model performed in a similar fashion. 
 
 
 
Metrics Random Forest 
Gradient Boosting 
XGBoost Elastic Net Lasso 
RMSE 99.635 101.6264 103.0013 101.9289 101.652 
Metrics Logistic Regression XGBoost 
AUC - ROC 78.95% - 
Confusion Matrix 77.11% 76.88% 
Metrics Linear Regression 
Random Forest 
XGBoost Elastic Net Lasso 
R - Squared 75.89% - 51.11% 75.17% 75.17% 
RMSE - 3.7568 5.369 3.82 3.712 
## 5.  Results: 
• Primary Question: The most important variables that can be used to determine the price of a listing in NYC are accommodates, bathrooms, bedrooms, maximum nights, minimum nights, number of reviews and beds with the random forest model performing the best. 
 
## • Secondary Questions:  
RQ1:  1. The Random forest model with the best mtry and ntree performed the best. 2. Most important amenities that an Airbnb listing could have in order to increase its price are: 
• TV 
• Gym 
• Elevator 
• Family_kid_friendly 
• Private Entrance 
• Luggage_dropoff_allowed 
• Indoor_fireplace 
 
RQ2: Logistic Regression performs the best owing to the binary nature of the target variable. We obtained the most important factors that affect the host being a super host. We obtained the below factors: 
● Host_acceptance_rate 
● Host_response_rate 
● Host_listings_count 
● Beds 
● Maximum_nights and minimum_nights 
● Number_of_reviews 
● review_scores_ratings 
Thus, in order to gain profits and be a superhost, every host should have a higher acceptance and response rating. Additionally, higher review rating score and listing count also influence being a superhost. 
 
RQ3: Lasso worked the best with higher R-Squared and lower RMSE. It was inferred that review scores for each aspect i.e. cleanliness, location, communication, check in, accuracy and value hold the same importance. Thus, the host must focus on each aspect to expect a surge in listing price. 
 
 
## 6.  Challenges encountered: 
● The dataset consisted of more than 50,000 records. Building models with this large dataset led to higher computation time and space. Hence, in order to simplify the process, we performed stratified sampling. 
● The dataset required exhaustive data cleaning and munging in order to utilise them in our models. 
● There were multiple predictors which had factors with levels of more than 100. This caused severe issues during model fitting. 
 
## 7.   Future Work: 
We observed that review scores play an important role in predicting the price of listings. However, we did not obtain a prominent review score. Thus, we would like to perform sentiment analysis on the reviews to categorize the reviews as positive and negative. These reviews would then be utilized to check an impact on the listing price. 
 
## 8. Learnings: 
● Based on research papers and domain knowledge, we expected certain predictors like host_response_rate, review_rating, amenities etc to have a prominent impact on price. After fitting models, we realized that our expectation was fulfilled. 
● Our final analysis was on the lines of what we expected. However, due to limited RAM we couldn’t test the models with all the 106 predictors. We had to reduce the predictors significantly and then fit the models.  
● There were few projects with similar dataset. Most of the projects worked on the exploratory analysis of data. Some of them made predictions with price as the target variable but used all the variables for model building rather than doing feature selection effectively. We divided our main research question into specific sub questions and then collated predictors obtained at each question. This stepwise approach gave us a broader perspective of the analysis. 
 
## References: 
● https://www.researchgate.net/publication/319178393_The_web_of_hostguest_connection s_on_Airbnb-A_social_network_perspective 
●  http://insideairbnb.com/get-the-data.html 
● http://www.sthda.com/english/articles/37-model-selection-essentials-in-r/153-penalizedregression-essentials-ridge-lasso-elastic-net/ 
● https://www.storybench.org/tidytuesday-bike-rentals-part-2-modeling-with-gradientboosting-machine/

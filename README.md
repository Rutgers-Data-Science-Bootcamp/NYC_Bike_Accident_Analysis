# NYC Bike Risk 
Rutgers University Data Science Bootcamp Final Project

### Group Members:
| Member               	| Role 	                 | Responsibilities                                                 |
|----------------------	|---------------------   | -------------                                                    |
| [Shirali Obul](https://github.com/ShiraliObul)|  Project Manager    	 |  Manage the Project flow, Technology and Communication  |
| [Moya Heinzelmann](https://github.com/Moya112)    	|  Database Lead         |  Manage the Database and Loading data Process                             |
| [Seung-Wook Noh](https://github.com/noahnohisalwaysgood)       	|  Machine Learning Lead |  Manage the Machine Learning Model and Design 	                |
| [Vanessa Cartagena](https://github.com/Vanessa-Cartagena)    	|  Dashboard Lead  	     |  Manage Tableau Dashboard, EDA and Presentation         

## Selected Topic: NYC Bike Lane Safety
For this project, we analyze bike accidents across New York City from January 2020 to October 2022 by performing comprehensive exploratory data analysis and visualizations to have insights into the bike riding risk in NYC at different times of the day, weekdays, months, and streets. Bicycle trips make up about one percent of trips in the United states. However, it accounts for over two percent of people who die in motor vehicle crashes. Therefore, in large cities such as New York, residents and travelers must know which areas are safest at a given time of day. To generate this data, we created a machine learning model that can be fed with static/updated data from the sources, followed by the ETL process, and run the model to predict whether the accident happened on the bike lane. We aim to provide information that travelers and residents can use to plan bike rides in NYC and also first-line responders to take action based on accidents, whether on bike lanes or not.

<p align="center">
<img width="800" height="600" src="https://user-images.githubusercontent.com/105958160/196839331-7d5f1036-e870-4102-8f4d-5f1e01ac739b.jpeg">
</p>

## Questions We Would Like to Answer:
- Are there more accidents on or off bike lanes?
- Which borough and/or streets have the most accidents?
- What time has the most accidents?
  - Hour
  - Day/Night
  - Weekday
  - Month
- How does different types of weather affect the frequency of bike accidents?
  - Rain
  - Snow
  - Visibility
  - Humidity
  - Clear
  - Fog
  - Cloudiness

## Resources 
### Description of data and data sources
NYC_Bike_Risk -- This database uses a multitude of factors to input details on a bike accident:
- 1st dataset contains location(longitutde, latitutde), borough, street, severity, time and date of accident over the course of 3 years from 2020-2022: 
- [NYC-Crash-Cyclist-2020](https://data.cityofnewyork.us/Public-Safety/Crash-Cyclist-2020/2kbb-e72t)
- 2nd dataset contains bike routesdata:
- [NYC-Bike-Lanes](https://data.cityofnewyork.us/Transportation/New-York-City-Bike-Routes/7vsa-caz7)
- 3rd dataset contains zipcode for boroughs which allow us to fill up missing values in 1st dataset:
- [NYC-ZIPCODE-MAP](https://bklyndesigns.com/new-york-city-zip-code/#:~:text=Manhattan%3A%2010001%2D10282,11004%2D11109%2C%2011351%2D11697)
- 4th dataset is NYC weather data using OpenWeatherMap API for the days of accident happend:
- [NYC-Weather-Data](https://openweathermap.org/city/5128581)

## Tools
#### Creating Database
- PostgreSQL
- SQLAlchemy
#### Analyzing Data
- Pandas
- Numpy
- Geopy
- Machine Learning
- Scikit-Learn
#### Dashboard
- Tableau
- Google Slides


## Data Cleaning and ETL process
We have mainly used Python pandas library in Jupyter notebook to clean the data from 3 different resources: NYC_Crash_cyclist 2020-2022, NYC_Weather_2020-2022, and NYC_Bike_Lanes.

- NYC_Crash_cyclist 2020-2022: Dropping columns without values and duplicated rows across all the columns, scraping missing zip codes with geopy based on geolocation, filled missing borough names based on zip code data; Transformed TIME of the accident to keep hours, not minutes or seconds to be able to merge with weather data where have hourly weather.

- NYC_Weather_2020-2022: hourly weather data from OpenWeatherMap for 1004 days in which accidents happened in NYC again cleaned and transformed with python pandas library in jupyter notebooks, such as DateTime format for dates and times columns, with the formatting DATE column we were able to get Name of the weekday and months, we split it into DATE, NAME_OF_WEEKDAY, MONTH columns.

- NYC_bike_lanes: First, we dropped the duplicated rows based on STREET NAME to get unique streets with bike lanes, then we split the BIKE_GEOME column into four geo parameters as it has two pairs of geolocation of the street (from street to street). This news LAT1, LAT2, LON1, and LON2 were used to get the bike lane column for the crash data by matching the accident location (lat and lon) within the two pairs of location parameters.

- Final dataset: contains NYC bike crash data, weather data, and bike lane data, which we are using for EDA and Visualization with Tableau. Also, selected features are selected for ML model training.   
-    <img width="575" alt="Screen Shot 2022-11-06 at 5 39 48 PM" src="https://user-images.githubusercontent.com/65901034/200199296-2933e180-2c98-4f1f-9e9f-f1901a37b4ca.png">


### Database: 
- Crash and weather data are loaded into the SQL database and merged with SQL code. Crash_data, weather_data, merged data, and NYC_borough_zipcode data are stored in our database as tables; We stored all the tables in our GitHub database folder as well as we have our local database from where we are going to load merged_data by using SQLite in our python code for further analysis and modeling.

- ERD:
<img width="1347" alt="QuickERD" src="https://user-images.githubusercontent.com/65901034/198191925-afcc699e-388c-451f-86cd-049d23ef2cf9.png">

- Tables post merge:
- <img width="1409" alt="Tables_post_merge" src="https://user-images.githubusercontent.com/65901034/198350933-77414e65-e573-49d1-a202-e32c5cb8bc4c.png">


### Machine Learning
#### Random Forest Classifier
- [Code is here](https://github.com/ShiraliObul/Final_Project_by_Met_A_Four/blob/main/Data_EDAtableau_ML_xSegment3/Final_project_NYC_BIKE_All_ML_xSegment3.ipynb) 


- After connecting our jupyter notebook to the database by SQLAlchemy, we performed ETL to prepare data for ML and loaded it into the database as an ML_data table. From this dynamic database, read the ML_data table and print out the header for each column to see all of the features available. 

<img width="1385" alt="Screen Shot 2022-10-30 at 10 57 30 PM" src="https://user-images.githubusercontent.com/65901034/198922259-74974974-9403-4c5c-99e6-2eac201383de.png">

- First, we checked categorical and numerical data in the dataset. Then, based on the features, we selected columns that might be important for the modeling and dropped columns that have duplicate information with others or no value on the prediction (such as DATE and COLLISION ID).

- We split our data training and testing. We used the default 75% to 25% split.

- We have tried supervised learning imblearn.ensemble library trained and compared two different ensemble classifiers. Balanced Random Forest Classifier and Easy Ensemble Classifier to predict bike lanes based on the features we selected for the model. Using both algorithms, we resampled the dataset, viewed the target classes' count, trained the ensemble classifier, calculated the balanced accuracy score, generated a confusion matrix, and developed a classification report. We chose Random forest algorithms because they can handle thousands of input variables without variable deletion, are robust to outliers and nonlinear data, and run efficiently on large datasets, as we have here. To improve prediction accuracy, we used Adaptive Boosting, called AdaBoost, which is easy to understand. In AdaBoost, a model is trained and then evaluated. After evaluating the errors of the first model, another model is introduced. This time, however, the model gives extra weight to the errors from the previous model. The purpose of this weighting is to minimize similar errors in subsequent models.

- For each algorithm, we train the model using the training data. Calculate the balanced accuracy score from sklearn.metrics. Print the confusion matrix from sklearn.metrics. Finally, generate a classification report using the imbalanced_classification_report from imbalanced-learn. Print the feature importance sorted in descending order (a most important feature to least significant) along with the feature score.

- The result of Balanced Random Forest Classifier:
![Screen Shot 2022-10-30 at 10 50 22 PM](https://user-images.githubusercontent.com/65901034/198920891-7ba4858e-2685-4c0e-87db-d87868377b06.png)

- With the Easy Ensemble AdaBoost classifier, we have improved the accuracy to 80.56% from 80.26% with the Balanced Random Forest Classifier. However, it took more time to execute, as we can see in the screenshots:

![Screen Shot 2022-10-30 at 10 51 28 PM](https://user-images.githubusercontent.com/65901034/198921048-d2073226-5af3-4153-b759-cf5a6e0a468a.png)

- Feature importance from the Balanced Random Forest Classifier with 100 iteration:
![featureimportance](https://user-images.githubusercontent.com/65901034/198863195-890e4c46-894a-4a12-b161-e7e5e7501a64.png)

##### Advantage and Limitations
* We initially wanted to have ML to predict risk of bike riding, however, due to the fact that we only have bike accident data not total bike ride. Therefore, we did ML prediction on bike lanes or not by using logistic regression Random forest classifier.  
* Random Forests Classifier provides feature importance which allowed us visualize the features with importance.
* Random Forests can be computationally much faster than Neural Network.
* We still have rooms to improve the accuracy score as Random forests are found to be biased while dealing with categorical variables because we have more categorical data than numerical data in our dataset. 

#### Limitations of Neural Network (this is our extra work to try with Neural Network deep learning).
* It requires very large amount of data in order to perform better than other techniques.
* It is extremely expensive to train due to complex data models. Moreover deep learning requires expensive GPUs and hundreds of machines. This increases cost to the users.
* Neural Network has more layers to train and test but it does not necessarily mean that will bring better results.
* Random Forests has better efficiency than Neural Network. Moreover, Neural Network takes longer time to get work done.

- [Code is here](https://github.com/ShiraliObul/Final_Project_by_Met_A_Four/blob/main/Data_EDAtableau_ML_xSegment3/ML_NEURAL_NETWORK_trial.ipynb) 


### Dashboard
[Tableau Link](https://public.tableau.com/app/profile/gerald.green7809/viz/NYCBikeRiskProject/Boroughs_1#1)

To visualize the data analysis we used Tableau. Our dashboard displays the comparison of the data by the following:
- Borough
- Bike Lane v. No Bike Lane
- Date/Time (Hour, Day/Night, Weekday, Month)
- Weather 

<img width="1440" alt="Screen Shot 2022-11-02 at 9 08 23 PM" src="https://user-images.githubusercontent.com/105958160/199637191-abc3102c-deb5-4b12-a1b7-b550277fd823.png">

## Conclusions
    - More accidents occur off the bike lanes than on bike lanes. 
    - There are more accidents when it is clear outside because more people desire to ride bikes in pleasant weather.
    - More accidents in the borough of Bronx and the least accidents in the borough of Staten Island. 
    - Rush hours tend to have more bike accidents, especially Tuesdays and Fridays.
    - Our ML model has an accuracy of over 82% in predicting bike lanes.

## Presentation
[Google Slides File Link](https://docs.google.com/presentation/d/1g-AAN2hNbT6YlkHPxmpXGSd241pEJA0cvLY3mLy1-SQ/edit#slide=id.g173790adf6e_1_0)

# NYC Bike Risk Predictor
Rutgers University Data Science Bootcamp Final Project

### Group Members:
| Member               	| Role 	                 | Responsibilities                                                 |
|----------------------	|---------------------   | -------------                                                    |
| [Shirali Obul](https://github.com/ShiraliObul)|  Project Manager    	 |  Manage the Project flow, Technology and Communication  |
| [Moya Heinzelmann](https://github.com/Moya112)    	|  Database Lead         |  Manage the Database and Loading data Process                             |
| [Seung-Wook Noh](https://github.com/noahnohisalwaysgood)       	|  Machine Learning Lead |  Manage the Machine Learning Model and Design 	                |
| [Vanessa Cartagena](https://github.com/Vanessa-Cartagena)    	|  Dashboard Lead  	     |  Manage Tableau Dashboard, EDA and Presentation         

## Selected Topic: NYC Bike Lane Safety
We want to analyse bike accidents across the New York city from Jan-2020 to October-2022 and to make a model where we can feed it with updated satatic data from the original sources, perform ETL and to predict wether the accident happended on bike lane or not. By doing exploratory data analysis and visualization, we want to have insights of the bike riding risk in NYC at different time of the day, weekdays, months, and streets. We aim to provide information which can be used by travelers and residents who are planing to ride bike in NYC and also first line responders to take action based on accidients wether on bike lane or not.

<p align="center">
<img width="800" height="600" src="https://user-images.githubusercontent.com/105958160/196839331-7d5f1036-e870-4102-8f4d-5f1e01ac739b.jpeg">
</p>

## Questions We Would Like to Answer:
- Does bike lanes reduce accident?
- What are the risk factors based on the given data?
  - Weather
  - Weekday
  - Time of day
  - Location (Borough)
- How does different types of weather affect the frequency of bike accidents?
  - Rain
  - Snow
  - Visibility
  - Humidity
  - Clear

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
We have mainly used python pandas library in Jupyter notebook to clean the data from 3 different resources: NYC_Crash_cyclist 2020-2022, NYC_Weather_2020-2022, NYC_Bike_Lanes.
- NYC_Crash_cyclist 2020-2022: Dropping columns without values and duplicated rows across all the columns, scraping missing zipcodes with geopy based on geolocation, filled missing borough names based on zipcode data; Transformed TIME of the accident to keep hours not minutes or seconds to be able to merge with weather data where have hourly weather;
- NYC_Weather_2020-2022: hourly weather data from OpenWeatherMap for 1004 days in which accidents happended in NYC again cleaned and trasformed with python pandas library in jupyter notebook, such as datetime format for dates and times columns, with the formatting DATE column we were able to get Name of the weekday and months, we split it into DATE, NAME_OF_WEEKDAY, MONTH columns;
- NYC_bike_lanes: First we dropped the dupilcated rows based on STREET NAME to get unique streets with bike lane, then we split the BIKE_GEOME column into four geo parameters as it has two pairs of geolocation of the street (from street to to street). These news LAT1, LAT2, LON1, LON2 were used to get bike lane column for the crash data by matching the accident location (lat and lon) within the two pairs of location parameters; 
- Final dataset: contains NYC bike crash data, weather data, and bike lane data all together which we are using for EDA and Visualization with Tableau. Selected features selected for ML model training.     

### Database: 
- Crash data and weather data are loaded into SQL database and merged with sql code. Crash_data, weather_data, merged data, and NYC_borough_zipcode data are stored in our database as tables; We stored all the tables in our github database folder as well as we have our local database from where we are going to load merged_data by using SQLite in our python code for further analysis and modelling.
- ERD:
- <img width="1347" alt="QuickERD" src="https://user-images.githubusercontent.com/65901034/198191925-afcc699e-388c-451f-86cd-049d23ef2cf9.png">
- Tables post merge:
- <img width="1409" alt="Tables_post_merge" src="https://user-images.githubusercontent.com/65901034/198350933-77414e65-e573-49d1-a202-e32c5cb8bc4c.png">


### Machine Learning
- After connecting our jupyter notebook to the database by SQLAlchemy, performed data cleaning to prepare for ML from Merged_data table, and save it into the database as ML_data table. Then we read the ML_data from the database, printed out the header for each column to see all of the features available. 
- <img width="1385" alt="Screen Shot 2022-10-30 at 10 57 30 PM" src="https://user-images.githubusercontent.com/65901034/198922259-74974974-9403-4c5c-99e6-2eac201383de.png">
- First we checked categorical and numerical data in the dataset, based on the features we selected columns which might be important for the modelling and dropped columns which have same information with others or no value on the prediction (such as DATE and COLIISION ID); 
- We split our data training and testing. We used the default 75% to 25% split;
- We have tried supervised learning imblearn.ensemble library, we trained and compared two different ensemble classifiers, BalancedRandomForestClassifier and EasyEnsembleClassifier, to predict Bike lane based on the features we selected for the model. Using both algorithms, we resampled the dataset, view the count of the target classes, trained the ensemble classifier, calculated the balanced accuracy score, generated a confusion matrix, and generate a classification report. We chose Random forest algorithms because it can handle thousands of input variables without variable deletion, robust to outliers and nonlinear data, and also run efficiently on large datasets as we have it here; In order to improve the accurace of prediction, we used Adaptive Boosting, called AdaBoost, is easy to understand. In AdaBoost, a model is trained then evaluated. After evaluating the errors of the first model, another model is trained. This time, however, the model gives extra weight to the errors from the previous model. The purpose of this weighting is to minimize similar errors in subsequent models.
- For each algorithm, we did following steps: train the model using the training data. Calculate the balanced accuracy score from sklearn.metrics. Print the confusion matrix from sklearn.metrics. Generate a classification report using the imbalanced_classification_report from imbalanced-learn. Print the feature importance sorted in descending order (most important feature to least important) along with the feature score.
- The result of Balanced Random Forest Classifier:
- ![Screen Shot 2022-10-30 at 10 50 22 PM](https://user-images.githubusercontent.com/65901034/198920891-7ba4858e-2685-4c0e-87db-d87868377b06.png)
- With Easy Ensemble AdaBoost classifier, we have imporved the accuracy to 80.56% from 80.26% with Balanced Random Forest Classifier, however, it took more times to exucte as we can see in the screenshots:

- ![Screen Shot 2022-10-30 at 10 51 28 PM](https://user-images.githubusercontent.com/65901034/198921048-d2073226-5af3-4153-b759-cf5a6e0a468a.png)

- Feature importance from the Balanced Random Forest Classifier with 100 iteration:
- ![featureimportance](https://user-images.githubusercontent.com/65901034/198863195-890e4c46-894a-4a12-b161-e7e5e7501a64.png)

### Dashboard
[Tableau Link](https://public.tableau.com/authoring/NYCBikeRiskProject/BikeAccidentsMap_1#2)

To visualize the data analysis we will use Tableau to create a dashboard. 
- Bike Accidents Map
- Accidents per Borough
- Accidents by time such as weekday and hour
- Weather data during time of accident

## Presentation
[Google Slides File Link](https://docs.google.com/presentation/d/1g-AAN2hNbT6YlkHPxmpXGSd241pEJA0cvLY3mLy1-SQ/edit#slide=id.g173790adf6e_1_0)

[Google Storyboard](https://app.boords.com/s/llowv4/frame)

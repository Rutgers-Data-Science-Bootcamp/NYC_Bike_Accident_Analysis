# NYC Bike Risk Predictor
Rutgers University Data Science Bootcamp Final Project

### Group Members:
| Member               	| Role 	                 | Responsibilities                                                 |
|----------------------	|---------------------   | -------------                                                    |
| [Shirali Obul](https://github.com/ShiraliObul)|  Project Manager    	 |  Manage the Project flow, Technology and Communication  |
| [Moya Heinzelmann](https://github.com/Moya112)    	|  Database Lead         |  Manage the Database and Loading data Process                             |
| [Seung-Wook Noh](https://github.com/noahnohisalwaysgood)       	|  Machine Learning Lead |  Manage the Machine Learning Model and Design 	                |
| [Vanessa Cartagena](https://github.com/Vanessa-Cartagena)    	|  Dashboard Lead  	     |  Manage Tableau Dashboard, EDA and Presentation         

## Communication Protocols
In order to keep updated on the status of each of our parts of the project, we message each other regularly through Slack and organized regular zoom meetings.

## Selected Topic: NYC Bike Lane Safety
We want to build a model that will predict the safety of bike-riding on NYC streets depending on different factors.
This information can be used by travelers and residents who are planing to ride bike in NYC.

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
- SQLite
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
<img width="1409" alt="Tables_post_merge" src="https://user-images.githubusercontent.com/65901034/198350933-77414e65-e573-49d1-a202-e32c5cb8bc4c.png">


### Machine Learning
Prepare the data for modelling: First we checked categorical and numerical data in the dataset, based on the features we selected columns which might be important for the modelling and dropped columns which have same information with others or no value on the prediction (such as DATE and COLIISION ID);
- We have tried supervised learning model with SciKitLearn random forest clustering algorithm to predict Bike lane based on the features we selected for the model. 
- We split our data training and testing, and compare two ensemble algorithms to determine which algorithm results in the best performance. Balanced Random Forest Classifier and an Easy Ensemble AdaBoost classifier. For each algorithm, we did following steps: train the model using the training data. Calculate the balanced accuracy score from sklearn.metrics. Print the confusion matrix from sklearn.metrics. Generate a classification report using the imbalanced_classification_report from imbalanced-learn. Print the feature importance sorted in descending order (most important feature to least important) along with the feature score.
The result of Balanced Random Forest Classifier:
<img width="447" alt="Screen Shot 2022-10-27 at 12 19 30 AM" src="https://user-images.githubusercontent.com/65901034/198190482-5f122792-aeae-4371-94a4-9398e7129ef9.png">
With Easy Ensemble AdaBoost classifier, we have imporved the accuracy to 80.5% from 78.3% with Balanced Random Forest Classifier:
<img width="679" alt="Screen Shot 2022-10-27 at 12 21 24 AM" src="https://user-images.githubusercontent.com/65901034/198190732-a42f715d-82b8-4764-b537-df1001a44f20.png">

- Make real meaningful insights from the model rather than just predicting Bike lane, we are trying to see the features to calssify streets/areas safer or not safe if cyclist riding on bike lane or not. We did some EDA, please have a look and give your feedback on it. 


### Dashboard
[Tableau Link](https://public.tableau.com/app/profile/gerald.green7809/viz/NYCBikeRiskPredictor/BikeAccidentsMap#1)

To visualize the data analysis we will use Tableau to create a dashboard. 
- Bike Accidents Map
- Accidents per Borough
- Accidents by time such as weekday and hour
- Weather data during time of accident

## Presentation
[Google Slides File Link](https://docs.google.com/presentation/d/1g-AAN2hNbT6YlkHPxmpXGSd241pEJA0cvLY3mLy1-SQ/edit#slide=id.g173790adf6e_1_0)

[Google Storyboard](https://app.boords.com/s/llowv4/frame)

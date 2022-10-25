# NYC Bike Risk Predictor
Rutgers University Data Analytics Bootcamp Final Project

### Group Members:
| Member               	| Role 	                 | Responsibilities                                                 |
|----------------------	|---------------------   | -------------                                                    |
| [Shirali Obul](https://github.com/ShiraliObul)|  Project Manager    	 |  Manage the Project flow, Technology, Presentation, and Communication  |
| [Moya Heinzelmann](https://github.com/Moya112)    	|  Database Lead         |  Manage the Database and ETL Process                             |
| [Seung-Wook Noh](https://github.com/noahnohisalwaysgood)       	|  Machine Learning Lead |  Manage the Machine Learning Model and Design 	                |
| [Vanessa Cartagena](https://github.com/Vanessa-Cartagena)    	|  Dashboard Lead  	     |  Manage the GitHub Repository and Presentation Dashboard         

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
- What are risk factors badsed on the given data?
  - Weather
  - Weekday
  - Time of day
  - Location (Borough)
- How does different types of weather affect the frequency of bike accident?
  - Rain
  - Snow
  - Invisibility
  - Humidity

## Resources 
### Description of data and data sources
NYC_Bike_Risk -- This database uses a multitude of factors to input details on a bike accident:
- 1st dataset contains location(longitutde, latitutde), borough, street, severity, time and date of accident over the course of 3 years from 2020-2022: 
- [NYC-Crash-Cyclist-2020]([https://data.cityofnewyork.us/Public-Safety/Crash-Cyclist-2020/2kbb-e72t/data](https://data.cityofnewyork.us/Public-Safety/Crash-Cyclist-2020/2kbb-e72t))
- 2nd dataset contains bike routesdata:
- [NYC-Bike-Routes](https://data.cityofnewyork.us/Transportation/New-York-City-Bike-Routes/7vsa-caz7)
- 3rd dataset contains zipcode for boroughs which allow us to fill up missing values in 1st dataset:
- [NYC-ZIPCODE-MAP](https://bklyndesigns.com/new-york-city-zip-code/#:~:text=Manhattan%3A%2010001%2D10282,11004%2D11109%2C%2011351%2D11697)
- 4th dataset is NYC weather data using OpenWeatherMap API for the days of accident happend:
- [NYC-Weather-Data](https://openweathermap.org/city/5128581)


## Tools
#### Creating Database
- PostgreSQL
- Amazon Web Services (AWS)
#### Analyzing Data
- Pandas
- Machine Learning
- Scikit-Learn
#### Dashboard
- Tableau
- Javascript
- HTML
- CSS

### Database: 
- For our database, we have used PostgreSQL by use of pgAdmin and we are also hosting our raw data in an AWS S3 bucket. This enables anyone with the access codes to work the project data.
![image](https://user-images.githubusercontent.com/105985796/196833444-2df3322e-5d16-4f90-b580-5caff7eca2cc.png)

### Machine Learning
Preliminary Data Processing 
- We will use supervised learning model with SciKitLearn random forest clustering algorithm to create a classifier for the safety of bike riding in New York City. Our training and testing setup will be the default 75% to 25% split. Our input will be bike lane, accident date and time, location, severity, mortality, and contributing factor vehicle. Our output labels will be safe or unsafe streets for bike-riding. 

### Dashboard
- To exploratory analysis visualize the data we will use Tableau to create a dashboard
- In addition to using a Flask template, we will also integrate D3.js (JavaScript ) for a fully functioning and interactive dashboard with . It will be hosted on Github page.



# Predicting Crime Rates in San Francisco

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/2015%20Crime%20Heatmap.gif?raw=true)



## Introduction

San Francisco's economy is continuously growing, but its wealth seems to be heavily undistributed, a situation that has driven the crime rates higher than the national average. If the number of police officers is not enough to decrease crime rates in San Francisco, what should law enforcement do to prevent high crime rates?

Law enforcement in San Francisco is facing an optimization problem where it has relatively few resources to take care of a big area. The purpose of this project is to build machine learning models that can aid law enforcement in better distributing the resources across different police department districts.

The final model implements the most predictive features from 3 different datasets with the primary objective of predicting the number of crimes for each police department district in San Francisco for any given date. 

------

## Data Overview

- **`San Francisco Crime Classification Dataset`** - This dataset contains the reported crimes that occurred in San Francisco from *01/01/2003* to *05/13/2015*. It has *878,049* records.
- **`Date & Time`** - Timestamp of the crime incident
- **`Category`** - Category of the crime incident
- **`Description`** - Detailed description of the crime incident
- **`Day of the Week`** - The day of the week
- **`Police Department District`** - Name of the Police Department District
- **`Resolution`** - How the crime incident was resolved
- **`Address`** - The approximate street address of the crime incident
- **`X`** - Longitude
- **`Y`** - Latitude

Source: https://www.kaggle.com/c/sf-crime/data

------

**`San Francisco Weather Dataset`** - Scraped weather data for every day in the *San Francisco Crime Classification Dataset*. It contains *4516* records.

- **`date`** - Date of recorded data
- **`avg_temp`** - Average temperature *(Celsius)*
- **`precipitation`** - Precipitation *(mm)*
- **`wind_speed`** - Wind speed *(km/h)*
- **`visibility`** - Visibility *(km)*
- **`moon_illumination, %`** - Moon phase and illumination *(Name of phase, %)*

Source: Scraped from https://wunderground.com/

------

**`San Francisco 49ers Game Schedule`** - Collected data for the San Francisco 49ers game between seasons *2003* and *2014*. The schedule for *Season 2015* was not included because it starts in Fall, and the *San Francisco Crime Classification Dataset* ends on *05/13/2015*. It contains 200 records.

- **`Date`** - Date of game
- **`Output`** - Whether the 49ers won or lost the game ("W", "L")
- **`Home`** - Whether the 49ers played home or away (1 = Home, 0 = Away)

Source: Collected from https://www.pro-football-reference.com/

------

## Exploratory Data Analysis

#### Crimes per Year

![crimes per year](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/crimes%20per%20year.png?raw=true)

The plot above represents the cumulative crimes per year. It is essential to mention that the San Francisco Crime Classification Dataset only included crime records up to May 13, 2015; this is why the yearly crime rate seems to suddenly improve in 2015 when in reality, we are just missing data for most of that year.

------

####  Average Daily Crimes vs. Day of Month

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/daily%20crimes%20vs%20day%20of%20month.png?raw=true)

During the exploratory data analysis, an interesting finding was that the first day tends to have crime rates xx% higher than the monthly average. The reason behind this pattern could be something as simple as this being how police in San Francisco file crime reports. Another possible cause could be that the U.S. government tends to pay Supplemental Security Income and Welfare checks on the first day of every month, potentially increasing the number of robbery targets.

------

#### Crimes by Day of the Week

![crimes by day of the week](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/crimes%20by%20day%20of%20the%20week.png?raw=true)

Crimes rates vary between days of the week, being Friday the day in which most crimes are committed on average while Sunday has the lowest crime rates of all days of the week. Please note that although the variations of crime rates between different days of the week seem to be relatively small, the day of the week plays a vital role in improving our model's accuracy.

------

#### Average Crimes per Hour

![crimes per hour](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/crimes%20per%20hour.png?raw=true)

The hour of the day also plays an important role when trying to predict crime rates. You can appreciate how 4:00 am - 5:00 am have the lowest crime rates. As we increase the hour, the crime rates increase on average until we reach 5:00 pm. After 6:00 pm, crime rates tend to decrease until 4:00 am where the cycle repeats. Although the hour of the day is relevant to the predictions of the model, I decided not to use it as a feature because the dataset would suffer a significant reduction because of many hours of the day that lack crime records. Additionally, having criminal records in a daily is consistent with the weather and football datasets.

------

#### Crimes by Category

![crimes by category](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/crimes%20by%20category.png?raw=true)

From the plot above, it was evident that larceny and theft lead as the most common crimes in San Francisco. For this project, I decided not to include the crime category as a feature in the machine learning model. However, I consider that this could be a useful future implementation that could help law enforcement make better decisions regarding the distribution of police officers based on the severity of the predicted crimes.

------

#### Crimes by Police Department District - Bar Chart

![crimes by police department](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/crimes%20by%20police%20department.png?raw=true)

It is expected for crimes not to be evenly distributed across different police department districts, especially taking into consideration that the area of these districts varies in shape and size. The plot above clearly highlights the crime distribution, and it tells us that some police department districts need more resources than others. The machine learning model uses the police department district to predict the crime rates for each zone as they vary significantly between areas.

------

#### Crimes by Police Department District - Map

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/Cumulative%20Crime%20Map%20labeled.png?raw=true)

The plot above is a visual representation of the crime count for each Police Department District in San Francisco where the darker colors denote a greater crime count.

------

#### Crimes Resolutions

![crime resolutions](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/crime%20resolutions.png?raw=true)

It is interesting to see the distribution of crime resolutions. Unfortunately, when trying to predict the crime rates, the resolutions are not available before a crime occurs. For the previously mentioned reason, I decided to exclude the resolution of the crimes as a feature for the machine learning model.

------

#### Correlation Matrix

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/correlation%20matrix.png?raw=true)



From the correlation matrix above, we can quickly determine that the average daily temperature (avg_temp) is the weather variable with the highest absolute correlation to our target variable (Daily Crimes) and potentially the only weather variable that deserves to be further explored. All the other weather variables were discarded from being features.

------

#### Mean Daily Crimes vs. Mean Daily Temperature

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/daily%20crimes%20vs%20temperature.png?raw=true)

The plot above depicts a correlation between the average daily crimes and the average daily temperature. It is evident that as temperature increases, so does crime.

------

#### Comparison of Crime Between NFL Game Day and Regular Day

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/game%20day%20vs%20regular%20day.png?raw=true)

Most of the crime reports happen to be in a day when the San Francisco 49ers don't play. This could be due to the way in which police reports are filed, maybe most police officers wait until the next day to file reports, or most people don't commit crimes during game day, or most people don't report crimes during game days.

------

#### Comparison of Crime Between NFL Home Game and Away Game

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/home%20game%20vs%20away%20game.png?raw=true)

Crime rates seem to remain the same between those days in which the 49ers play home and those days in which they play away.

------

#### Comparison of Crime Between NFL Win and Loss

![](https://github.com/fescobar96/Crime-Prediction-in-San-Francisco/blob/master/images/nfl%20win%20vs%20loss.png?raw=true)

Once again, there doesn't seem to be any difference in the crime rates if the San Francisco 49ers either lose or win a game.

------

## Machine Learning

#### Model

*XGBoost* is a gradient boosting algorithm that tends to perform better than most stand-alone ensemble algorithms, and it requires minimum data preprocessing. Unlike *random forest* that works by creating decision trees parallelly, *XGBoost* sequentially creates decision trees and tries to minimize errors through gradient descent.

------

#### Scores before Hyperparameter Optimization

| Subset   | R^2    |
| -------- | ------ |
| Training | 71.16% |
| Test     | 68.86% |

------

#### Hyperparameter Optimization

To improve our scores, it is crucial to tweak those variables that rule the model's training process: the hyperparameters. *GridSearchCV* lets us train and test a machine learning model by using different combinations of hyperparameters. From my experience, only a few hyperparameters tend to dominate a model's behavior. In the following table, you'll find the hyperparameters that I optimized and the different values that I tried for each of them.

| Hyperparameters  | List of Values                       |
| ---------------- | ------------------------------------ |
| learning_rate    | [0.05, 0.10, 0.15, 0.20, 0.25, 0.30] |
| max_depth        | [3, 4, 5, 6, 8, 10, 12, 15]          |
| min_child_weight | [1, 3, 5, 7]                         |
| gamma            | [0, 0.1, 0.2, 0.3, 0.4]              |
| colsample_bytree | [0.3, 0.4, 0.5, 0.7]                 |

------

#### Best Hyperparameters

| Hyperparameters  | Values |
| ---------------- | ------ |
| learning_rate    | 0.20   |
| max_depth        | 5      |
| min_child_weight | 3      |
| gamma            | 0.2    |
| colsample_bytree | 0.7    |

The table above shows the hyperparameters for our model after performing a grid search for 8 hours and 43 minutes. Please note that the best values were not extremes of the hyperparameters list of values, with *colsample_bytree* being the only exception. This indicates that we could improve our model by experimenting with higher values for our hyperparameter *colsample_bytree*.

------

#### Scores after Hyperparameter Optimization

| Subset   | R^2    |
| -------- | ------ |
| Training | 76.78% |
| Test     | 72.16% |

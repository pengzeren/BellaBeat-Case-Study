# CASE STUDY: Bellabeat Case Study
##### Author: Zer En Peng

##### Date: June 1, 2022

#

_The case study follows the six step data analysis process:_

###  [Ask](#1-ask)
###  [Prepare](#2-prepare)
###  [Process](#3-process)
###  [Analyze](#4-analyze)
###  [Share](#5-share)
###  [Act](#6-act)

## Summary
Bellabeat is a high-tech manufacturer of health-focused products for women. Bellabeat believes that analysing smart device fitness data could help unlock new growth opportunities for the company. This case study will focus on analysing FitBit Fitness Tracker Data to gain insight into how consumers are using their smart devices. This data analysis will help guide marketing strategy for the company, particularly for the Ivy Health Tracker by Bellabeat. The main features of the tracker include monitoring heart rate, cardiac coherence, respiratory rate, activity, and sleep. The tracker correlates menstrual cycle data, lifestyle habits and biometric readings.  I will be analyzing the FitBit data in order to present recommendations for Bellabeat’s marketing strategy. The dataset and some of the instructions will be provided by Google Data Analytics Professional Certificate, by Coursera and Google. 

## 1. Ask
 **BUSINESS TASK: Analysing current consumer’s habit on using their smart devices of an existing competitor(FitBit) to gain insight and guide marketing strategy for Bellabeat.**

Questions to address
1.	What are some trends in smart device usage?
2.	How could these trends apply to BellaBeat customers?
3.	How could these trends help influence BellaBeat marketing strategy?

Deliverables: 
1. A clear summary of the business task
2. A description of all data sources used
3. Documentation of any cleaning or manipulation of data
4. A summary of your analysis
5. Supporting visualizations and key findings
6. Your top high-level content recommendations based on your analysis

Key stakeholders:
•	Urška Sršen: Bellabeat’s cofounder and Chief Creative Officer  
•	Sando Mur: Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
•	Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy. 


## 2. Prepare 

The data for processing and analysing will come form Fitbit Fitness Tracker Data on Kaggle. This dataset contains personal fitness tracker from thirty Fitbit users. The data include minute-level output for physical activity, heart rate, and sleep monitoring. 

ROCCC framework:

- Reliable: The dataset is secondary data collected via survey by Amazon Mechanical Turk. Little information was given about the collection process and hence not very reliable.
- Original: The data is not original, it is sourced from a public dataset
- Comprehensive: Information about the users is also limited, information such as: gender, age, employment status, location, and lifestyle were left out. Not very comprehensive with only 30 samples.
- Current: The data is from 2016 which might be a bit outdated considering smart device user habits.
- Cited: there was no information on this dataset being cited

Limitations:

-Considering the small data pool, limiting, and lack of credibility in the data, it can only give us a broad generalization of how users utilize their smart tracker at that time. 


## 3. Process


Installing and loading packages and libraries

```
install.packages("tidyverse")

library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2) #visualize data
library(tidyr) #clean data
```

Prepare the data

```
dailyActivity_merged <- read.csv("C:/Users/pengz/Downloads/archive/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
dailyCalories_merged <- read.csv("C:/Users/pengz/Downloads/archive/Fitabase Data 4.12.16-5.12.16/dailyCalories_merged.csv")
dailyIntensities_merged <- read.csv("C:/Users/pengz/Downloads/archive/Fitabase Data 4.12.16-5.12.16/dailyIntensities_merged.csv")
sleepDay_merged <- read.csv("C:/Users/pengz/Downloads/archive/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
weightLogInfo_merged <- read.csv("C:/Users/pengz/Downloads/archive/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")

#renaming the data to simpler terms activity
calories <- dailyCalories_merged
intensity <- dailyIntensities_merged
sleep <- sleepDay_merged
weight <- weightLogInfo_merged
activity <- dailyActivity_merged

#remove duplicates
activity <- activity[!duplicated(activity),]

#checking duplicated rows after cleaning
sum(duplicated(activity))
sum(duplicated(intensity))

#check missing value from each column 
sum(is.na(weight)) 

#display missing values 
is.na((weight)) 

#exclude weight column 
weight %>% filter(!is.na(Fat)) 

#removing "Fat" column from the analysis 
weight2 <- subset(weight,select = -Fat) 

#checking 
sum(is.na(weight2))

#Seperate data and time to two columns 
sleep <-sleep %>% + separate(SleepDay,c("date","time")," ") 
weight<-weight %>% separate(Date,c("date","time")," ") 
activity$date <- as.Date(activity$ActivityDate, format="%m/%d/%y") 
calories$ActivityDay <- as.Date.character(calories$ActivityDay, format = "%m/%d/%y") 
intensity$time <- as.Date(intensity$ActivityDay, format ="%m/%d/%y") 
sleep$date <- as.Date.character(sleep$date, format = "%m/%d/%y") 
weight$date <- as.Date.character(weight$date, format = "%m/%d/%y")

#merging data 
sleep_activity <- merge(sleep, activity,by=c("Id", "date")) head(sleep_activity)
```

Cleaning the data to prepare for analysis in 4. Analyze!

## 4. Analyze

-  [Summary](#summary)
-  [Total Steps](#total-steps)
-  [Sleep](#sleep)


### Summary:

Looking at the summary of the datasets.

```
#exploring and summarizing data 
n_distinct(activity$Id) 
n_distinct(calories$Id) 
n_distinct(intensities$Id) 
n_distinct(sleep$Id) 
n_distinct(weight2$Id) 
nrow(activity) 
nrow(sleep)

head(activity) 
summary(activity) 
str(activity) 
summary(calories) 
summary(sleep) 
summary(weight)
```

### Total Steps:
[Back to Analyze](#4-analyze)

Average daily steps of 7638 which is fair, but still less than the widely recommended 10,000 steps a day.

```
head(activity) 
summary(activity) 
str(activity) 

```


### Sleep:
[Back to Analyze](#4-analyze)

The average sleep a user gets is around 419.5 minutes which is around 7 hours. Also, the users are averaging around 40 minutes more on bed than asleep which suggest that they might have trouble falling asleep at night. Some users have average minimum sleep of an hour, which is unhealthy.

```
summary(sleep)
head(sleep)
str(sleep)
```

Next step is sharing which will be done through visualizing the data using ggplot.

## 5. Share 


First, comparing the TotalSteps to Calories, we can see from the scatterplot that it is positively correlated.

```
ggplot(data=activity, aes(x=TotalSteps, y=Calories)) + geom_point() + geom_smooth() + labs(title="Total Steps vs. Calories")
```
![Total_steps_vs_Calories](https://user-images.githubusercontent.com/20769378/171519425-8c00f8cf-9652-405a-8e47-1d8df881763b.png)


This is fairly obvious as walking more meaning more calories burned, but from a marketing standpoint BellaBeat can use this information to encourage their customers to walk more daily if they are overweight. Which we can see from the weight data, the users average BMI falls in the overweight range of 25.0-29.9.


When comparing weekday and weekend activity for the users, first I created a column with the days of the week ordered. Then plotted it against the total steps walked on average.

```
activity$weekday <- weekdays(activity$date) average_steps <- aggregate(TotalSteps ~ weekday, activity, mean) average_steps$weekday <- ordered(average_steps$weekday, levels=c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")) 

ggplot(average_steps, aes(x=weekday, y=TotalSteps)) + geom_bar(stat="identity", fill="yellow") + geom_text(aes(label=round(TotalSteps, digits = 0)), color="black", size=5.5) + labs(title = "Total Steps Walked on Average", x="Day", y="Number of Steps") + theme(plot.title = element_text(hjust = 0.5, size=20), axis.text=element_text(size=10), axis.title=element_text(size=14), axis.title.y=element_text(margin = margin(t = 0, r = 15, b = 0, l = 0)), axis.title.x=element_text(margin = margin(t = 15, r = 0, b = 0, l = 0)))

```
![Total_steps_walked_on_average](https://user-images.githubusercontent.com/20769378/171519296-de59cce0-58d7-4c48-86f0-a0c049ddfb06.png)


From this, the bar chart shows that the users are walking slightly less steps daily than recommended. This means that it is likely due to sedentary habits rather than work since it happens also during the weekend.


```
ggplot(data=sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + geom_point()+ labs(title="Total Minutes Asleep vs. Total Time in Bed")
```

Next, the total minutes asleep versus total time in bed will be plotted. The relationship between these two information is also showing postive correlation.

![Total_minutes_asleep_vs_total_time_in_bed](https://user-images.githubusercontent.com/20769378/171519457-b6ff4bff-823d-4346-9662-93435093ca43.png)

The outliers that is on the upper part of the trendline would show that some people have greater difficulty in falling alseep. They require more time in bed than other users to get the same amount of sleep.

In order to improve sleep time, I decided to conduct an analysis on another sleep data which is the sedentary minutes vs total minutes asleep.

```
ggplot(data=sleep_activity, aes(x=TotalMinutesAsleep, y=SedentaryMinutes)) + geom_point(color='red') + geom_smooth() + labs(title="Minutes Asleep vs. Sedentary Minutes")
```
![Minutes_asleep_vs_Sedentary_minutes](https://user-images.githubusercontent.com/20769378/171519467-c01df7ab-6145-487e-bfda-7fa30d6ddb64.png)

From the scatterplot we see that it is negatively correlated. This means that if BellaBeat users want to get more sleep time they should decrease their sedentary minutes.


## 6. Act

Insights and marketing recommendations based on the analysis:

- Walking and staying active helps user burn more calories. BellaBeat could use this information to include an educational health campaign on their app to encourage their customers to exercise. 
- The campaign could also be taken further through gamification, include a point reward system, which can be used to redeem discounts on other products that not just helps the users but also increases engagement with the app and might boost sales of other products.
- The BellaBeat app that monitors the users BMI should include customised workout plans which could help overweight users.
- Bellabeat should introduce referral system that can help push the product to more people, this might be able to boost data collection so that the analysis will be more accurate and the recommendations could be targeted to a more specific target group.
- The application can also include custom notification for users with arrythmia or other cardiovascular conditions, this could be life saving and could appeal to customers with chronic heart diseases who needs to track their cardiovascular data daily.


##End

This is my capstone project with R. Would appreciate any comments and recommendations.

Thank you for your attention!


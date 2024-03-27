# Bellabeat_daily_activity_analysis
This analysis is focused on exploring Fitbit Fitness Tracker data to draw insights for business growth opportunities.

## TABLE OF CONTENT
[DELIVERABLES](#deliverables)  
[QUESTIONS TO ANSWER](#questions-to-answer)  
[DATA SOURCE](#data-source)  
[TOOLS](#tools)  
[DATA CLEANING/ PREPARATION](#data-cleaning-preparation)  
[EXPLORATORY DATA ANALYSIS](#exploratory-data-analysis)  
[DATA ANALYSIS](#data-analysis)  
[VISUALIZATION](#visualization)  
[KEY FINDINGS](#key-findings)  
[RECOMMENDATION](recommendation)  
[LIMITATIONS](limitations)  
### DELIVERABLES
1.	A clear summary of the business task 
2.	A description of all data sources used. 
3.	Documentation of any cleaning or manipulation of data 
4.	A summary of your analysis 
5.	Supporting visualizations and key findings 
6.	Your top high-level content recommendations based on your analysis.

### QUESTIONS TO ANSWER
1.	What are some trends in smart device usage? 
2.	How could these trends apply to Bellabeat customers? 
3.	How could these trends help influence Bellabeat marketing strategy?

### DATA SOURCE
- Data source: Kaggle/ https://zenodo.org/record/53894#.X9oeh3Uzaao  
- Author: Arashnic Morbius.  
- About Data: BellaBeat Wellness (hourly_calories_merged, hourly_steps_merged, hourly_intensities_merged, daily_activity_merged)

### TOOLS
- Excel: Data cleaning  
- SQL: analysis  
- Tableau: creating dashboard

### DATA CLEANING/ PREPARATION
1.	I downloaded and opened the dataset on Microsoft excel.
2.	Packed all data from different workbooks into one workbook (multiple sheets)
3.	The date format was not in the right format. it was hashed (#). i changed it into the right Format. 
4.	Formatted measurements and put them in two decimal places which are consistent and easy to read.
5.	Changed the column case to all-lower case and separated words with underscore, to maintain consistency and avoid errors during analysis.
6.	Checked and deleted rows with **null** customer id (primary key) values.
7.	Named my file using the file naming convention, applying ISO 8601 standard (authorslastname_date_contentname). This is to maintain high level organization in file storage for easy access.
8.	Uploaded the dataset on Bigquery to be stored and prepared for analysis.
9.	After i uploaded to Bigquery, I noticed that the **time** column wasn’t uploaded in a time format even if I assigned the format on excel. This was because It was stored in **hm** format. I changed the format to **hms** and reuploaded it, and it worked.

## EXPLORATORY DATA ANALYSIS 
1.  What is the Average steps for each user per day and Average calories burned.
2.  What is the Average intensity per day) for individual user 
4.	Average steps, calories, intensities for each for each user.
7.	Which hour has the highest a calorie burned.
8.	Which hour has the highest steps walked.
9.	Which hour of the day are the users most active?

## DATA ANALYSIS
At first i wanted to know the sums and averages of the fields per day in the daily_activity dataset.  
this includes calories burned, distance and steps walked. I wrote the query below to get that.
```SQL
#this code is to find the average of the fields
SELECT
activity_date,
round(avg(calories)) as avg_calories,
round(avg(total_distance)) as avg_distance, 
round(avg(total_steps)) as avg_steps
FROM `adesanmi-bq-project.bellabeat_fitbit.daily_activity`
group by activity_date
order by activity_date
LIMIT 1000
```
```SQL
#this code is to find the sum of the fields
SELECT 
activity_date,
round(sum(calories)) as sum_calories,
round(sum(total_distance)) as sum_distance, 
round(sum(total_steps)) as sum_steps
FROM `adesanmi-bq-project.bellabeat_fitbit.daily_activity`
group by activity_date
order by activity_date
LIMIT 1000
```
i added the round function because averages often end up in decimal places.  

I also queried for the user who burned the highest amount of caories. The marketing team could send out bonuses to the top 3 most active users.  
This would boost the morale of the users, create a sense of competition among ethuiastic users and therefore cause more engagement among users
```SQL
#
SELECT 
id,
round(sum(calories)) as sum_calories,
round(sum(total_distance)) as sum_distance, 
round(sum(total_steps)) as sum_steps
FROM `adesanmi-bq-project.bellabeat_fitbit.daily_activity`
group by id 
order by sum_calories desc
LIMIT 1000
```
Below is the most active time of the most active user. 
```SQL
SELECT 
id, 
round(avg(calories)) as avg_calories, 
activity_hour 
FROM `adesanmi-bq-project.bellabeat_fitbit.hourly_calories` 
where id = 8378563200 
group by id, 
activity_hour 
order by avg_calories desc
LIMIT 1000
```
I also queried to get the hour with the highest calories burned, distance covered, and total steps.
I wanted to get some data which could be used when planning a marketing strategy for running commercials
```SQL
SELECT 
activity_hour,
round(avg(calories)) as avg_cal
FROM `adesanmi-bq-project.bellabeat_fitbit.hourly_calories`
group by activity_hour
order by avg_cal desc
```
```SQL
SELECT 
activity_hour2,
round(avg(total_intensity)) as avg_int
FROM `adesanmi-bq-project.bellabeat_fitbit.hourly_intensities`
group by activity_hour2
order by avg_int desc
```
```SQL
SELECT 
activity_hour,
round(avg(step_total)) as avg_steps
FROM `adesanmi-bq-project.bellabeat_fitbit.hourly_steps`
group by activity_hour
order by avg_steps desc
```

## VISUALIZATION
1.	a chart showing the trendline for calories and steps for each user over the last one month.
2.	Average steps, calories, intensities for each day, regardless of user
3.	(Average steps for each user per day. Average calories burned. Average intensity per day) for individual user 
4.	A dashboard on tableau
5.	Plots on R
6.	A trend of calories burned per hour, over the month.
   
## KEY FINDINGS 
1.	The steps, distance covered, and calories burned are all positively proportional to each other.
2.	The high correlation of these analysis could mean that a considerablenumber of users are dependent on physical exercise to burn calories.
3.	There was a decline in the average amount of calories burned throughout the time frame of data collection. 
4.	Users are most active during the evening at around 5-7 pm. This knowledge could be used in choosing the right activity to be done for marketing commercial.

## RECOMMENDATION
1. The most active_hour finding could be used when planning a marketing strategy for running commercials. study show that targetting marketng campaign on specific activities that are most caried out by users are more likely to cause more engagement. the most prevailent activities that are most carried out at 5-7pm are: walking back from work, gym, evening walk etc.
2. 

## LIMITATIONS
- available data does not span over wide span of time so it is difficult to identify trends.
- 

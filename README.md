# Google Data Analytics Capstone Project: Cyclistic Case Study

### Scenario
_This is copied from the capstone project._

> You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director
of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore,
your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights,
your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives
must approve your recommendations, so they must be backed up with compelling data insights and professional data
visualizations.

### Ask
The marketing analytics team at Cyclistic has determined that annual members are more profitable for the company than casual riders. I have been tasked with answering the question: **How do annual members and casual riders use Cyclistic bikes differently**? My insights will help stakeholders determine how we can turn casual riders into annual members.

### Prepare
I have been given 12 databases with all of the customer and bike usage data for the year 2021. The data provided is reliable and free from bias. It has been provided in a CSV format and I have been given free rein to choose the tools I desire to complete the task set out in the **Ask** section. I have stored the data on a secure hard drive with encryption to make sure it's secure.

The team that collected the data at Cyclistic has outlined some information about the data:
* It contains each trip that took place during 2021.
* Customer's personal information has been removed for privacy reasons.
* The term 'docked bikes' should be replaced with 'classic bikes'.
* Classic bikes must be docked at a station whereas Electric bikes can be parked anywhere.

To prepare the data I downloaded 12 months of data and merged them in RStudio. The first thing I did before starting was to install some dependencies that will help me achieve future tasks in R.

```r
library(tidyverse)
library(lubridate)
library(scales)
library(ggplot2)
library(dplyr)
library(hms)
```
**Console**
```
── Attaching core tidyverse packages ──────────────── tidyverse 2.0.0 ──
✔ dplyr     1.1.2     ✔ purrr     1.0.2
✔ forcats   1.0.0     ✔ stringr   1.5.0
✔ ggplot2   3.4.3     ✔ tibble    3.2.1
✔ lubridate 1.9.2     ✔ tidyr     1.3.0
── Conflicts ────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
ℹ Use the conflicted package to force all conflicts to become errors
```

To make my data readable by RStudio I created 12 data frames using csv.read().

```r
df1 <- read.csv("D:/Cyclistic_Capstone/Data/202104-divvy-tripdata.csv")
df2 <- read.csv("D:/Cyclistic_Capstone/Data/202105-divvy-tripdata.csv")
df3 <- read.csv("D:/Cyclistic_Capstone/Data/202106-divvy-tripdata.csv")
df4 <- read.csv("D:/Cyclistic_Capstone/Data/202107-divvy-tripdata.csv")
df5 <- read.csv("D:/Cyclistic_Capstone/Data/202108-divvy-tripdata.csv")
df6 <- read.csv("D:/Cyclistic_Capstone/Data/202109-divvy-tripdata.csv")
df7 <- read.csv("D:/Cyclistic_Capstone/Data/202110-divvy-tripdata.csv")
df8 <- read.csv("D:/Cyclistic_Capstone/Data/202111-divvy-tripdata.csv")
df9 <- read.csv("D:/Cyclistic_Capstone/Data/202112-divvy-tripdata.csv")
df10 <- read.csv("D:/Cyclistic_Capstone/Data/202201-divvy-tripdata.csv")
df11 <- read.csv("D:/Cyclistic_Capstone/Data/202202-divvy-tripdata.csv")
df12 <- read.csv("D:/Cyclistic_Capstone/Data/202203-divvy-tripdata.csv")
```

I then calculated the time that each ride took, I used the HH:MM:SS format for the time. Also, this process couldn't be done after the data frames were bound due to crashing.

```r
df1$ride_length <- as_hms(difftime(df1$ended_at, df1$started_at, units = "secs"))
df2$ride_length <- as_hms(difftime(df2$ended_at, df2$started_at, units = "secs"))
df3$ride_length <- as_hms(difftime(df3$ended_at, df3$started_at, units = "secs"))
df4$ride_length <- as_hms(difftime(df4$ended_at, df4$started_at, units = "secs"))
df5$ride_length <- as_hms(difftime(df5$ended_at, df5$started_at, units = "secs"))
df6$ride_length <- as_hms(difftime(df6$ended_at, df6$started_at, units = "secs"))
df7$ride_length <- as_hms(difftime(df7$ended_at, df7$started_at, units = "secs"))
df8$ride_length <- as_hms(difftime(df8$ended_at, df8$started_at, units = "secs"))
df9$ride_length <- as_hms(difftime(df9$ended_at, df9$started_at, units = "secs"))
df10$ride_length <- as_hms(difftime(df10$ended_at, df10$started_at, units = "secs"))
df11$ride_length <- as_hms(difftime(df11$ended_at, df11$started_at, units = "secs"))
df12$ride_length <- as_hms(difftime(df12$ended_at, df12$started_at, units = "secs"))
```

I combined the data frames using rbind().
```r
bike_rides <- rbind(df1, df2, df3, df4, df5, df6, df7, df8, df9, df10, df11, df12)
```

To make sure the process has worked I used the dim() function to get the size of bike_rides.
```r
dim(bike_rides)
```
**Output**
```
[1] 5723532      13
```

I have decided to separate the date times in the started_at and ended_at columns to get the hour of the day that a bike was used.

```r
# parse time
bike_rides$start_hour <- lubridate::hour(bike_rides$started_at)
bike_rides$end_hour <- lubridate::hour(bike_rides$ended_at)

bike_rides$start_date <- as.Date(bike_rides$started_at)
bike_rides$end_date <- as.Date(bike_rides$ended_at)
```

I also added a new column called 'day_of_week' to the data frame to get the day of the week that the bike journey started. (Monday - Sunday)

```r
# gets the day of the week from start_date
bike_rides$day_of_week <- weekdays(bike_rides$start_date)
```

After running the code I decided to check it worked by creating a bar chart that shows the number of rides started by hour. As this was a test I decided not to add colours.

```r
bike_rides %>% count(start_hour, sort = T) %>% 
  ggplot(aes(x = start_hour, y = n)) + geom_bar(stat = "identity") +
  scale_y_continuous(label = comma) +
  labs(title = "Bike rides started by hour", 
       x = "Start Hour", y = "Ride Count")
```

![image](https://github.com/flewis267/GoogleDataAnalytics-Cyclist-Casestudy/assets/81341510/60c83a67-f1fd-4fb8-9b07-aeac5be74e31)

Finally, I decided to export the bike_rides data frame to a CSV file so it could be loaded into PostgreSQL. I also had to remove the NA values and replace them with nulls. 

```r
# replaces NA values with nulls
bike_rides[is.na(bike_rides)] <- ""

# creates a CSV file
write.table(bike_rides, "C:\\Users\\felix\\Downloads\\bike_rides.csv", sep = ",", row.names = FALSE, col.names = FALSE, quote = FALSE)
```

To check this has worked I wrote some SQL code. I didn't import all columns as I was just testing PostgreSQL to see if it would crash with 5.7 million records.

```sql
SELECT * FROM bike_rides
LIMIT 10
```
**Output**
```
"6C992BD37A98A63F"	"classic_bike"	"2021-04-12 18:25:36"	"2021-04-12 18:56:55"	"State St & Pearson St"	"TA1307000061"	"Southport Ave & Waveland Ave"	"13235"	41.897448	-87.628722	41.94815	-87.66394	"member"	"00:31:19"	18	18	"2021-04-12"	"2021-04-12"	"Monday"
"1887262AD101C604"	"classic_bike"	"2021-04-17 09:17:42"	"2021-04-17 09:42:48"	"Honore St & Division St"	"TA1305000034"	"Southport Ave & Waveland Ave"	"13235"	41.903119	-87.673935	41.94815	-87.66394	"member"	"00:25:06"	9	9	"2021-04-17"	"2021-04-17"	"Saturday"
"097E76F3651B1AC1"	"classic_bike"	"2021-04-25 18:43:18"	"2021-04-25 18:43:59"	"Clinton St & Polk St"	"15542"	"Clinton St & Polk St"	"15542"	41.87146651779	-87.6409491327	41.87146651779	-87.6409491327	"casual"	"00:00:41"	18	18	"2021-04-25"	"2021-04-25"	"Sunday"
"53C38EB01E6FA5C4"	"classic_bike"	"2021-04-03 16:28:21"	"2021-04-03 16:29:47"	"Ashland Ave & 63rd St"	"16948"	"Ashland Ave & 63rd St"	"16948"	41.779374	-87.664843	41.779374	-87.664843	"casual"	"00:01:26"	16	16	"2021-04-03"	"2021-04-03"	"Saturday"
"D53AC014EFD6E2BA"	"electric_bike"	"2021-04-06 16:35:06"	"2021-04-06 17:00:56"	"Dorchester Ave & 49th St"	"KA1503000069"	"Dorchester Ave & 49th St"	"KA1503000069"	41.8058323333333	-87.5924785	41.8058028333333	-87.5926615	"casual"	"00:25:50"	16	17	"2021-04-06"	"2021-04-06"	"Tuesday"
"6E2F7CA1FA9E0AFB"	"classic_bike"	"2021-04-12 15:22:54"	"2021-04-12 16:15:48"	"Ashland Ave & 63rd St"	"16948"	"Ashland Ave & 63rd St"	"16948"	41.779374	-87.664843	41.779374	-87.664843	"casual"	"00:52:54"	15	16	"2021-04-12"	"2021-04-12"	"Monday"
"04218447AAC80BD1"	"classic_bike"	"2021-04-24 15:04:55"	"2021-04-24 15:06:16"	"Dorchester Ave & 49th St"	"KA1503000069"	"Dorchester Ave & 49th St"	"KA1503000069"	41.805772	-87.592464	41.805772	-87.592464	"casual"	"00:01:21"	15	15	"2021-04-24"	"2021-04-24"	"Saturday"
"B45BBE0734834247"	"electric_bike"	"2021-04-03 18:03:40"	"2021-04-03 19:13:22"	"Loomis Blvd & 84th St"	"20121"	"Loomis Blvd & 84th St"	"20121"	41.7415615	-87.6584185	41.7415958333333	-87.6584198333333	"casual"	"01:09:42"	18	19	"2021-04-03"	"2021-04-03"	"Saturday"
"70DEC4E102F00DC2"	"electric_bike"	"2021-04-24 18:01:29"	"2021-04-24 19:39:00"	"Halsted St & 69th St"	"15597"	"Halsted St & 69th St"	"15597"	41.7690143333333	-87.6446325	41.7690086666667	-87.6445838333333	"casual"	"01:37:31"	18	19	"2021-04-24"	"2021-04-24"	"Saturday"
"B33484FA16A9A0FE"	"classic_bike"	"2021-04-27 18:31:52"	"2021-04-27 19:17:20"	"Dorchester Ave & 49th St"	"KA1503000069"	"Dorchester Ave & 49th St"	"KA1503000069"	41.805772	-87.592464	41.805772	-87.592464	"casual"	"00:45:28"	18	19	"2021-04-27"	"2021-04-27"	"Tuesday"
```


### Process
Now I have prepared the data, I can process it. To do this I will continue to use PostgreSQL. I decided on PostgreSQL due to my familiarity with it and its good reputation in the SQL industry. PostgreSQL is used by many large companies such as Netflix, Uber, Twitch, Instagram, Spotify, Reddit, and Skype. To keep data integrity I set out to ensure the trustworthiness and reliability of the data. I achieved this by checking for human error after each change to the database and keeping a changelog to revert any errors I have made. However, because of the documentation process I used (this Github readme file) the changelog is void but wouldn't be in production code.

The columns of my database:
* ride_id: The primary key, I checked in r for duplicates before importing into PostgreSQL. Each string is 16 characters long. I will not need to do any data cleaning for this column.
* rideable_type: The data consists of 2 different string values (formally 3), 'classic_bike', 'electic_bike', and formally 'docked_bike'.
* started_at/ended_at: The start and end date of the ride, if one of these doesn't exist then the row should be deleted.
* start_station_name/end_station_name: The name of the start station and end station. It should be noted that only classic bikes have a start station and end station. Electric bikes can be parked anywhere.
* start_station_id/end_station_id: The id for the start station and end station.
* start_lat/end_lat & start_lng/end_lng: The start and end longitudes and latitudes for each bike ride.
* member_casual: The data can only be 2 string values. Either 'casual' or 'member'. A casual user pays per ride, while a member pays yearly. The stakeholders want to make more casual riders become members.
* ride_length: How long the ride went on for in the format HH:MM:SS.
* start_hour/end_hour: The start and end hour on a 24-hour clock. This allows me to get a general idea of what time of the day bikes are rented.
* start_date/end_date: The start and end date that the bikes are rented.
* day_of_week: The day of the week in text from 'Monday' to 'Sunday'.

##
**Cleaning the data**

Now I have all my data filtered, sorted, and modified for analysis the last thing I need to do is clean the data. This entails removing rows that have invalid data that could potentially cause unintended bias in the data. An example of this would be if a classic bike doesn't have a start or end location.

*I was on a laptop when I wrote this SQL code and it kept crashing so I decided to remove the rows from the database, this worked to stop the crashing although it's not the most effective way to write the code.*

Here are the following cleaning steps I have taken:
* Search the rideable_type and replace 'docked_bike' with 'classic_bike' as the docked bike is a legacy name for the classic bike. Then check if there are only 2 distinct values in the column. 
```sql
UPDATE bike_rides
SET rideable_type = 'classic_bike'
WHERE rideable_type = 'docked_bike'
```
```sql
/* lists all of the distinct instances of rideable_type */
SELECT DISTINCT rideable_type FROM bike_rides
```

**Output**

![image](https://github.com/flewis267/GoogleDataAnalytics-Cyclist-Casestudy/assets/81341510/47329386-3ff5-4248-b25d-482764d09280)

* Make sure there are only 2 values for the member_casual column, 'member' or 'casual'.
```sql
/* Lists all of the distinct instances of member_casual */
SELECT DISTINCT member_casual FROM bike_rides
```

**Output**

![image](https://github.com/flewis267/GoogleDataAnalytics-Cyclist-Casestudy/assets/81341510/d2f2025f-068f-4278-ac26-a51f64e83951)

* Delete any rows where a classic bike has no start and/or end station.
```sql
DELETE FROM bike_rides
WHERE rideable_type = 'classic_bike' AND
	(start_station_name IS NULL OR
	end_station_name IS NULL)
```

**Output**

```
DELETE 9167

Query returned successfully in 1 secs 604 msec.
```

* Delete any rows where the start/end longitude or latitude are null.
```sql
DELETE FROM bike_rides
WHERE start_lat IS NULL OR
	start_lng IS NULL OR
	end_lat IS NULL OR
	end_lng IS NULL;
```

* Remove trips under 1 minute and above 1 day.
```sql
DELETE FROM bike_rides
WHERE ride_length > interval '24:00:00' OR 
	ride_length < interval '00:01:00'
```

**Output**
```
DELETE 368098

Query returned successfully in 19 secs 151 msec.
```

### Analyse
Now I have done the preparation, processing, and cleaning of my data I can begin to analyse my data in PostgreSQL. In the process stage, I made sure that the data was formatted correctly so it could be used and manipulated effectively in the analysis stage. The stakeholders have given me some suggestions for data they want from my database. This includes the mean and max ride_length as well as the mode of day_of_week. I have added some of my own queries that I thought would be useful when sharing my data in the next stage.

Mean of ride_length:
```sql
SELECT AVG(ride_length) FROM bike_rides
```
**Output**
```
00:19:35.436667
```
##
Max of ride_length:
```sql
SELECT MAX(ride_length) FROM bike_rides
```
**Output**

Note: I decided to only use rides at or under 24 hours and above 1 minute.
```
24:00:00
```
##
Mode of day_of_week:
```sql
SELECT day_of_week, COUNT(day_of_week) AS Count 
FROM bike_rides
GROUP BY day_of_week
```
**Output**
```
"Friday"	776200
"Monday"	676882
"Saturday"	930050
"Sunday"	816924
"Thursday"	712871
"Tuesday"	709944
"Wednesday"	723396
```
Saturday is the mode with 930050 rides.

##
Calculate the average ride_length for members and casual riders.
```sql
SELECT member_casual, AVG(ride_length) AS Count 
FROM bike_rides
GROUP BY member_casual
```
**Output**
```
"casual"	"00:27:00.774746"
"member"	"00:13:27.890855"
```

##
Calculate the average ride_length for users by day_of_week.
```sql
SELECT day_of_week, member_casual, 
	AVG(ride_length) AS Avg_Ride_Length
FROM bike_rides
GROUP BY day_of_week, member_casual
```
**Output**
```
"Friday"	"casual"	"00:25:11.294569"
"Friday"	"member"	"00:13:18.566936"
"Monday"	"casual"	"00:27:17.150024"
"Monday"	"member"	"00:12:56.638372"
"Saturday"	"casual"	"00:29:16.245624"
"Saturday"	"member"	"00:15:02.898144"
"Sunday"	"casual"	"00:31:09.271476"
"Sunday"	"member"	"00:15:23.264523"
"Thursday"	"casual"	"00:23:30.021489"
"Thursday"	"member"	"00:12:41.556728"
"Tuesday"	"casual"	"00:24:30.00403"
"Tuesday"	"member"	"00:12:36.320822"
"Wednesday"	"casual"	"00:23:41.512825"
"Wednesday"	"member"	"00:12:44.285322"
```

##
To download my PostgreSQL queries as a CSV file I used variations of:
```sql
COPY (SELECT * FROM bike_rides) TO 
	'D:\bike_rides_cleaned.csv' WITH csv
```

I am surprised by the fact that casual users took longer average rides than members. The average length of a ride for a casual user was 27 minutes exactly and 13 minutes and 27 seconds for members. I think this could either be down to members using the bikes more frequently for shorter trips as it's cheaper whilst a casual user might take it out for longer but less frequently. I also noticed an interesting trend when calculating the mode of day_of_week. All the days of the week have a similar total amount of rides, with the maximum difference between days being 253168 from Saturday to Monday.

These insights will be crucial in answering the business question **How do annual members and casual riders use Cyclistic bikes differently** as it shows clearly how the different categories of riders use the Cyclistic service. The most important thing to pass on to the stakeholders is the fact that for rides under 24 hours, casual riders spend double the time per ride than members.

### Share
I have collected data and analysed it in Tableau with the intention of answering the question of how annual members and casual riders use Cyclistic bikes differently. I believe with the data I have collected, I can start to answer that question. The data tells a story that proves members use the bikes more often than regular casual users. Most riders overall start their trips between 3 PM and 4 PM. Below I have some data that shows my methodology in answering the question above.


For my first insight I examined the types of bikes people were using and what proportion of bike riders were casual riders or members.
![rideable_type](https://github.com/flewis267/GoogleDataAnalytics-Cyclist-Casestudy/assets/81341510/29d7dbb9-3f59-4226-b3c0-cc57fba1c868)
Members makeup 55.44% of classic bike users and 53.67% of electric bike riders and casual riders make up the rest. It should be noted, however, that both members and casual users prefer classic bikes over electric bikes. The difference in members vs. casual riders on classic and electric bikes is so minimal that it does not affect the results in any meaningful way.

##
Another idea I had was to focus my analysis on each day of the week. The reason for this is I wanted to know if casual riders used the bikes more on the weekend or maybe at a certain time of day.
![Bike rides per day of the week 2021](https://github.com/flewis267/GoogleDataAnalytics-Cyclist-Casestudy/assets/81341510/87b361c8-7cab-4b00-8432-d783f189d1fc)
Unfortunately, the percentage of casual riders vs. members does not really change at all on any day. This surprised me as I expected there to be more casual riders on the weekends.

##
The third idea I wanted to try was to check how casual and member riders used the bikes by start hour (24-hour clock). I was looking for a time of day (in chunks of 2 hours) when casual riders outweighed members.
![start_hour](https://github.com/flewis267/GoogleDataAnalytics-Cyclist-Casestudy/assets/81341510/955c0c78-6439-40ed-a043-72a77f16684c)
This shows the time of day when casual riders mostly start a ride. In the middle of the day from around 10 AM to 10 PM, the number of casual riders is dwarfed by members but still has a significant rider base. Between 10 PM to 4 AM, there are more casual riders than members although the number of rides taken is significantly less. 

### Act
During the sharing process, I was defining the difference between casual riders and members. The stakeholders, in this case, the marketing department can use my insights to develop a new marketing strategy. I have also been able to outline some problems with the data provided to me. The main issue I had was that I had no way of knowing how many rides each individual user took. This means that I am unsure how many more rides members took than casual riders, I wanted to add the rides per unique user into my data as this would have given me greater flexibility in answering the question set forth by the stakeholders, **How do annual members and casual riders use Cyclistic bikes differently**. Another thing I picked up when writing my SQL queries is that a large percentage of casual riders are also tourists, this means that there would be almost no ability to convert them to members unless Cyclistic expanded beyond Chicago.

My top three recommendations based on the data I have analysed:
1. Cyclistic could look into new subscription models that mimic how the annual membership works on a shorter scale and/or at a certain time of day. An example of this would be annual nighttime passes. As the insights above showed at night most riders are casual riders.
2. Cyclistic could also use my insights to target their advertising based on local areas where there are classic bike stations. Both members and casual users used more classic bikes overall, so by targeting these people Cyclistic would get more users which should therefore translate into more annual members.     
3. The final recommendation I have for the stakeholders at Cyclistic is to give an incentive to casual riders who use the bikes for a significant amount of time. My findings show that casual riders spend 27 minutes per ride on average compared to annual members who spend 13 minutes and 27 seconds. The Cyclistic marketing team could advertise ~10% off membership to casual riders who've rode the bikes over a certain time threshold.


### Cyclistic Capstone Project By Felix Lewis



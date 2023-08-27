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

After running the code I decided to check it worked by creating a bar chart that shows the number of rides started by hour. As this was a test I decided not to add colours.

```r
bike_rides %>% count(start_hour, sort = T) %>% 
  ggplot(aes(x = start_hour, y = n)) + geom_bar(stat = "identity") +
  scale_y_continuous(label = comma) +
  labs(title = "Bike rides started by hour", 
       x = "Start Hour", y = "Ride Count")
```

![image](https://github.com/flewis267/GoogleDataAnalytics-Cyclist-Casestudy/assets/81341510/60c83a67-f1fd-4fb8-9b07-aeac5be74e31)

Finally, I decided to export the bike_rides data frame to a CSV file so it could be loaded into PostgreSQL. 

```r
write.csv(bike_rides, "C:\\Users\\felix\\Downloads\\bike_rides.csv", row.names = FALSE)
```

To check this has worked I wrote some SQL code. I didn't import all columns as I was just testing PostgreSQL to see if it would crash with 5.7 million records.

```sql
SELECT * FROM bike_rides
LIMIT 10
```
**Output**
```
"6C992BD37A98A63F"	"classic_bike"	"State St & Pearson St"	"TA1307000061"	"Southport Ave & Waveland Ave"	"13235"	"41.897448"	"-87.628722"	"41.94815"	"-87.66394"	"member"	"18"	"18"	"2021-04-12"	"2021-04-12"
"1E0145613A209000"	"docked_bike"	"Dorchester Ave & 49th St"	"KA1503000069"	"Dorchester Ave & 49th St"	"KA1503000069"	"41.805772"	"-87.592464"	"41.805772"	"-87.592464"	"casual"	"17"	"18"	"2021-04-27"	"2021-04-27"
"E498E15508A80BAD"	"docked_bike"	"Loomis Blvd & 84th St"	"20121"	"Loomis Blvd & 84th St"	"20121"	"41.741487"	"-87.65841"	"41.741487"	"-87.65841"	"casual"	"12"	"11"	"2021-04-03"	"2021-04-07"
"1887262AD101C604"	"classic_bike"	"Honore St & Division St"	"TA1305000034"	"Southport Ave & Waveland Ave"	"13235"	"41.903119"	"-87.673935"	"41.94815"	"-87.66394"	"member"	"9"	"9"	"2021-04-17"	"2021-04-17"
"C123548CAB2A32A5"	"docked_bike"	"Loomis Blvd & 84th St"	"20121"	"Loomis Blvd & 84th St"	"20121"	"41.741487"	"-87.65841"	"41.741487"	"-87.65841"	"casual"	"12"	"14"	"2021-04-03"	"2021-04-03"
"097E76F3651B1AC1"	"classic_bike"	"Clinton St & Polk St"	"15542"	"Clinton St & Polk St"	"15542"	"41.87146651779"	"-87.6409491327"	"41.87146651779"	"-87.6409491327"	"casual"	"18"	"18"	"2021-04-25"	"2021-04-25"
"53C38EB01E6FA5C4"	"classic_bike"	"Ashland Ave & 63rd St"	"16948"	"Ashland Ave & 63rd St"	"16948"	"41.779374"	"-87.664843"	"41.779374"	"-87.664843"	"casual"	"16"	"16"	"2021-04-03"	"2021-04-03"
"D53AC014EFD6E2BA"	"electric_bike"	"Dorchester Ave & 49th St"	"KA1503000069"	"Dorchester Ave & 49th St"	"KA1503000069"	"41.8058323333333"	"-87.5924785"	"41.8058028333333"	"-87.5926615"	"casual"	"16"	"17"	"2021-04-06"	"2021-04-06"
"6E2F7CA1FA9E0AFB"	"classic_bike"	"Ashland Ave & 63rd St"	"16948"	"Ashland Ave & 63rd St"	"16948"	"41.779374"	"-87.664843"	"41.779374"	"-87.664843"	"casual"	"15"	"16"	"2021-04-12"	"2021-04-12"
"04218447AAC80BD1"	"classic_bike"	"Dorchester Ave & 49th St"	"KA1503000069"	"Dorchester Ave & 49th St"	"KA1503000069"	"41.805772"	"-87.592464"	"41.805772"	"-87.592464"	"casual"	"15"	"15"	"2021-04-24"	"2021-04-24"
```


### Process
Now I have prepared the data, I can process it. To do this I will continue to use PostgreSQL. I decided on PostgreSQL due to my familiarity with it and good reputation in the SQL industry. PostgreSQL is used by many large companies such as Netflix, Uber, Twitch, Instagram, Spotify, Reddit, and Skype. To keep data integrity I set out to ensure the trustworthiness and reliability of the data. I achieved this by checking for human error after each change to the database and keeping a changelog to revert any errors I have made. However, because of the documentation process I used (this Github readme file) the changelog is void but wouldn't be in production code.

The columns of my database:
* ride_id: The primary key, I checked in r for no duplicates before importing into PostgreSQL. Each string is 16 characters long. I will not need to do any data cleaning for this column.
* rideable_type: The data consists of 2 different string values (formally 3), 'classic_bike', 'electic_bike', and formally 'docked_bike'.
* start_station_name/end_station_name: The name of the start station and end station. It should be noted that only classic bikes have a start station and end station. Electric bikes can be parked anywhere.
* start_station_id/end_station_id: The id for the start station and end station.
* start_lat/end_lat & start_lng/end_lng: The start and end longitudes and latitudes for each bike ride.
* member_casual: The data can only be 2 string values. Either 'casual' or 'member'. A casual user pays per ride, while a member pays yearly. The stakeholders want to make more casual riders become members. 
* start_hour/end_hour: The start and end hour on a 24-hour clock. This allows me to get a general idea of what time of the day bikes are rented.
* start_date/end_date: The start and end date that the bikes are rented.

**Cleaning the data**
Now I have all my data filtered, sorted, and modified for analysis the last thing I need to do is clean the data. This entails removing rows that have invalid data that could potentially cause unintended bias in the data. An example of this would be if a classic bike doesn't have a start or end location.

Here are the following cleaning steps I have taken:
* Replace docked_bike with classic_bike as the docked bike is a legacy name for the classic bike.
```sql
UPDATE bike_rides
SET rideable_type = 'classic_bike'
WHERE rideable_type = 'docked_bike'
```
* Delete any rows where a classic bike has no start and/or end station.

* Delete any rows where the start/end longitude or latitude are null.
* Format the start and end station names and trim beginning and end spaces.
* Remove trips under 1 minute and above 1 day.


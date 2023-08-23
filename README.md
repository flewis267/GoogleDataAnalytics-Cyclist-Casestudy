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

### Process
I have chosen to use 

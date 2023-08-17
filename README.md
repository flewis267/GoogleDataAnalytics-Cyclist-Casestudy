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

To prepare the data I downloaded 12 months of data and merged them in RStudio. The first thing I did before starting is to install some dependencies that will help me achieve future tasks in R.

```r
library(tidyverse)
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

### Process
I have chosen to use 

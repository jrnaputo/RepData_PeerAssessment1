# **Reproducible Research Course Project 1**
#### *John Joseph R. Naputo*
#### *May 16, 2019*


### Introduction
It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the "quantified self" movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.
The data for this assignment can be downloaded from the course web site:

Dataset: [Activity monitoring data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip) [52K]

The variables included in this dataset are:

**steps**: Number of steps taking in a 5-minute interval (missing values are coded as *NA*)

**date**: The date on which the measurement was taken in YYYY-MM-DD format

**interval**: Identifier for the 5-minute interval in which measurement was taken

The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.

### Review Criteria 

**Repo**

1. Valid GitHub URL
2. At least one commit beyond the original fork
3. Valid SHA-1
4. SHA-1 corresponds to a specific commit

**Commit containing full submission**

1. Code for reading in the dataset and/or processing the data
2. Histogram of the total number of steps taken each day
3. Mean and median number of steps taken each day
4. Time series plot of the average number of steps taken
5. The 5-minute interval that, on average, contains the maximum number of steps
6. Code to describe and show a strategy for imputing missing data
7. Histogram of the total number of steps taken each day after missing values are imputed
8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends
9. All of the R code needed to reproduce the results (numbers, plots, etc.) in the report

### Assignment
This assignment will be described in multiple parts. You will need to write a report that answers the questions detailed below. Ultimately, you will need to complete the entire assignment in a **single R markdown** document that can be processed by **knitr** and be transformed into an HTML file.

Throughout your report make sure you always include the code that you used to generate the output you present. When writing code chunks in the R markdown document, always use *echo = TRUE* so that someone else will be able to read the code. **This assignment will be evaluated via peer assessment so it is essential that your peer evaluators be able to review the code for your analysis.**

For the plotting aspects of this assignment, feel free to use any plotting system in R (i.e., base, lattice, ggplot2)

Fork/clone the [GitHub repository created for this assignment](http://github.com/rdpeng/RepData_PeerAssessment1). You will submit this assignment by pushing your completed files into your forked repository on GitHub. The assignment submission will consist of the URL to your GitHub repository and the SHA-1 commit ID for your repository state.

NOTE: The GitHub repository also contains the dataset for the assignment so you do not have to download the data separately.

### Installing and loading necessary packages

```r
#install.packages("knitr")
#install.packages("dplyr")
#install.packages("ggplot2")
#install.packages("scales")
#install.packages("timeDate")
#install.packages("xtable")

library(knitr)
library(dplyr)
library(ggplot2)
library(scales)
library(timeDate)
library(xtable)
```

### Downloading the data from the web

```r
if(!file.exists("./data")){dir.create("./data")}
fileUrl <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
download.file(fileUrl, destfile = "./data/ActivityMonitoring_data.zip")
unzip("./data/ActivityMonitoring_data.zip", exdir = "./data")
```

### Loading and preprocessing the data
Show any code that is needed to

1. Load the data (i.e. *read.csv()*)
2. Process/transform the data (if necessary) into a format suitable for your analysis


```r
activity <- read.csv("./data/activity.csv", header = TRUE, sep = ",")
activity$date <- as.Date(activity$date)

FilteredActivity <- subset(activity, !is.na(activity$steps))
```

### What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.

1. Calculate the total number of steps taken per day
2. If you do not understand the difference between histogram and a barplot, research the difference between them. Make a histograme of the total number of steps taken each day
3. Calculate and report the mean and median of the total number of steps taken per day


```r
by_date <- group_by(FilteredActivity, date)
StepsPerDay <- summarise(by_date, TotalSteps = sum(steps, na.rm = TRUE), 
                         Mean = mean(steps, na.rm = TRUE),
                         Median = median(steps, na.rm = TRUE))

kable(StepsPerDay[,1:2], caption = "Total Number of Steps Taken Each Day")
```



|date       | TotalSteps|
|:----------|----------:|
|2012-10-02 |        126|
|2012-10-03 |      11352|
|2012-10-04 |      12116|
|2012-10-05 |      13294|
|2012-10-06 |      15420|
|2012-10-07 |      11015|
|2012-10-09 |      12811|
|2012-10-10 |       9900|
|2012-10-11 |      10304|
|2012-10-12 |      17382|
|2012-10-13 |      12426|
|2012-10-14 |      15098|
|2012-10-15 |      10139|
|2012-10-16 |      15084|
|2012-10-17 |      13452|
|2012-10-18 |      10056|
|2012-10-19 |      11829|
|2012-10-20 |      10395|
|2012-10-21 |       8821|
|2012-10-22 |      13460|
|2012-10-23 |       8918|
|2012-10-24 |       8355|
|2012-10-25 |       2492|
|2012-10-26 |       6778|
|2012-10-27 |      10119|
|2012-10-28 |      11458|
|2012-10-29 |       5018|
|2012-10-30 |       9819|
|2012-10-31 |      15414|
|2012-11-02 |      10600|
|2012-11-03 |      10571|
|2012-11-05 |      10439|
|2012-11-06 |       8334|
|2012-11-07 |      12883|
|2012-11-08 |       3219|
|2012-11-11 |      12608|
|2012-11-12 |      10765|
|2012-11-13 |       7336|
|2012-11-15 |         41|
|2012-11-16 |       5441|
|2012-11-17 |      14339|
|2012-11-18 |      15110|
|2012-11-19 |       8841|
|2012-11-20 |       4472|
|2012-11-21 |      12787|
|2012-11-22 |      20427|
|2012-11-23 |      21194|
|2012-11-24 |      14478|
|2012-11-25 |      11834|
|2012-11-26 |      11162|
|2012-11-27 |      13646|
|2012-11-28 |      10183|
|2012-11-29 |       7047|

```r
hist(StepsPerDay$TotalSteps, xlab = "Total Steps", 
     main = "Total Number of Steps Taken Each Day", breaks = seq(0,25000, by = 2500))
```

![plot of chunk unnamed-chunk-19](figure/unnamed-chunk-19-1.png)

```r
kable(Mean_Median_StepsPerDay <- StepsPerDay[,c(1,3:4)], 
      caption = "Mean and Median of the Total Number of Steps Taken Each Day")
```



|date       |       Mean| Median|
|:----------|----------:|------:|
|2012-10-02 |  0.4375000|      0|
|2012-10-03 | 39.4166667|      0|
|2012-10-04 | 42.0694444|      0|
|2012-10-05 | 46.1597222|      0|
|2012-10-06 | 53.5416667|      0|
|2012-10-07 | 38.2465278|      0|
|2012-10-09 | 44.4826389|      0|
|2012-10-10 | 34.3750000|      0|
|2012-10-11 | 35.7777778|      0|
|2012-10-12 | 60.3541667|      0|
|2012-10-13 | 43.1458333|      0|
|2012-10-14 | 52.4236111|      0|
|2012-10-15 | 35.2048611|      0|
|2012-10-16 | 52.3750000|      0|
|2012-10-17 | 46.7083333|      0|
|2012-10-18 | 34.9166667|      0|
|2012-10-19 | 41.0729167|      0|
|2012-10-20 | 36.0937500|      0|
|2012-10-21 | 30.6284722|      0|
|2012-10-22 | 46.7361111|      0|
|2012-10-23 | 30.9652778|      0|
|2012-10-24 | 29.0104167|      0|
|2012-10-25 |  8.6527778|      0|
|2012-10-26 | 23.5347222|      0|
|2012-10-27 | 35.1354167|      0|
|2012-10-28 | 39.7847222|      0|
|2012-10-29 | 17.4236111|      0|
|2012-10-30 | 34.0937500|      0|
|2012-10-31 | 53.5208333|      0|
|2012-11-02 | 36.8055556|      0|
|2012-11-03 | 36.7048611|      0|
|2012-11-05 | 36.2465278|      0|
|2012-11-06 | 28.9375000|      0|
|2012-11-07 | 44.7326389|      0|
|2012-11-08 | 11.1770833|      0|
|2012-11-11 | 43.7777778|      0|
|2012-11-12 | 37.3784722|      0|
|2012-11-13 | 25.4722222|      0|
|2012-11-15 |  0.1423611|      0|
|2012-11-16 | 18.8923611|      0|
|2012-11-17 | 49.7881944|      0|
|2012-11-18 | 52.4652778|      0|
|2012-11-19 | 30.6979167|      0|
|2012-11-20 | 15.5277778|      0|
|2012-11-21 | 44.3993056|      0|
|2012-11-22 | 70.9270833|      0|
|2012-11-23 | 73.5902778|      0|
|2012-11-24 | 50.2708333|      0|
|2012-11-25 | 41.0902778|      0|
|2012-11-26 | 38.7569444|      0|
|2012-11-27 | 47.3819444|      0|
|2012-11-28 | 35.3576389|      0|
|2012-11-29 | 24.4687500|      0|

### What is the average daily pattern?

1. Make a time series plot (i.e. *type = "l"*) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)
2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
by_interval <- group_by(FilteredActivity, interval)
DailyPattern <- summarise(by_interval, AverageStep = mean(steps, na.rm = TRUE))

ggplot(data = DailyPattern, aes(x = interval, y = AverageStep)) + geom_line() +
        labs(x = "Minute Interval", y = "Average Steps") + 
        ggtitle("Average Daily Pattern") + theme(plot.title = element_text(hjust = 0.5))
```

![plot of chunk unnamed-chunk-20](figure/unnamed-chunk-20-1.png)

```r
DailyPattern[which.max(DailyPattern$AverageStep),]$interval
```

```
## [1] 835
```

### Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded as *NA*). The presence of missing days may introduce bias into some calculations or summaries of the data.

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with *NAs*)
2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
3. Create a new dataset that is equal to the original dataset but with the missing data filled in.
4. Make a histogram of the total number of steps taken each day and Calculate and report the **mean** and **median** total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```r
sum(is.na(activity))
```

```
## [1] 2304
```

```r
ActivityNew <- activity

for (i in 1:nrow(ActivityNew)) {
        if (is.na(ActivityNew[i,1])) {

                        ActivityNew[i,1] = DailyPattern[which(DailyPattern$interval 
                                                              == ActivityNew[i,3]),2]
        }
        else {
                ActivityNew[i,1] = ActivityNew[i,1]
        }
}

by_date_New <- group_by(ActivityNew, date)
StepsPerDay_New <- summarise(by_date_New, TotalSteps = sum(steps, na.rm = TRUE),
                             Mean = mean(steps, na.rm = TRUE),
                             Median = median(steps, na.rm = TRUE))

hist(StepsPerDay_New$TotalSteps, xlab = "Total Steps", 
     main = "Total Number of Steps Taken Each Day", breaks = seq(0,25000, by = 2500))
```

![plot of chunk unnamed-chunk-21](figure/unnamed-chunk-21-1.png)

```r
kable(Mean_Median_New <- StepsPerDay_New[,c(1,3:4)],
      caption = "Mean and Median of the Total Number of Steps Taken Each Day")
```



|date       |       Mean|   Median|
|:----------|----------:|--------:|
|2012-10-01 | 37.3825996| 34.11321|
|2012-10-02 |  0.4375000|  0.00000|
|2012-10-03 | 39.4166667|  0.00000|
|2012-10-04 | 42.0694444|  0.00000|
|2012-10-05 | 46.1597222|  0.00000|
|2012-10-06 | 53.5416667|  0.00000|
|2012-10-07 | 38.2465278|  0.00000|
|2012-10-08 | 37.3825996| 34.11321|
|2012-10-09 | 44.4826389|  0.00000|
|2012-10-10 | 34.3750000|  0.00000|
|2012-10-11 | 35.7777778|  0.00000|
|2012-10-12 | 60.3541667|  0.00000|
|2012-10-13 | 43.1458333|  0.00000|
|2012-10-14 | 52.4236111|  0.00000|
|2012-10-15 | 35.2048611|  0.00000|
|2012-10-16 | 52.3750000|  0.00000|
|2012-10-17 | 46.7083333|  0.00000|
|2012-10-18 | 34.9166667|  0.00000|
|2012-10-19 | 41.0729167|  0.00000|
|2012-10-20 | 36.0937500|  0.00000|
|2012-10-21 | 30.6284722|  0.00000|
|2012-10-22 | 46.7361111|  0.00000|
|2012-10-23 | 30.9652778|  0.00000|
|2012-10-24 | 29.0104167|  0.00000|
|2012-10-25 |  8.6527778|  0.00000|
|2012-10-26 | 23.5347222|  0.00000|
|2012-10-27 | 35.1354167|  0.00000|
|2012-10-28 | 39.7847222|  0.00000|
|2012-10-29 | 17.4236111|  0.00000|
|2012-10-30 | 34.0937500|  0.00000|
|2012-10-31 | 53.5208333|  0.00000|
|2012-11-01 | 37.3825996| 34.11321|
|2012-11-02 | 36.8055556|  0.00000|
|2012-11-03 | 36.7048611|  0.00000|
|2012-11-04 | 37.3825996| 34.11321|
|2012-11-05 | 36.2465278|  0.00000|
|2012-11-06 | 28.9375000|  0.00000|
|2012-11-07 | 44.7326389|  0.00000|
|2012-11-08 | 11.1770833|  0.00000|
|2012-11-09 | 37.3825996| 34.11321|
|2012-11-10 | 37.3825996| 34.11321|
|2012-11-11 | 43.7777778|  0.00000|
|2012-11-12 | 37.3784722|  0.00000|
|2012-11-13 | 25.4722222|  0.00000|
|2012-11-14 | 37.3825996| 34.11321|
|2012-11-15 |  0.1423611|  0.00000|
|2012-11-16 | 18.8923611|  0.00000|
|2012-11-17 | 49.7881944|  0.00000|
|2012-11-18 | 52.4652778|  0.00000|
|2012-11-19 | 30.6979167|  0.00000|
|2012-11-20 | 15.5277778|  0.00000|
|2012-11-21 | 44.3993056|  0.00000|
|2012-11-22 | 70.9270833|  0.00000|
|2012-11-23 | 73.5902778|  0.00000|
|2012-11-24 | 50.2708333|  0.00000|
|2012-11-25 | 41.0902778|  0.00000|
|2012-11-26 | 38.7569444|  0.00000|
|2012-11-27 | 47.3819444|  0.00000|
|2012-11-28 | 35.3576389|  0.00000|
|2012-11-29 | 24.4687500|  0.00000|
|2012-11-30 | 37.3825996| 34.11321|

### Are there differences in activity patterns between weekdays and weekends?
For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

1. Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.
2. Make a panel plot containing a time series plot (i.e. type="l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
activity$day <- ifelse(isWeekday(activity$date) == TRUE, "Weekday", "Weekend")

interval_by_day <- aggregate(steps~interval + day, activity, mean, na.rm = TRUE)

ggplot(interval_by_day, aes(x = interval, y = steps, color = day)) + geom_line() + 
        facet_wrap(~day, ncol = 1, nrow = 2) + 
        labs(title = "Activity Pattern Between Weekday and Weekend", x = "Interval", y = "Average number of steps") +
        theme(plot.title = element_text(hjust = 0.5))
```

![plot of chunk unnamed-chunk-22](figure/unnamed-chunk-22-1.png)

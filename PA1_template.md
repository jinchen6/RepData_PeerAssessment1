---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data
Show any code that is needed to
Load the data (i.e. read.csv())
Process/transform the data (if necessary) into a format suitable for your analysis

```r
activity <- read.table("activity.csv", sep = "," ,header = T )
```

## What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.
1. Calculate the total number of steps taken per day
2.If you do not understand the difference between a histogram and a barplot, research the difference between them. 
Make a histogram of the total number of steps taken each day
3. Calculate and report the mean and median of the total number of steps taken per day


```r
knitr::opts_chunk$set(echo = TRUE)

aTotal <- with(activity, aggregate(steps, by = list(date), sum))

names(aTotal)[[1]] <- "Date"
names(aTotal)[[2]] <- "Total Steps"
```



```r
hist(aTotal$`Total Steps`, col = "green", xlab="total number of steps taken each day", ylab="Frequency of Total Steps", 
     main="Histogram of the total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

3. Calculate and report the mean and median of the total number of steps taken per day


```r
aMean <- mean(aTotal$`Total Steps`, na.rm = T)
aMean
```

```
## [1] 10766.19
```

```r
aMedian <- median(aTotal$`Total Steps`, na.rm = T)
aMedian 
```

```
## [1] 10765
```

## What is the average daily activity pattern?
1. Make a time series plot(i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, 
averaged across all days (y-axis)

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?
1. Make a time series plot(i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)



```r
hist(aTotal$`Total Steps`, col = "green", xlab="total number of steps taken each day", ylab="Frequency of Total Steps", 
     main="Histogram of the total number of steps taken each day")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->



```r
stepsInterval<-aggregate(steps~interval, activity, mean)
with(stepsInterval, plot(interval, steps, type = "l", col= "blue"))
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
stepsInterval[which.max(stepsInterval[,2]),1]
```

```
## [1] 835
```


## Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded as \color{red}{\verb|NA|}NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, 
you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

4. Make a histogram of the total number of steps taken each day and 
Calculate and report the mean and median total number of steps taken per day. 
Do these values differ from the estimates from the first part of the assignment?
What is the impact of imputing missing data on the estimates of the total daily number of steps?


Are there differences in activity patterns between weekdays and weekends?
For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.


```r
missingValues <- is.na(activity[, 1])
sum(missingValues)
```

```
## [1] 2304
```

```r
stepsInterval<-aggregate(steps~interval, activity, mean)
m<-mean(stepsInterval$steps)
```


```r
activityImputed<-activity
activityImputed[missingValues,1]<-m

aTotalImputed <- with(activityImputed, aggregate(steps, by = list(date), sum))
names(aTotalImputed) <- c("date","steps")

hist(aTotalImputed$steps, xlab="Total steps per day after missing values are imputed" , col="green")
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png)<!-- -->



```r
aMean <- mean(aTotal$`Total Steps`, na.rm = T)
aMean
```

```
## [1] 10766.19
```

```r
aMedian <- median(aTotal$`Total Steps`, na.rm = T)
aMedian 
```

```
## [1] 10765
```


## Are there differences in activity patterns between weekdays and weekends?
1.  Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

2.  Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, 
averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot 
should look like using simulated data.




```r
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 4.1.2
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(lattice)

activityImputed$date <- as.Date(activityImputed$date)

activityWeekdays<-activityImputed%>%
  mutate(weekDay = ifelse(weekdays(activityImputed$date)=="Saturday" | weekdays(activityImputed$date)=="Sunday", "Weekend", "Weekday"))

aveStepWeekdayInterval<-activityWeekdays %>%
  group_by(weekDay, interval) %>%
  summarize(aveStepWeekday=sum(steps))
```

```
## `summarise()` has grouped output by 'weekDay'. You can override using the
## `.groups` argument.
```

```r
xyplot(aveStepWeekday ~ interval | weekDay, data = aveStepWeekdayInterval,layout = c(2, 1),  type ="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
  

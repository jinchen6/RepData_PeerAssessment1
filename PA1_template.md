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




![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

3. Calculate and report the mean and median of the total number of steps taken per day


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
aMean <- with(activity, aggregate(steps, by = list(date), mean, na.rm = T))
aMedian <- with(activity, aggregate(steps, by = list(date), median, na.rm = T))
names(aMean)[[1]] <- "date"
names(aMean)[[2]] <- "mean"
names(aMedian)[[2]] <-  "median"

aMeanMedian <- cbind(aMean$date,aMean$mean,aMedian$median)
aMeanMedian <- cbind(as.character(aMean$date),round(aMean$mean,0),aMedian$median)
aMeanMedian <- as.data.frame(aMeanMedian)
colnames(aMeanMedian) <-c("Date","Mean","Median")
aMeanMedian
```

```
##          Date Mean Median
## 1  2012-10-01  NaN   <NA>
## 2  2012-10-02    0      0
## 3  2012-10-03   39      0
## 4  2012-10-04   42      0
## 5  2012-10-05   46      0
## 6  2012-10-06   54      0
## 7  2012-10-07   38      0
## 8  2012-10-08  NaN   <NA>
## 9  2012-10-09   44      0
## 10 2012-10-10   34      0
## 11 2012-10-11   36      0
## 12 2012-10-12   60      0
## 13 2012-10-13   43      0
## 14 2012-10-14   52      0
## 15 2012-10-15   35      0
## 16 2012-10-16   52      0
## 17 2012-10-17   47      0
## 18 2012-10-18   35      0
## 19 2012-10-19   41      0
## 20 2012-10-20   36      0
## 21 2012-10-21   31      0
## 22 2012-10-22   47      0
## 23 2012-10-23   31      0
## 24 2012-10-24   29      0
## 25 2012-10-25    9      0
## 26 2012-10-26   24      0
## 27 2012-10-27   35      0
## 28 2012-10-28   40      0
## 29 2012-10-29   17      0
## 30 2012-10-30   34      0
## 31 2012-10-31   54      0
## 32 2012-11-01  NaN   <NA>
## 33 2012-11-02   37      0
## 34 2012-11-03   37      0
## 35 2012-11-04  NaN   <NA>
## 36 2012-11-05   36      0
## 37 2012-11-06   29      0
## 38 2012-11-07   45      0
## 39 2012-11-08   11      0
## 40 2012-11-09  NaN   <NA>
## 41 2012-11-10  NaN   <NA>
## 42 2012-11-11   44      0
## 43 2012-11-12   37      0
## 44 2012-11-13   25      0
## 45 2012-11-14  NaN   <NA>
## 46 2012-11-15    0      0
## 47 2012-11-16   19      0
## 48 2012-11-17   50      0
## 49 2012-11-18   52      0
## 50 2012-11-19   31      0
## 51 2012-11-20   16      0
## 52 2012-11-21   44      0
## 53 2012-11-22   71      0
## 54 2012-11-23   74      0
## 55 2012-11-24   50      0
## 56 2012-11-25   41      0
## 57 2012-11-26   39      0
## 58 2012-11-27   47      0
## 59 2012-11-28   35      0
## 60 2012-11-29   24      0
## 61 2012-11-30  NaN   <NA>
```

## What is the average daily activity pattern?
1. Make a time series plot(i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, 
averaged across all days (y-axis)

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?
1. Make a time series plot(i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

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
Yes.
What is the impact of imputing missing data on the estimates of the total daily number of steps?
NAs is replaced by the filled in value.

Are there differences in activity patterns between weekdays and weekends?
Yes.
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

![](PA1_template_files/figure-html/unnamed-chunk-8-1.png)<!-- -->



```
##          Date Mean Median
## 1  2012-10-01   37     37
## 2  2012-10-02    0      0
## 3  2012-10-03   39      0
## 4  2012-10-04   42      0
## 5  2012-10-05   46      0
## 6  2012-10-06   54      0
## 7  2012-10-07   38      0
## 8  2012-10-08   37     37
## 9  2012-10-09   44      0
## 10 2012-10-10   34      0
## 11 2012-10-11   36      0
## 12 2012-10-12   60      0
## 13 2012-10-13   43      0
## 14 2012-10-14   52      0
## 15 2012-10-15   35      0
## 16 2012-10-16   52      0
## 17 2012-10-17   47      0
## 18 2012-10-18   35      0
## 19 2012-10-19   41      0
## 20 2012-10-20   36      0
## 21 2012-10-21   31      0
## 22 2012-10-22   47      0
## 23 2012-10-23   31      0
## 24 2012-10-24   29      0
## 25 2012-10-25    9      0
## 26 2012-10-26   24      0
## 27 2012-10-27   35      0
## 28 2012-10-28   40      0
## 29 2012-10-29   17      0
## 30 2012-10-30   34      0
## 31 2012-10-31   54      0
## 32 2012-11-01   37     37
## 33 2012-11-02   37      0
## 34 2012-11-03   37      0
## 35 2012-11-04   37     37
## 36 2012-11-05   36      0
## 37 2012-11-06   29      0
## 38 2012-11-07   45      0
## 39 2012-11-08   11      0
## 40 2012-11-09   37     37
## 41 2012-11-10   37     37
## 42 2012-11-11   44      0
## 43 2012-11-12   37      0
## 44 2012-11-13   25      0
## 45 2012-11-14   37     37
## 46 2012-11-15    0      0
## 47 2012-11-16   19      0
## 48 2012-11-17   50      0
## 49 2012-11-18   52      0
## 50 2012-11-19   31      0
## 51 2012-11-20   16      0
## 52 2012-11-21   44      0
## 53 2012-11-22   71      0
## 54 2012-11-23   74      0
## 55 2012-11-24   50      0
## 56 2012-11-25   41      0
## 57 2012-11-26   39      0
## 58 2012-11-27   47      0
## 59 2012-11-28   35      0
## 60 2012-11-29   24      0
## 61 2012-11-30   37     37
```


## Are there differences in activity patterns between weekdays and weekends?
1.  Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

2.  Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, 
averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot 
should look like using simulated data.




```
## `summarise()` has grouped output by 'weekDay'. You can override using the
## `.groups` argument.
```

![](PA1_template_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  

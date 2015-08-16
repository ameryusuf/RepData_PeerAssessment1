---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---

The very first step is to load and preprocess the data and for this we need to go step by step as per the instructions. We load unzip the file, read the file into R. In the same step loading the dplyr library to group the dataframe by date to make the necessary manipulations on the data.


```r
## Loading and preprocessing the data

unzip("./repdata/RepData_PeerAssessment1/activity.zip", exdir = "./repdata/RepData_PeerAssessment1")
```

```
## Warning in unzip("./repdata/RepData_PeerAssessment1/activity.zip", exdir =
## "./repdata/RepData_PeerAssessment1"): error 1 in extracting from zip file
```

```r
activity<- read.csv("./repdata/RepData_PeerAssessment1/activity.csv", header = TRUE, stringsAsFactors = FALSE)
```

```
## Warning in file(file, "rt"): cannot open file './repdata/
## RepData_PeerAssessment1/activity.csv': No such file or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```

```r
library(dplyr)

# Group the activities by date to make the manipulation on data easy
activity_day<-group_by(activity, date)
```

## What is mean total number of steps taken per day?

The mean total number of steps are 

```r
sum_day<-summarise(activity_day, sum = sum(steps))
```

## Make a histogram of the total number of steps taken each day


```r
library(lattice)
histogram(~sum, sum_day, xlab = 'Number of Steps taken per Day')
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

##Calculate and report the mean and median of the total number of steps taken per day

The mean and median are

```r
mean_median<-summarise(sum_day, mean = mean(sum, na.rm = TRUE), median = median(sum, na.rm = TRUE))
```

## What is the average daily activity pattern?


A time series plot

```r
activity_time<-group_by(activity, interval)
avg_int<-summarise(activity_time, average = mean(steps, na.rm = TRUE))
xyplot(average~interval, avg_int, type = "l", xlab = "5-minute Interval", ylab = "Average number of steps across all days")
```

```
## Warning in order(as.numeric(x)): NAs introduced by coercion
```

```
## Warning in diff(as.numeric(x[ord])): NAs introduced by coercion
```

```
## Warning in (function (x, y, type = "p", groups = NULL, pch = if
## (is.null(groups)) plot.symbol$pch else superpose.symbol$pch, : NAs
## introduced by coercion
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 

##Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

The maximum number of steps are in 

```r
avg_int[which.max(avg_int$average),]
```

```
## Source: local data frame [1 x 2]
## 
##   interval  average
## 1    08:35 206.1698
```


###Imputing missing values


##Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
summary(activity)
```

```
##      steps            date             interval        
##  Min.   :  0.00   Length:17568       Length:17568      
##  1st Qu.:  0.00   Class :character   Class :character  
##  Median :  0.00   Mode  :character   Mode  :character  
##  Mean   : 37.38                                        
##  3rd Qu.: 12.00                                        
##  Max.   :806.00                                        
##  NA's   :2304
```

```r
sum(is.na(activity))
```

```
## [1] 2304
```

##Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.


```r
newdata<-activity

if(newdata$interval <500)
{
newdata[index_na] == mean(avg_int[1:40,2])
}
```

```
## Warning in if (newdata$interval < 500) {: the condition has length > 1 and
## only the first element will be used
```

```
## Error in `[.data.frame`(newdata, index_na): object 'index_na' not found
```

```r
elseif(newdata$interval <1000)
```

```
## Error in eval(expr, envir, enclos): could not find function "elseif"
```

```r
{
newdata[index_na] == mean(avg_int[40:80,2])
}
```

```
## Error in `[.data.frame`(newdata, index_na): object 'index_na' not found
```


## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?

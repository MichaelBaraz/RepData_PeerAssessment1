# Reproducible Research: Peer Assessment 1
Michael Baraz  
Feb 09, 2016  


## Loading and preprocessing the data
Show any code that is needed to

1. Load the data (i.e. **read.csv()**)
2. Process/transform the data (if necessary) into a format suitable for your analysis

Code for reading in the dataset and/or processing the data
If the file does not exist then download archive file 


```r
archiveFile <- "../repdata-data-activity.zip"
if(!file.exists(archiveFile)) {
  archiveURL <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
  if(Sys.info()["sysname"] == "Windows") {
    download.file(url=archiveURL,destfile=archiveFile,method="curl")
  } else {
    download.file(url=url,destfile=archiveFile)
  }
}
if(!(file.exists("activity.txt"))) { unzip(archiveFile) }

## Read data files into data frames
#  Note: stringsAsFactors is set to FALSE so that dates are not factors but strings 
#  that will be converted to dates later
activity <- read.csv("activity.csv", stringsAsFactors = F)
```

### After we read in the data file, inspect contents of the data frame.

```r
names(activity)
```

```
## [1] "steps"    "date"     "interval"
```

```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : chr  "2012-10-01" "2012-10-01" "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

```r
head(activity, 10)
```

```
##    steps       date interval
## 1     NA 2012-10-01        0
## 2     NA 2012-10-01        5
## 3     NA 2012-10-01       10
## 4     NA 2012-10-01       15
## 5     NA 2012-10-01       20
## 6     NA 2012-10-01       25
## 7     NA 2012-10-01       30
## 8     NA 2012-10-01       35
## 9     NA 2012-10-01       40
## 10    NA 2012-10-01       45
```




## What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.

Calculate the total number of steps taken per day

```r
## create a dataset with NA values removed. To consider only those days for which
#  step counts were recorded, use the complete.cases method
activity_complete_days <- activity[complete.cases(activity), ]

## Perform the analysis required for this part of the assignment using the group_by 
#  and summarize functions from dplyr. The sum can be calculated using summarise once
#  the data has been organized by date using group_by.

## Use group_by and summarize functions from dplyr
library("dplyr")
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
step_summary  <-  activity_complete_days %>% 
  group_by(date) %>% 
  summarize(daily_step_count = sum(steps))

## verify new data frame
head(step_summary, 5)
```

```
## Source: local data frame [5 x 2]
## 
##         date daily_step_count
##        (chr)            (int)
## 1 2012-10-02              126
## 2 2012-10-03            11352
## 3 2012-10-04            12116
## 4 2012-10-05            13294
## 5 2012-10-06            15420
```

```r
## Plot the graph
hist(step_summary$daily_step_count, 
     main = "Histogram of total steps per day",
     xlab = "Total steps Daily",
     col = "blue",
     breaks = 20
     )
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)\

Calculate and report the mean and median of the total number of steps taken per day

```r
## mean
mean(step_summary$daily_step_count)
```

```
## [1] 10766.19
```

```r
## median
median(step_summary$daily_step_count)
```

```
## [1] 10765
```






## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?

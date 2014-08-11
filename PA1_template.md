# Reproducible Research: Peer Assessment 1
In this file, I present the answers for the peer assessment 1 from the *Reproducible Research* course @ Coursera.

##Loading required libraries
The first step consists on charge the required libraries to process the data.

- **ggplot2**:  A plotting system for R, based on the grammar of graphics, which tries to take the good parts of base and lattice graphics and none of the bad.
- **knitr**: A general-purpose tool for dynamic report generation in R (Literate Statistical Programming).
- **impute**: Imputation for microarray data (currently KNN only)
- **markdown**: R Markdown is an authoring format that enables easy creation of dynamic documents, presentations, and reports from R.
- **xtable**: Coerce data to LaTeX and HTML tables.




```r
#install.packages("ggplot2")
library(ggplot2)
library(knitr)
library(impute)
library(markdown)
library(xtable)
```

## Loading and preprocessing the data
In order to perform the analysis, I loaded all the variables included in the dataset.

- **steps**: Number of steps taking in a 5-minute interval (missing values are coded as NA)

- **date**: The date on which the measurement was taken in YYYY-MM-DD format

- **interval**: Identifier for the 5-minute interval in which measurement was taken

The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset.

The following code chunk loads the data in R.


```r
acdat <-  read.csv(unz("activity.zip", "activity.csv"),
                   header = TRUE,
                   colClasses = c("numeric","POSIXct","numeric"))
```

## What is mean total number of steps taken per day?
In this section, an histogram present the distribution of the number of steps taken across all days. 


```r
stepsperday <- with(acdat,tapply(steps,date,sum,na.rm=T))
binw <- (range(stepsperday,na.rm = T)[2]-range(stepsperday,na.rm = T)[1])/30
qplot(stepsperday, binwidth =binw, xlab= "Steps per day", ylab = "Frequency")
```

![plot of chunk steps per day](figure/steps per day.png) 

It is important to notice that there exists a high frecuency of zeros on the histogram due to the lack of data (missing values) in some days. Those missing values will be imputed and the new histogram is presented in two sections below.


In order to provide a more detail view of the data, the following table present the mean and median number of steps taken each day (empty boxes indicates missing data):


```r
step_mean <- with(acdat,tapply(steps,date,mean,na.rm=T))
step_median <- with(acdat,tapply(steps,date,median,na.rm=T))
mm <- data.frame(step_mean, step_median)
xt <- xtable(mm)
print(xt, type= "html")  
```

<!-- html table generated in R 3.1.1 by xtable 1.7-3 package -->
<!-- Mon Aug 11 18:40:25 2014 -->
<TABLE border=1>
<TR> <TH>  </TH> <TH> step_mean </TH> <TH> step_median </TH>  </TR>
  <TR> <TD align="right"> 2012-10-01 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
  <TR> <TD align="right"> 2012-10-02 </TD> <TD align="right"> 0.44 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-03 </TD> <TD align="right"> 39.42 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-04 </TD> <TD align="right"> 42.07 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-05 </TD> <TD align="right"> 46.16 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-06 </TD> <TD align="right"> 53.54 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-07 </TD> <TD align="right"> 38.25 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-08 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
  <TR> <TD align="right"> 2012-10-09 </TD> <TD align="right"> 44.48 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-10 </TD> <TD align="right"> 34.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-11 </TD> <TD align="right"> 35.78 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-12 </TD> <TD align="right"> 60.35 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-13 </TD> <TD align="right"> 43.15 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-14 </TD> <TD align="right"> 52.42 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-15 </TD> <TD align="right"> 35.20 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-16 </TD> <TD align="right"> 52.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-17 </TD> <TD align="right"> 46.71 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-18 </TD> <TD align="right"> 34.92 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-19 </TD> <TD align="right"> 41.07 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-20 </TD> <TD align="right"> 36.09 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-21 </TD> <TD align="right"> 30.63 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-22 </TD> <TD align="right"> 46.74 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-23 </TD> <TD align="right"> 30.97 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-24 </TD> <TD align="right"> 29.01 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-25 </TD> <TD align="right"> 8.65 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-26 </TD> <TD align="right"> 23.53 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-27 </TD> <TD align="right"> 35.14 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-28 </TD> <TD align="right"> 39.78 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-29 </TD> <TD align="right"> 17.42 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-30 </TD> <TD align="right"> 34.09 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-31 </TD> <TD align="right"> 53.52 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-01 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
  <TR> <TD align="right"> 2012-11-02 </TD> <TD align="right"> 36.81 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-03 </TD> <TD align="right"> 36.70 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-04 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
  <TR> <TD align="right"> 2012-11-05 </TD> <TD align="right"> 36.25 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-06 </TD> <TD align="right"> 28.94 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-07 </TD> <TD align="right"> 44.73 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-08 </TD> <TD align="right"> 11.18 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-09 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
  <TR> <TD align="right"> 2012-11-10 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
  <TR> <TD align="right"> 2012-11-11 </TD> <TD align="right"> 43.78 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-12 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-13 </TD> <TD align="right"> 25.47 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-14 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
  <TR> <TD align="right"> 2012-11-15 </TD> <TD align="right"> 0.14 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-16 </TD> <TD align="right"> 18.89 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-17 </TD> <TD align="right"> 49.79 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-18 </TD> <TD align="right"> 52.47 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-19 </TD> <TD align="right"> 30.70 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-20 </TD> <TD align="right"> 15.53 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-21 </TD> <TD align="right"> 44.40 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-22 </TD> <TD align="right"> 70.93 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-23 </TD> <TD align="right"> 73.59 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-24 </TD> <TD align="right"> 50.27 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-25 </TD> <TD align="right"> 41.09 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-26 </TD> <TD align="right"> 38.76 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-27 </TD> <TD align="right"> 47.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-28 </TD> <TD align="right"> 35.36 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-29 </TD> <TD align="right"> 24.47 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-30 </TD> <TD align="right">  </TD> <TD align="right">  </TD> </TR>
   </TABLE>



```r
means <- mean(stepsperday)
medians <- median(stepsperday)
```

Finally, I compute the mean of the daily steps, 9354.23 and its median, 10395.00.

## What is the average daily activity pattern?
In this section, the reader can observe the average dail activity pattern for the studied subject.


```r
acdat$datetime <- acdat$date+acdat$interval*60
interAvg <- with(acdat, tapply(steps, interval,FUN = mean, na.rm =T)) 
interdf <- data.frame(interval = as.numeric(names(interAvg)),interAvg)
ggplot(data = interdf, aes(interval, interAvg)) + geom_line(color="steelblue",size=1)+ ylab("Number of steps") + xlab("Date/time")
```

![plot of chunk time series plot](figure/time series plot.png) 

```r
maxd <- which.max(interdf$interAvg)
hour <- floor(interdf$interval[maxd]/60)
minute <- (interdf$interval[maxd]/60 - floor(interdf$interval[maxd]/60))*60
```

The plot shows that the 5-minute interval 835 of the day (i.e. 13:55) contains the maximum number of steps (206.17) on average.

## Imputing missing values

As the reader may have seen in previous sections, the orginal data contains some missing values: 2304.

In order to perform an exploratory analysis, the missing values were imputed using the `impute.knn()` function from the impute package. This function imputes the missing valules using nearest neighbor averaging.

The R code used is he following.


```r
acdat2 <- acdat  
acdat2$steps<- (impute.knn(
  as.matrix(cbind(acdat2$steps,
                  acdat2$interval,
                  as.numeric(acdat2$date))))$data)[,1]
```

The following histogram exhibits the data distribution with the imputed data. It worth to notice that this graph doesn't has an excess of zeros produced by the missing values. 


```r
stepsperday2 <- with(acdat2,tapply(steps,date,sum,na.rm=T))
binw2 <- (range(stepsperday2,na.rm = T)[2]-range(stepsperday2,na.rm = T)[1])/30
qplot(stepsperday2, binwidth =binw2, xlab= "Steps per day", ylab = "Frequency")
```

![plot of chunk imputation plot](figure/imputation plot.png) 

The following table presents the steps mean and median per day with the imputed data. 


```r
step_mean <- with(acdat2,tapply(steps,date,mean,na.rm=T))
step_median <- with(acdat2,tapply(steps,date,median,na.rm=T))
mm <- data.frame(step_mean, step_median)
xt <- xtable(mm)
print(xt, type= "html")  
```

<!-- html table generated in R 3.1.1 by xtable 1.7-3 package -->
<!-- Mon Aug 11 18:40:26 2014 -->
<TABLE border=1>
<TR> <TH>  </TH> <TH> step_mean </TH> <TH> step_median </TH>  </TR>
  <TR> <TD align="right"> 2012-10-01 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
  <TR> <TD align="right"> 2012-10-02 </TD> <TD align="right"> 0.44 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-03 </TD> <TD align="right"> 39.42 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-04 </TD> <TD align="right"> 42.07 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-05 </TD> <TD align="right"> 46.16 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-06 </TD> <TD align="right"> 53.54 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-07 </TD> <TD align="right"> 38.25 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-08 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
  <TR> <TD align="right"> 2012-10-09 </TD> <TD align="right"> 44.48 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-10 </TD> <TD align="right"> 34.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-11 </TD> <TD align="right"> 35.78 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-12 </TD> <TD align="right"> 60.35 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-13 </TD> <TD align="right"> 43.15 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-14 </TD> <TD align="right"> 52.42 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-15 </TD> <TD align="right"> 35.20 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-16 </TD> <TD align="right"> 52.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-17 </TD> <TD align="right"> 46.71 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-18 </TD> <TD align="right"> 34.92 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-19 </TD> <TD align="right"> 41.07 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-20 </TD> <TD align="right"> 36.09 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-21 </TD> <TD align="right"> 30.63 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-22 </TD> <TD align="right"> 46.74 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-23 </TD> <TD align="right"> 30.97 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-24 </TD> <TD align="right"> 29.01 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-25 </TD> <TD align="right"> 8.65 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-26 </TD> <TD align="right"> 23.53 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-27 </TD> <TD align="right"> 35.14 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-28 </TD> <TD align="right"> 39.78 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-29 </TD> <TD align="right"> 17.42 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-30 </TD> <TD align="right"> 34.09 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-10-31 </TD> <TD align="right"> 53.52 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-01 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
  <TR> <TD align="right"> 2012-11-02 </TD> <TD align="right"> 36.81 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-03 </TD> <TD align="right"> 36.70 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-04 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
  <TR> <TD align="right"> 2012-11-05 </TD> <TD align="right"> 36.25 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-06 </TD> <TD align="right"> 28.94 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-07 </TD> <TD align="right"> 44.73 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-08 </TD> <TD align="right"> 11.18 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-09 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
  <TR> <TD align="right"> 2012-11-10 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
  <TR> <TD align="right"> 2012-11-11 </TD> <TD align="right"> 43.78 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-12 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-13 </TD> <TD align="right"> 25.47 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-14 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
  <TR> <TD align="right"> 2012-11-15 </TD> <TD align="right"> 0.14 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-16 </TD> <TD align="right"> 18.89 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-17 </TD> <TD align="right"> 49.79 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-18 </TD> <TD align="right"> 52.47 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-19 </TD> <TD align="right"> 30.70 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-20 </TD> <TD align="right"> 15.53 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-21 </TD> <TD align="right"> 44.40 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-22 </TD> <TD align="right"> 70.93 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-23 </TD> <TD align="right"> 73.59 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-24 </TD> <TD align="right"> 50.27 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-25 </TD> <TD align="right"> 41.09 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-26 </TD> <TD align="right"> 38.76 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-27 </TD> <TD align="right"> 47.38 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-28 </TD> <TD align="right"> 35.36 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-29 </TD> <TD align="right"> 24.47 </TD> <TD align="right"> 0.00 </TD> </TR>
  <TR> <TD align="right"> 2012-11-30 </TD> <TD align="right"> 37.38 </TD> <TD align="right"> 37.38 </TD> </TR>
   </TABLE>


```r
means2 <- mean(stepsperday2)
medians2 <- median(stepsperday2)
```


Therefore, we can compute a more precise steps' mean and median across the days: 10766.19 and 10766.19, respectively.  


## Are there differences in activity patterns between weekdays and weekends?

The activity patters differ between weekends and midweek. This can be observe in the following plot.


```r
weekend <- c(weekdays(as.Date(c("19072014","20072014"),format="%d%m%Y")))
acdat2$weekday <- ifelse(weekdays(acdat2$date) %in% weekend, "weekend", "weekday")
acdat2$weekday <- as.factor(acdat2$weekday)
interAvg2 <- aggregate(acdat2$steps,by = list(acdat2$weekday,acdat2$interval),mean,na.rm=T)
ggplot(interAvg2, aes(Group.2,x)) + geom_line(color = "steelblue", size =1) + facet_wrap(~ Group.1,2) + xlab(label = "Interval") + ylab("Number fo steps")
```

![plot of chunk weekdays](figure/weekdays.png) 

In the weekday plot, the reader can observe a peak of activity on the morning but after certain hour that activity become more stable until the end of the day. On the other hand, the weekend plot shows how the activity is stable along the day without a marked peak.

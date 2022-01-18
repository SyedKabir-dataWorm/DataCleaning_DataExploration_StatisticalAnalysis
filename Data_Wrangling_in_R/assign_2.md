---
title: "University project: MATH2349 Data Wrangling"
author: "SYED NAZMUL KABIR"
subtitle: Data wrangling assignment
rmarkdown::github_document

---


## Required packages 


Here are the  packages used in this report. 


```r
# This is the R chunk for the required packages

# setwd("C:/Users/User/OneDrive/Desktop/assign2_wran") 
library(readr) 
library(dplyr)
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
library(magrittr)
library(tidyr) 
```

```
## 
## Attaching package: 'tidyr'
```

```
## The following object is masked from 'package:magrittr':
## 
##     extract
```

```r
library(ggplot2) 
library(knitr)
library(forecast)
```

```
## Registered S3 method overwritten by 'quantmod':
##   method            from
##   as.zoo.data.frame zoo
```

```r
library(infotheo)
```


## Executive Summary 

  Every year, many Australians lose their lives on roads. There are many factors causing road accidents. `Australian Road Death Database` provides invaluable information for finding out genuine causes of road accidents. Prior driving to the depth of analysis on root causes, it is of great importance to pre-process the data to get clean and tidier ones for further analysis. This report will emphasis on pre-processing activities of the data related to the road accidents in Australia.
  
  
  The initial pre-processing consists of importing two data-sets and joining those by using appropriate keys. The merged data-set is then inspected thoroughly to figure out the attributes and data types of all important variables following by necessary conversion of classes of variables.
  
  
  In middle stage, data is tidied up by consolidating various festival periods in to a single column. To facilitate the future analysis, a new column is being created from the existing data with a focus on illegal driving based on age.
  
  
  Next, data was thoroughly scanned for missing values including special values and inconsistencies. Imputation as well as removal techniques are used to neutralize those problems. 
  
  Later on, outliers for all numeric variables are detected using Turkey’s method. Reasons of choosing this particular method is also evaluated. Capping and removal techniques are being applied to get rid of those outliers. In all cases, problematic data are being replaced by clean, outlier free, capped data within the working data-set.  
  
  
  In final section, histogram of Age variable reflects the data nature of non-symmetric distribution. Different mathematical operations are applied as well as various transformation techniques are used to achieve normality characteristics of Age variable.
  

## Data 

* Data Souce:  data.gov.au

The page is :Australian Road Deaths Database (ARDD)

link: https://data.gov.au/data/dataset/5b530fb8-526e-4fbf-b0f6-aa24e84e4277, first viewed on 2nd of October 2020, latest viewed on 17th of January 2022.

* Two csv files named "ardd_fatalities" and "ardd_fatal_crashes" are the source data. Those are downloadable by just by clicking 'explore' option and then clicking 'Go to Resource' across both of "ARDD Fatalities December 2021" and "ARDD Fatal Crashes December 2021" options.These are live data, so the data is changing in every year.

* "ardd_fatalities" data is about fatalities: each record is a killed person in a road accident. The data set has 23 variables and 53129 observations (on December 2021). However this report will work with 12 out of those 23 variables.  

* "ardd_fatal_crashes" data is about Crashes: each record is a fatal crash which caused fatality or not. The data set has 20 variables and 47836 observations(on December 2021), however this report will work with only 2 out of those 20 variables. Because those are not necessary for this analysis.



### Variable Description:

    . Crash ID : National crash identifying number consisting of 8-characters, unique to each fatal crash.This is a common variable in both data-sets.
    
    . State: Australian Jurisdiction. A categorical variable representing abbreviation for each state/territory.
    
    . Month: An integer variable representing month of crash. 
    
    . Year: An integer variable representing year of crash.The data set covers the year from 1989 to 2020. However this report will work on years since 2015 to 2020
    
    . Crash Type: Imported as a Character variable.A crash is coded as a Pedestrian crash if a pedestrian was killed; otherwise, the number of vehicles involved.Three types of Observation: Pedestrian, Single, Multiple.
    
    . Speed Limit: An integer. Posted speed limit at location of crash.
    
    . Age: Integer varaiable. Age of killed person (by years)
    
    . Road User: A catagorical type variable representing the types of road user of killed person. The observations are of this types:Driver,Passenger, Pedestrian, Motorcycle rider, Motorcycle pillion passenger, Pedal cyclist(Note: includes pillion passenger).
    
    . Christmas Period: Character Type. Indicates if crash occurred during the 12 days commencing on December 23rd. Values are Yes/ No.
    
    . Easter Period: Character Type. Indicates if crash occurred during the 5 days commencing on the Thursday before Good Friday. Values are Yes/No.
    
    . Day of week: Character variable. Indicates if crash occurred during the weekday or weekend.(Note: ‘Weekday’ refers to 6am Monday through to 5:59pm Friday)
    
    . Time of day: Character variable. Indicates if crash occurred during the day or night (Note: ‘Day’ refers to 6am through to 5:59 pm).
    
    . Number Fatalities:Integer type. This variable is taken from second data set ("ardd_fatal_crashes"). Number of killed persons(fatalities) in the crash.
    
    . The remaining variables, superfluous to this analysis, were dropped after reading the file.
    

### Data Import, Joining Two Data Sets:

 This is the R chunk for the Data Section
 

```r
# Importing first data set from working directory.Then subsetting data selecting all rows and 12 number of columns.

# fatal_1<-read_csv("https://data.gov.au/data/dataset/5b530fb8-526e-4fbf-b0f6-aa24e84e4277/resource/fd646fdc-7788-4bea-a736-e4aeb0dd09a8/download/ardd_fatalities_dec2021.csv")

fatal_1 <- read_csv("ardd_fatalities.csv") # data retrieved on 2020
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_character(),
##   `Crash ID` = col_double(),
##   Month = col_double(),
##   Year = col_double(),
##   Time = col_time(format = ""),
##   `Speed Limit` = col_double(),
##   Age = col_double()
## )
## i Use `spec()` for the full column specifications.
```

```r
fatal<-fatal_1[,c("Crash ID","State","Month","Year" ,"Crash Type","Speed Limit","Road User","Christmas Period","Easter Period","Day of week" ,"Time of day","Age")]

head(fatal)
```

```
## # A tibble: 6 x 12
##   `Crash ID` State Month  Year `Crash Type` `Speed Limit` `Road User`
##        <dbl> <chr> <dbl> <dbl> <chr>                <dbl> <chr>      
## 1   20201111 NSW       8  2020 Single                  60 Passenger  
## 2   20203056 Qld       8  2020 Single                  50 Passenger  
## 3   20204015 SA        8  2020 Single                 110 Pedestrian 
## 4   20203066 Qld       8  2020 Single                  70 Passenger  
## 5   20207007 NT        8  2020 Single                 130 Driver     
## 6   20203017 Qld       8  2020 Multiple                60 Driver     
## # ... with 5 more variables: Christmas Period <chr>, Easter Period <chr>,
## #   Day of week <chr>, Time of day <chr>, Age <dbl>
```

```r
# Importing Second data set from working directory.Then subsetting data selecting all rows and 2 number of columns.

# crash_1<- read_csv("https://data.gov.au/data/dataset/5b530fb8-526e-4fbf-b0f6-aa24e84e4277/resource/d54f7465-74b8-4fff-8653-37e724d0ebbb/download/ardd_fatal_crashes_dec2021.csv")

crash_1 <- read_csv("ardd_fatal_crashes.csv")   # data retrieved on 2020
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_character(),
##   `Crash ID` = col_double(),
##   Month = col_double(),
##   Year = col_double(),
##   Time = col_time(format = ""),
##   `Number Fatalities` = col_double(),
##   `Speed Limit` = col_double()
## )
## i Use `spec()` for the full column specifications.
```

```r
crash<-crash_1[,c("Crash ID","Number Fatalities")]

head(crash)
```

```
## # A tibble: 6 x 2
##   `Crash ID` `Number Fatalities`
##        <dbl>               <dbl>
## 1   20205010                   1
## 2   20205030                   1
## 3   20201104                   1
## 4   20201105                   1
## 5   20201111                   1
## 6   20201184                   1
```

```r
# Joining `fatal` object with `crash` oject to produce the ultimate data set for this analysis. 
# Joined object is named as `fatal_crash`. Only the years since 2015 to 2020 is filtered for the analysis as the report will focus on latest data.

fatal_crash<-fatal%>%left_join (crash, by="Crash ID")%>%filter(Year==2015:2020)
head(fatal_crash)
```

```
## # A tibble: 6 x 13
##   `Crash ID` State Month  Year `Crash Type` `Speed Limit` `Road User`     
##        <dbl> <chr> <dbl> <dbl> <chr>                <dbl> <chr>           
## 1   20203017 Qld       8  2020 Multiple                60 Driver          
## 2   20203049 Qld       8  2020 Single                 100 Motorcycle rider
## 3   20207009 NT        8  2020 Single                 100 Passenger       
## 4   20204016 SA        8  2020 Single                  50 Motorcycle rider
## 5   20203060 Qld       8  2020 Multiple                80 Driver          
## 6   20203128 Qld       8  2020 Multiple                80 Driver          
## # ... with 6 more variables: Christmas Period <chr>, Easter Period <chr>,
## #   Day of week <chr>, Time of day <chr>, Age <dbl>, Number Fatalities <dbl>
```

## Understand 

The following chunk is to have a summarized view on types of variables and data structures. Data-set has 13 variables and 1129 observations. Classes of variables are numerical, character. Some variables are needed to be re-classified which will be done later.



```r
# This is the R chunk for Understanding variable types.
str(fatal_crash)
```

```
## tibble [1,129 x 13] (S3: tbl_df/tbl/data.frame)
##  $ Crash ID         : num [1:1129] 20203017 20203049 20207009 20204016 20203060 ...
##  $ State            : chr [1:1129] "Qld" "Qld" "NT" "SA" ...
##  $ Month            : num [1:1129] 8 8 8 8 8 8 8 8 8 8 ...
##  $ Year             : num [1:1129] 2020 2020 2020 2020 2020 2020 2020 2020 2020 2020 ...
##  $ Crash Type       : chr [1:1129] "Multiple" "Single" "Single" "Single" ...
##  $ Speed Limit      : num [1:1129] 60 100 100 50 80 80 50 80 60 100 ...
##  $ Road User        : chr [1:1129] "Driver" "Motorcycle rider" "Passenger" "Motorcycle rider" ...
##  $ Christmas Period : chr [1:1129] "No" "No" "No" "No" ...
##  $ Easter Period    : chr [1:1129] "No" "No" "No" "No" ...
##  $ Day of week      : chr [1:1129] "Weekend" "Weekend" "Weekend" "Weekday" ...
##  $ Time of day      : chr [1:1129] "Day" "Day" "Day" "Day" ...
##  $ Age              : num [1:1129] 21 49 7 24 41 72 78 39 27 27 ...
##  $ Number Fatalities: num [1:1129] 3 1 1 1 2 1 2 1 1 1 ...
##  - attr(*, "problems")= tibble [47 x 5] (S3: tbl_df/tbl/data.frame)
##   ..$ row     : int [1:47] 2371 4850 4851 8978 9716 9799 12802 14813 15199 15666 ...
##   ..$ col     : chr [1:47] "Speed Limit" "Speed Limit" "Speed Limit" "Speed Limit" ...
##   ..$ expected: chr [1:47] "a double" "a double" "a double" "a double" ...
##   ..$ actual  : chr [1:47] "<40" "Unspecified" "Unspecified" "<40" ...
##   ..$ file    : chr [1:47] "'ardd_fatalities.csv'" "'ardd_fatalities.csv'" "'ardd_fatalities.csv'" "'ardd_fatalities.csv'" ...
```



### Change of Class of Variables:

This chunk is for conversion of class of variables that is deemed necessary for the analysis.

```r
# Here Month variable is converted from numeric to a factor type variable; 
# performing levelling and labelling and finally ordered.
fatal_crash$Month<-factor(fatal_crash$Month,levels=c(1,2,3,4,5,6,7,8,9,10,11,12),
                    labels=c("Jan","Feb","Mar","Apr","May",
                             "Jun","Jul","Aug","Sep","Oct","Nov","Dec"), 
                    ordered=TRUE)

levels(fatal_crash$Month)
```

```
##  [1] "Jan" "Feb" "Mar" "Apr" "May" "Jun" "Jul" "Aug" "Sep" "Oct" "Nov" "Dec"
```

```r
# The following R codes are for converting class of Crash ID variables from numeric to character type.

fatal_crash$`Crash ID`<-as.character(fatal_crash$`Crash ID`)

# Road User variable is converted to factor type from charter type
fatal_crash$`Road User`<-factor (fatal_crash$`Road User`, 
                levels=c("Driver","Motorcycle rider", 
                         "Passenger","Motorcycle pillion passenger", 
                         "Pedestrian", "Pedal cyclist"  ))
# Crash Type variable is converted to factor type from charter type
fatal_crash$`Crash Type`<-factor(fatal_crash$`Crash Type`, levels=
                                   c("Multiple","Pedestrian","Single"))

#Changing level of "Yes" to "Christmass" while factorising `Christmas.Period` variable
fatal_crash$`Christmas Period`<-factor(fatal_crash$`Christmas Period`, level=
                                         c("No","Yes"), labels=
                                       c("No","Christmass"))
##Changing level of "Yes" to "Easter" while factorising `Easter.Period` variable
fatal_crash$`Easter Period` <-factor(fatal_crash$`Easter Period`, 
                                     level=c("No","Yes"), 
                                     labels=c("No","Easter"))
fatal_crash$`Number Fatalities`<-factor(fatal_crash$`Number Fatalities`, levels=c(1,2,3,4))
```
Confirming the change of classes


```r
class(fatal_crash$Month)
```

```
## [1] "ordered" "factor"
```

```r
class(fatal_crash$`Crash ID`)
```

```
## [1] "character"
```

```r
class(fatal_crash$`Road User`)
```

```
## [1] "factor"
```

```r
class(fatal_crash$`Crash Type`)
```

```
## [1] "factor"
```

```r
class(fatal_crash$`Number Fatalities` )
```

```
## [1] "factor"
```


Next chunk is showing head of the data  after performing appropriate conversion of classes of variables. This confirms again conversion of classes of the variables. 


```r
head(fatal_crash)
```

```
## # A tibble: 6 x 13
##   `Crash ID` State Month  Year `Crash Type` `Speed Limit` `Road User`     
##   <chr>      <chr> <ord> <dbl> <fct>                <dbl> <fct>           
## 1 20203017   Qld   Aug    2020 Multiple                60 Driver          
## 2 20203049   Qld   Aug    2020 Single                 100 Motorcycle rider
## 3 20207009   NT    Aug    2020 Single                 100 Passenger       
## 4 20204016   SA    Aug    2020 Single                  50 Motorcycle rider
## 5 20203060   Qld   Aug    2020 Multiple                80 Driver          
## 6 20203128   Qld   Aug    2020 Multiple                80 Driver          
## # ... with 6 more variables: Christmas Period <fct>, Easter Period <fct>,
## #   Day of week <chr>, Time of day <chr>, Age <dbl>, Number Fatalities <fct>
```


##	Tidy & Manipulate Data I 


Christmas.Period and Easter.Period are just few days in two separate months of a year.So value of those two column is "No" except christmas and easter time. They can be combined in a single variable named Festival.Thus the wide format of data is converted to longer format and more tidy.  

```r
# This is the R chunk for the Tidy & Manipulate Data I 
# Creating a new variable named "Festival Period" that will accomodate all the values of "Christmas Period" and "Easter Period" variable. 
# Finally Event variable has been dropped as it is no longer necessary for the analysis.
fatal_tidy<-fatal_crash%>%gather(key=Event,value=`Festival Period`,`Christmas Period`,`Easter Period`)%>%select(-"Event")
```

```
## Warning: attributes are not identical across measure variables;
## they will be dropped
```

```r
head(fatal_tidy)
```

```
## # A tibble: 6 x 12
##   `Crash ID` State Month  Year `Crash Type` `Speed Limit` `Road User`     
##   <chr>      <chr> <ord> <dbl> <fct>                <dbl> <fct>           
## 1 20203017   Qld   Aug    2020 Multiple                60 Driver          
## 2 20203049   Qld   Aug    2020 Single                 100 Motorcycle rider
## 3 20207009   NT    Aug    2020 Single                 100 Passenger       
## 4 20204016   SA    Aug    2020 Single                  50 Motorcycle rider
## 5 20203060   Qld   Aug    2020 Multiple                80 Driver          
## 6 20203128   Qld   Aug    2020 Multiple                80 Driver          
## # ... with 5 more variables: Day of week <chr>, Time of day <chr>, Age <dbl>,
## #   Number Fatalities <fct>, Festival Period <chr>
```


##	Tidy & Manipulate Data II 


Age range is important to be a legal-driver. Proper range of age is aprecondition to be a legal driver. Any under-aged or over-aged drivers should be illegal. Illegal-driving might be an important cause of accidents too. So a new variable is formed named as illegal_driver based on age.


```r
# This is the R chunk for the Tidy & Manipulate Data II 
# Creating a new variable named "Legal.driver" with three types of values: not_driver, Yes, No. 
# The new variable is generated from two existing variables: Age and `Road User`.
fatality<-fatal_tidy%>%
  mutate(Legal.driver=
           ifelse(fatal_tidy$`Road User`=="Driver",
             ifelse(fatal_tidy$Age<16|fatal_tidy$Age>75, 
                          Legal.driver<-"No",Legal.driver<-"Yes"),
                                 Legal.driver<-"not_driver"))
head(fatality)
```

```
## # A tibble: 6 x 13
##   `Crash ID` State Month  Year `Crash Type` `Speed Limit` `Road User`     
##   <chr>      <chr> <ord> <dbl> <fct>                <dbl> <fct>           
## 1 20203017   Qld   Aug    2020 Multiple                60 Driver          
## 2 20203049   Qld   Aug    2020 Single                 100 Motorcycle rider
## 3 20207009   NT    Aug    2020 Single                 100 Passenger       
## 4 20204016   SA    Aug    2020 Single                  50 Motorcycle rider
## 5 20203060   Qld   Aug    2020 Multiple                80 Driver          
## 6 20203128   Qld   Aug    2020 Multiple                80 Driver          
## # ... with 6 more variables: Day of week <chr>, Time of day <chr>, Age <dbl>,
## #   Number Fatalities <fct>, Festival Period <chr>, Legal.driver <chr>
```



##	Scan I 



```r
# This is the R chunk for the Scan I
# Checking every numerical column whether they have special cases like infinite or NaN or NA values by creating a function called is.specialorNA
is.specialorNA <- function(x){
if (is.numeric(x)) (is.infinite(x) | is.nan(x) | is.na(x))
}
# apply this function to the  fatality data frame.

sapply(fatality, function(y)sum(is.specialorNA(y)))
```

```
##          Crash ID             State             Month              Year 
##                 0                 0                 0                 0 
##        Crash Type       Speed Limit         Road User       Day of week 
##                 0                 2                 0                 0 
##       Time of day               Age Number Fatalities   Festival Period 
##                 0                 0                 0                 0 
##      Legal.driver 
##                 0
```
* So, Speed Limit variable has only two missing values.

* Thorough scanning of the data in the next chunk using following codes shows that speed.limit variable has 30 and age has 2 negative values which is impossible.  



```r
#Speed Limit, Number of fatalities and age can not be negative. So any negative value can be a potential missing value.
sum(fatality$`Speed Limit`<=0, na.rm = TRUE)
```

```
## [1] 30
```

```r
sum(fatality$Age<0, na.rm=TRUE)
```

```
## [1] 2
```
* Data shows that all negative values are "-9". So, these are also missing values because the description of data in original data source mentioned that missing values in data-sets are represented as "-9". 
* Moreover, `Road User` has some ""Other/-9" values which are also missing values. There are also few observations like :"Unspecified","Other/-9". These are all considered missing values.
* Thorough reading finds no other in-consistency in all the data-set.
* So converting these all missing value and inconsistency to NA using following codes.



```r
fatality[fatality=="-9"]<-NA

fatality[fatality=="<40"]<-NA

fatality$`Speed Limit`[fatality$`Speed Limit`=="Unspecified"]<-NA

fatality$`Road User`[fatality$`Road User`=="Other/-9"]<-NA
```

Now checking total NA values. 


```r
colSums(is.na(fatality))
```

```
##          Crash ID             State             Month              Year 
##                 0                 0                 0                 0 
##        Crash Type       Speed Limit         Road User       Day of week 
##                 0                32                 4                 0 
##       Time of day               Age Number Fatalities   Festival Period 
##                 0                 2                 0                 0 
##      Legal.driver 
##                 4
```
So, all missing values, special cases and inconsistencies are checked and are converted to NA.Let check the position of NA in the data. The following codes showing the rows only that have missing values.


```r
#Finding the exact location of missing values:

fatality[!complete.cases(fatality),]
```

```
## # A tibble: 38 x 13
##    `Crash ID` State Month  Year `Crash Type` `Speed Limit` `Road User`
##    <chr>      <chr> <ord> <dbl> <fct>                <dbl> <fct>      
##  1 20202052   Vic   Aug    2020 Single                  NA Driver     
##  2 20202005   Vic   Aug    2020 Single                  NA Driver     
##  3 20202013   Vic   Aug    2020 Single                  NA Pedestrian 
##  4 20202037   Vic   Jul    2020 Single                  NA Driver     
##  5 20205037   WA    Jul    2020 Pedestrian              NA Pedestrian 
##  6 20202069   Vic   Apr    2020 Multiple               100 Pedestrian 
##  7 20195054   WA    Apr    2019 Multiple                NA Driver     
##  8 20185027   WA    Aug    2018 Single                  NA Passenger  
##  9 20182086   Vic   Jun    2018 Multiple                NA Passenger  
## 10 20185017   WA    Apr    2018 Single                  NA Passenger  
## # ... with 28 more rows, and 6 more variables: Day of week <chr>,
## #   Time of day <chr>, Age <dbl>, Number Fatalities <fct>,
## #   Festival Period <chr>, Legal.driver <chr>
```

* Total number of missing value are less than 4% of the sample size. 
* However, the missing values for `Speed.limit` variables are many. 
* The summary statistics of this variables is showing that mean and median value are almost same. So , it is safe to convert NAs to mean value of this  variable to maintain data integrity.


```r
summary(fatality$`Speed Limit` , na.rm=TRUE)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
##    5.00   60.00   80.00   81.07  100.00  130.00      32
```

Replacing NAs of 'Speed Limit' variable by "mean" value of the column.


```r
fatality$`Speed Limit`[is.na(fatality$`Speed Limit`)]<-mean(fatality$`Speed Limit`,na.rm=TRUE)
colSums(is.na(fatality))
```

```
##          Crash ID             State             Month              Year 
##                 0                 0                 0                 0 
##        Crash Type       Speed Limit         Road User       Day of week 
##                 0                 0                 4                 0 
##       Time of day               Age Number Fatalities   Festival Period 
##                 0                 2                 0                 0 
##      Legal.driver 
##                 4
```

Now fatality data frame has only 10 observations containing missing values.
So, there is less than 1% NA in fatality data frame.We can remove those using the following code


```r
fatality<-fatality%>%na.omit()
colSums(is.na(fatality))
```

```
##          Crash ID             State             Month              Year 
##                 0                 0                 0                 0 
##        Crash Type       Speed Limit         Road User       Day of week 
##                 0                 0                 0                 0 
##       Time of day               Age Number Fatalities   Festival Period 
##                 0                 0                 0                 0 
##      Legal.driver 
##                 0
```
So all missing values are being handled appropriately.


##	Scan II


 Before choosing appropriate method of outlier-detection,we are checking normality of data.
  

```r
shapiro.test(fatality$Age)
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  fatality$Age
## W = 0.9656, p-value < 2.2e-16
```
  So data of Age variable is not normally distributed as p<.05.
  

```r
shapiro.test(fatality$`Speed Limit` )
```

```
## 
## 	Shapiro-Wilk normality test
## 
## data:  fatality$`Speed Limit`
## W = 0.8972, p-value < 2.2e-16
```
  So Values of Speed Limit variable is not normally distributed as p<.05. 
  
  As the numeric variables are not mormally distributed;  boxplot, “Tukey’s method of outlier detection” is used to detect outliers. Because we know that Tukey’s method is mainly used to test outliers in non-symmetric/ non-normal data distributions which suits with our data set.
  

```r
boxplot(fatality$`Speed Limit`, main=" Box Plot for Speed Limit", ylab = "Speed limit of road(km/hr)")
```

![](assign_2_revised_files/figure-html/unnamed-chunk-23-1.png)<!-- -->
    
    
  So, in univariate relationship, Speed Limit variable is not showing any outlier. Now a bi-variate relationship (speed limit and crash type) is being checked.
    

```r
p1 <- ggplot(data = fatality, aes(x = `Crash Type`, y = `Speed Limit` ))

p11<-p1 + geom_boxplot() +
labs( title = "Speed limit of the road for each type of crash caused fatalities",y = "Speed limit of road(km/hr)", x = "Types of Crash") +
coord_flip() + theme(legend.title=element_blank())

p11
```

![](assign_2_revised_files/figure-html/unnamed-chunk-24-1.png)<!-- -->



* So the above bi-variate relationship discovers few outliers for Pedestrian on Speed Limit variable.

* Capping method is used to replace those outliers with nearest neighbours that are not outliers. The capped data is then mutated with fatality dataframe replacing uncapped data. The following R-chunk is showing that:


```r
cap <- function(x){
 quantiles <- quantile( x, c(.05, 0.25, 0.75, .95 ) )
 x[ x < quantiles[2] - 1.5*IQR(x) ] <- quantiles[1]
 x[ x > quantiles[3] + 1.5*IQR(x) ] <- quantiles[4]
 x
}

fatality_1<-fatality%>%filter(`Crash Type`=="Pedestrian")

# Checking the minimum and maximum value before uncapping. the range is : 5-110 
range(fatality_1$`Speed Limit`,na.rm = TRUE)
```

```
## [1]   5 110
```

Capping on Speed Limit variable for only Pedestrians.


```r
speed_capped_1<-fatality_1$`Speed Limit`%>%cap()

# Checking the minimum and maximum value after capping. Now the range is : 25-100
range(speed_capped_1 ,na.rm = TRUE)
```

```
## [1]  25 100
```
So the range has been narrowed. Now injecting capped data in the fatality data frame by replacing uncapped data,



```r
# Replacing uncapped data from Speed Limit variable by capped data for only pedestrians.
fatality_2<-fatality_1%>%select(-`Speed Limit`)%>%mutate(`Speed Limit`=speed_capped_1)
# Removing uncapped data from main data frame.
speed_all<-fatality%>%filter(`Crash Type`!="Pedestrian")
# Combinding all capped data for pedestrians with the data of all other crash types to get a data frame consisting only capped data for pedestrians. 
fatality_capped<-bind_rows(speed_all,fatality_2)
```

Finally checking outliers again from the data frame that is containing relevant capped data.  



```r
p4 <- ggplot(data = fatality_capped, aes(x = `Crash Type`, y = `Speed Limit` ))

p5<-p4 + geom_boxplot() +
labs( title = "Speed limit of the road for each type of crash caused fatalities",y = "Speed limit of road(km/hr)", x = "Types of Crash") +
coord_flip() + theme(legend.title=element_blank())

p5
```

![](assign_2_revised_files/figure-html/unnamed-chunk-28-1.png)<!-- -->



* So, outliers of the  bivariate system  has been replaced by the neighboring values. No more outliers. 

* Now, checking the outliers for Age numeric variable. Again boxplot, “Tukey’s method of outlier detection” is used to detect outliers as data is not normally distributed.
  
  

```r
boxplot(fatality$Age, main=" Box plot of age of dead person in accidents", ylab = "Age(years)")
```

![](assign_2_revised_files/figure-html/unnamed-chunk-29-1.png)<!-- -->


* No outlier is showing for Age variable as a univariate system.
 
* But considering multivariate system, few outliers are existing in Age variable. Here I showed a multivariate (Age, Crash Type, Day of Week) systems.

 

```r
# This is the R chunk for the Scan II

p22 <- ggplot(data = fatality, aes(x = Age, y = `Crash Type` ))

p33<-p22 + geom_boxplot(aes(fill=`Day of week`)) +
labs( title = "Age of the killed person in different types of crashes",x = "Age(years)", y = "Types of Crash") + theme(legend.title=element_blank())

p33
```

![](assign_2_revised_files/figure-html/unnamed-chunk-30-1.png)<!-- -->



* As only 3 outliers are three, those can be removed manually in the following way: 
* Subsetting fatality data for weekend and Single type crash. 
* Then checking the summary statistics of Age variable of the subset data.


```r
fatality_2<-fatality%>%filter(`Day of week` =="Weekend",`Crash Type`=="Single")
summary(fatality_2$Age)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    0.00   22.00   32.00   36.51   47.00   99.00
```
* Now identifying corresponding fence of Age variable for the subsetted data. 
* Data beyond that fence are outliers and are being removed.
* Clean data has been introduced to the fatality data with a new object named fatality_final.


```r
outfence<- IQR(fatality_2$Age)*1.5+49
outlier<-fatality%>%filter(`Day of week` =="Weekend",`Crash Type`=="Single", Age>outfence )

fatality_final<-fatality%>%anti_join(outlier)
```

```
## Joining, by = c("Crash ID", "State", "Month", "Year", "Crash Type", "Speed Limit", "Road User", "Day of week", "Time of day", "Age", "Number Fatalities", "Festival Period", "Legal.driver")
```

```r
head(fatality_final)
```

```
## # A tibble: 6 x 13
##   `Crash ID` State Month  Year `Crash Type` `Speed Limit` `Road User`     
##   <chr>      <chr> <ord> <dbl> <fct>                <dbl> <fct>           
## 1 20203017   Qld   Aug    2020 Multiple                60 Driver          
## 2 20203049   Qld   Aug    2020 Single                 100 Motorcycle rider
## 3 20207009   NT    Aug    2020 Single                 100 Passenger       
## 4 20204016   SA    Aug    2020 Single                  50 Motorcycle rider
## 5 20203060   Qld   Aug    2020 Multiple                80 Driver          
## 6 20203128   Qld   Aug    2020 Multiple                80 Driver          
## # ... with 6 more variables: Day of week <chr>, Time of day <chr>, Age <dbl>,
## #   Number Fatalities <fct>, Festival Period <chr>, Legal.driver <chr>
```
Now final checking for outliers


```r
p2 <- ggplot(data = fatality_final, aes(x = Age, y = `Crash Type` ))

p3<-p2 + geom_boxplot(aes(fill=`Day of week`)) +
labs( title = "Age of the killed person in different crashes at various times",x = "Age(years)", y = "Types of Crash") + theme(legend.title=element_blank())

p3
```

![](assign_2_revised_files/figure-html/unnamed-chunk-33-1.png)<!-- -->



So, all old outliers are being removed. Two new outliers are being formed and that is quite normal. 


##	Transform 

Here  appropriate transformation  is applied on Age variables. We already know that in normal cartesian scale, Age data is not showing normality behaviour. The following histogram is confirming that. 



```r
hist(fatality_final $Age,col="grey",
 xlab="Age (years)", main="Histograme of Age variable without transformation", breaks=15)
```

![](assign_2_revised_files/figure-html/unnamed-chunk-34-1.png)<!-- -->


  So, the histogram is showing right skewness with very fat, long right tail. According to literature, log, natural log, square root, reciprocal, boxcox transformation should be effective to reduce the right skewness.



```r
log_fatal<-log10(fatality_final$Age)
ln_fatal<-log(fatality_final$Age)
sqrt_fatal<-sqrt(fatality_final$Age)
rec_fatal<-((fatality_final$Age)^(-1))
boxcox_fatality<- BoxCox(fatality_final$Age, lambda = "auto")
```

```
## Warning in guerrero(x, lower, upper): Guerrero's method for selecting a Box-Cox
## parameter (lambda) is given for strictly positive data.
```


```r
par(mfrow=c(3,2))

hist(fatality_final $Age,col="grey",
 xlab="Age (years)", main="Histograme of Age variable without transformation", breaks=15)

hist(log_fatal,xlab="Age (years)", main="Histograme of Age with log transformation", breaks=15)

hist(ln_fatal,xlab="Age (years)", main="Histograme of Age with natural log transformation", breaks=15)

hist(sqrt_fatal,  xlab="Age (years)", main="Histograme of Age with square root transformation", breaks=15)

hist(rec_fatal, xlab="Age (years)", main="Histograme of Age with reciprocal transformation", breaks=15)

boxcox_fatality<- BoxCox(fatality_final$Age, lambda = "auto")
```

```
## Warning in guerrero(x, lower, upper): Guerrero's method for selecting a Box-Cox
## parameter (lambda) is given for strictly positive data.
```

```r
hist(boxcox_fatality, xlab="Age (years)", main="Histograme of Age with BoxCox transformation", breaks=15)
```

![](assign_2_revised_files/figure-html/unnamed-chunk-36-1.png)<!-- -->

```r
par(mfrow=c(1,1))
```

Unfortunately, all of this transformations can not remove the non-normal nature of the data, rather some transformations generate more skewness. 

Actually, Age is a discrete type variable. Specific range of age may play important role in accidents. Road accidents caused by younger aged people is a matter of great attention. Driving of old-aged person is also a matter of concern. So discretising this numeric variable using binning method shall be more helpful for further analysis (i.e. Naive Bayes and CHAID analysis).

Let’s apply equal-width binning to the Age variable using infotheo package.


```r
fatal_age<-fatality_final%>%select(Age)
ew_binned<-discretize(fatal_age, disc = "equalwidth")
```
Now showing the head of first 15 observations of equal binned.


```r
fatal_age %>% bind_cols(ew_binned) %>% head(15)
```

```
## New names:
## * Age -> Age...1
## * Age -> Age...2
```

```
## # A tibble: 15 x 2
##    Age...1 Age...2
##      <dbl>   <int>
##  1      21       3
##  2      49       7
##  3       7       1
##  4      24       4
##  5      41       6
##  6      72      10
##  7      78      11
##  8      39       6
##  9      27       4
## 10      27       4
## 11      57       8
## 12      81      11
## 13      31       5
## 14       2       1
## 15      81      11
```

## Beyond this report

As this report is limited to few packages, that is why effective transformation using bestNormalize package is not shown in main discussion. This is shown here:



```r
library(bestNormalize)

BN_obj_1<-bestNormalize(fatality_final$Age)
```

```r
BN_obj_1
```

```
## Best Normalizing transformation with 2248 Observations
##  Estimated Normality Statistics (Pearson P / df, lower => more normal):
##  - arcsinh(x): 4.6569
##  - Center+scale: 3.7109
##  - Exp(x): 246.63
##  - Log_b(x+a): 9.7384
##  - orderNorm (ORQ): 1.2471
##  - sqrt(x + a): 3.033
##  - Yeo-Johnson: 2.7676
## Estimation method: Out-of-sample via CV with 10 folds and 5 repeats
##  
## Based off these, bestNormalize chose:
## orderNorm Transformation with 2248 nonmissing obs and ties
##  - 97 unique values 
##  - Original quantiles:
##    0%   25%   50%   75%  100% 
##  0.00 25.00 42.00 62.25 98.00
```



```r
hist(BN_obj_1$x.t,xlab="Age (years)", main="Histograme of Age with best normalisation transformation", breaks=15)
```

![](assign_2_revised_files/figure-html/unnamed-chunk-40-1.png)<!-- -->

  So, data of Age variable has been transformed that generates almost symmetrical distribution using bestNormalize package. 


## Reference

Australian Government (2020) *data.gov.au* Australian Road Deaths Database(ARDD)

  Retrieved October 02,2020 from data.gov.au website

https://data.gov.au/data/dataset/5b530fb8-526e-4fbf-b0f6-aa24e84e4277

<br>
<br>

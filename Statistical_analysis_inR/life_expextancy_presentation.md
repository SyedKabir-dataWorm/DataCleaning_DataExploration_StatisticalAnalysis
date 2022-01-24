---
title: "Comparing life expectancy in between developing and developed countries"
author: "Syed Kabir"
date: "1/25/2022"
output: 
  slidy_presentation:
    keep_md : true
    font_adjustment: 0
    highlight: haddock
    mathjax: "default"
    df_print: paged
      

---




 
# Introduction

R-pub link: https://rpubs.com/kabir_data_worm/680654

- Life expectancy refers to a measure describing how long a person would like to live.

- Previous studies show that life expectancy varies among the countries worldwide due to different socioeconomic and health factors (Khan, Khan, & Khan, 2010; Lin, Chen, Chien, & Chan, 2012).

-  A better understanding of the difference in life expectancy in developing and developed countries is of great importance for international and global development.

- In spite of poor health facilities and lower standard of living and education level; the life expectancy in developing countries was increased 3.6 years per decade compare to 1.8 years per decade in developed countries (United Nations, 2017).

- So it is of great interest to know the present condition of developing and developed countries in terms of life expectancy. 


# Problem Statement

- This particular analysis is undertaken to investigate whether the life expectancy in developed countries differs from developing countries or not. 
- In order to address this research question, the data on life expectancy of developed and developing countries have been used over a period of five years. Data is collected from Kaggle. 
- Initially, descriptive statistics and visualizations on important variables are used to search out the impact of development condition of countries on life expectancy parameter.
- Then, all required assumptions are being checked to carry out appropriate hypothesis test
- Next, Hypothesis testing is used to find out any statistically significant difference in life expectancy in between developed and developing countries.
- Finally, a solid discussion is provided to highlight on limitations, strength, future study and ultimate results of the investigation.




# Data

- Data is collected by clicking on the "Download" tab of the webpage of following link: https://www.kaggle.com/kumarajarshi/life-expectancy-who?select=Life+Expectancy+Data.csv 
- The sampling method of the data is not mentioned in data source. 
- The "Life Expectancy Data.csv" data-set contains observations for 193 countries covering the time period from 2000-2015 with 22 variables and 2938 observations.
- However this analysis will work on four variables: Country,Year, Status, Life expectancy. The analysis will cover the years from 2011 to 2015.
- "Country" variable is catagorical type representing each country of the world.
- "Status" is nominal type variable with two categories: Developed, Developing. This variable divides country variable as developed and developing countries.
- "Life expectancy" is ratio type. Each value represents average life length of the people of a particular country. 



# Data cont.
- At first, importing data from working directory with necessary sub-setting and filtering. 
- Checking the attributes of data-frame and class of each variable.


```r
life_expectancy<-read_csv("Life Expectancy Data.csv")%>% select(Country,Year,Status, `Life expectancy`)%>%filter(Year==c(2011:2015))
str(life_expectancy)
```

```
## tibble [185 x 4] (S3: tbl_df/tbl/data.frame)
##  $ Country        : chr [1:185] "Afghanistan" "Albania" "Algeria" "Angola" ...
##  $ Year           : num [1:185] 2013 2011 2014 2012 2015 ...
##  $ Status         : chr [1:185] "Developing" "Developing" "Developing" "Developing" ...
##  $ Life expectancy: num [1:185] 59.9 76.6 75.4 56 76.4 76 73.9 82.7 88 72.7 ...
```
- Changing class of "Status" variable to a factor and defining the levels. 

```r
life_expectancy$Status<-factor(life_expectancy$Status,levels=c("Developing","Developed"))
levels(life_expectancy$`Status`)
```

```
## [1] "Developing" "Developed"
```


# Data Cont.(Handling Missing Values)
- Checking missing values: NA, nan, special values. Output is showing 2 NAs in Life expectancy. 

```r
is.specialorNA <- function(x){if (is.numeric(x)) (is.infinite(x) | is.nan(x) | is.na(x))}
sapply(life_expectancy, function(y)sum(is.specialorNA(y)))
```

```
##         Country            Year          Status Life expectancy 
##               0               0               0               2
```
- Finding location of missing values. Removing 2 missing values as sample size for developing countries is large(153).

```r
life_expectancy[!complete.cases(life_expectancy),]
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["Country"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Year"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Status"],"name":[3],"type":["fct"],"align":["left"]},{"label":["Life expectancy"],"name":[4],"type":["dbl"],"align":["right"]}],"data":[{"1":"Nauru","2":"2013","3":"Developing","4":"NA"},{"1":"Saint Kitts and Nevis","2":"2013","3":"Developing","4":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
life_expectancy<-life_expectancy%>%na.omit()
```

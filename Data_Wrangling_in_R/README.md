The purpose of this assignment is to do data wrangling and do appropriate data transformation.

The datasets are taken for this task is a real data. 
Data Souce: data.gov.au 
The page is :Australian Road Deaths Database (ARDD) 
link: https://data.gov.au/data/dataset/5b530fb8-526e-4fbf-b0f6-aa24e84e4277, viewed on 2nd of October,2020.


## Minimum Requirements for the Data sets for this task
 The following are the minimum requirements for the data sets on which data wrangling is needed to be done:
1. At least two data sets should be merged to create new data (for
example you can take crime statistics for the cities/states in Australia and merge this
data set with cities/states’ per capita income data).
2. Initial data set should include multiple data types (numerics, characters, factors, etc).
3. Initia data set should include variables suitable for data type conversions so that one
should be able to apply the required data type conversions (e.g., character ->
factor, character -> date, numeric -> factor, etc. conversions).
4. Initia data set should include at least one factor variable that needs to be labelled
and/or ordered.
5. At least one of the data sets should be Untidy. You need to explain
why the data set or data sets you used is/are Untidy. Then you need to apply the
required steps to reshape your data into a tidy format.
6. At least one variable needs to be created/mutated fro m the existing ones (e.g.
the data may contain income and expense variables and you may create a savings
variable out of the income and expense variables).
7. You are expected to scan all variables for missing values, special values and
obvious errors (i.e. inconsistencies). If there are missing values, use any of the
suitable techniques. 
8. You are expected to scan all numeric variables for outliers. If there are outliers,
use any of the suitable techniques outlined in Module 6 to deal with them, reason
and document your approach properly.
9. You are expected to apply data transformations on at least one of the variables.
The purpose of this transformation should be one of the following reasons: i) to
change the scale for better understanding of the variable, ii) to convert a non-linear
relation into linear one, or iii) to decrease the skewness and convert the distribution
into a normal distribution.
10. You are expected to use only readr, xlsx, readxl, foreign, gdata, rvest, dplyr, tidyr,
deductive, deducorrect, editrules, validate, Hmisc, forecast, stringr, lubridate, car,
outliers, MVN, infotheo, MASS, caret, MLR , ggplot2, knitr and base R functions for
this section. You can also use your own functions. This will show your accumulated
knowledge that you gained throughout the semester in this course.

## Report Section Details covering following activities:

1. Report title and student details [Plain text] : You can add the title of your report
and student details by updating the “title” and “author” entries at the top of the R
Markdown Template.
2. Required packages [R code]: Provide the packages required to reproduce the
report. Make sure you fulfilled the minimum requirement #10.
3. Executive Summary [Plain text] : In your own words, provide a brief summary of the
preprocessing. Explain the steps that you have taken to preprocess your data. Write
this section last after you have performed all data preprocessing. (Word count Max:
300 words).
4. Data [Plain text & R code & Output]: A clear description of data sets, their sources,
and variable descriptions should be provided. In this section, you must also provide
the R codes with outputs (e.g. head of data sets) that you used to import/read/scrape
the data set. You need to fulfil the minimum requirement #1 and merge at least two
data sets to create the one you are going to work on. In addition to the R codes and
outputs, you need to explain the steps that you have taken.
5. Understand [Plain text & R code & Output]: Summarise the types of variables and
data structures, check the attributes in the data and apply proper data type
conversions. In addition to the R codes and outputs, explain briefly the steps that you
have taken. In this section, show that you have fulfilled minimum requirements 2-4.
6. Tidy & Manipulate Data I [Plain text & R code & Output]: Explain why your data
(or one of the data sets) doesn’t conform the tidy data principles (minimum
requirement #5). Apply the required steps to reshape the data into a tidy format. In
addition to the R codes and outputs, explain everything that you do in this step.
7. Tidy & Manipulate Data II [Plain text & R code & Output]: Create/mutate at least
one variable from the existing variables (minimum requirement #6). In addition to the
R codes and outputs, explain everything that you do in this step.
8. Scan I [Plain text & R code & Output]: Scan the data for missing values, special
values and obvious errors (i.e. inconsistencies). In this step, you should fulfil the
minimum requirement #7. In addition to the R codes and outputs, explain your
methodology (i.e. explain why you have chosen that methodology and the actions
that you have taken to handle these values) and communicate your results clearly.
9. Scan II [Plain text & R code & Output]: Scan the numeric data for outliers. In this
step, you should fulfil the minimum requirement #8. In addition to the R codes and
outputs, explain your methodology (i.e. explain why you have chosen that
methodology and the actions that you have taken to handle these values) and
communicate your results clearly.
10. Transform [Plain text & R code & Output]: Apply an appropriate transformation for
at least one of the variables. In addition to the R codes and outputs, explain
everything that you do in this step. In this step, you should fulfil the minimum
requirement #9.

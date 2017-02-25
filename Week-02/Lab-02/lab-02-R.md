---
title: "SOC 4650/5650: Lab-02"
author: "Christopher Prener, Ph.D."
date: "25 Feb 2017"
output: html_notebook
---

## Description
This R Notebook file illustrates basic data exploration functions in R using data on lakes listed by the State of Missouri under the Clean Water Act.

## Assumptions
This R Notebook assumes that you have your data organized in a single `lab-02` directory. The notebook should be saved in this directory, which is also where your R project file should be saved and where html output that will be generated on each save will be located.

`lab-02` directory should have two sub-directories:
*   `Output` - save your knit markdown file here along with your cleaned output tables
*   `RawData` - the raw data should be saved here

## Dependencies
This R Notebook uses functions from the `dplyr` and `broom` packages, which are part of the `tidyverse`. We also use the `knit` package, which is another R Studio maintained package for R. The `library()` function is used to load packages.


```r
library(dplyr)
library(broom)
library(knitr)
```

## Question 11

### 11a

Data need to be loaded into a type of `R` object called a "data frame". We give the object a name, in this case `impairedLakes`, and use the assignment operator `<-` to feed data into it.


```r
impairedLakes <- read.csv("RawData/MO_HYDRO_ImpairedLakes.csv", stringsAsFactors = FALSE)
```

The `read.csv` function, which is part of base `R`, loads the specified `.csv` file from your working directory and places it in the `impairedLakes` data frame. Remember that, when you use an RStudio project, your working directory will be automatically set. Also recall that this function is used specifically for .csv files. If you want to read other types of files, you will need to use specific functions for them. Finally, remember to set `stringsAsFactors = FALSE` to avoid any unplanned or unwanted changes to string data.

### 11b


```r
str(impairedLakes)
```

```
## 'data.frame':	86 obs. of  28 variables:
##  $ YR        : int  2014 2006 2014 2012 2012 2016 2006 2014 2002 2016 ...
##  $ BUSINESSID: int  60348 59691 54686 51313 51312 60247 60020 55954 60036 60037 ...
##  $ WBID      : num  7309 7365 7003 7003 7003 ...
##  $ WATER_BODY: chr  "Bee Tree Lake" "Belcher Branch Lake" "Bowling Green Lake - Old" "Bowling Green Lake - Old" ...
##  $ WB_CLS    : chr  "L3" "L3" "L1" "L1" ...
##  $ MDNR_IMPSZ: num  10 42 7 7 7 ...
##  $ SIZE_     : num  10 42 7 7 7 ...
##  $ EPA_APPRSZ: num  10 42 7 7 7 ...
##  $ UNIT      : chr  "Acres" "Acres" "Acres" "Acres" ...
##  $ POLLUTANT : chr  "Mercury in Fish Tissue (T)" "Mercury in Fish Tissue (T)" "Chlorophyll-a (W)" "Nitrogen, Total (W)" ...
##  $ SOURCE_   : chr  "Atmospheric Deposition - Toxics" "Atmospheric Deposition - Toxics" "Rural NPS" "Rural NPS" ...
##  $ IU        : chr  "HHP" "HHP" "AQL" "AQL" ...
##  $ OU        : chr  "AQL, IRR, LWW, SCR, WBC B" "AQL, IRR, LWW, SCR, WBC B" "DWS, IRR, LWW, SCR, WBC B, HHP" "DWS, IRR, LWW, SCR, WBC B, HHP" ...
##  $ DWN_X     : num  732843 351264 658498 658497 658502 ...
##  $ DWN_Y     : num  4254646 4382887 4356565 4356565 4356562 ...
##  $ COUNTY_U_D: chr  "St. Louis" "Buchanan" "Pike" "Pike" ...
##  $ WB_EPA    : chr  "Bee Tree Lake" "Belcher Branch Lake" "Bowling Green Lake - Old" "Bowling Green Lake - Old" ...
##  $ COMMENT_  : chr  "1" "1" "1, 4*" "1, 4*" ...
##  $ PERM_ID   : chr  "{33B32820-E019-41A1-9CF7-9956A70E34C9}" "{71DF0578-7733-4C7D-8090-B5141D828E36}" "{46944D6C-A9D1-492F-AE85-2DC7C29198AF}" "{46944D6C-A9D1-492F-AE85-2DC7C29198AF}" ...
##  $ EVENTDAT  : chr  "2012-09-20" "2011-09-29" "2011-09-29" "2011-09-29" ...
##  $ REACHCODE : num  7.14e+12 NA 7.11e+12 7.11e+12 7.11e+12 ...
##  $ RCHSMDATE : logi  NA NA NA NA NA NA ...
##  $ RCH_RES   : logi  NA NA NA NA NA NA ...
##  $ SRC_DESC  : chr  "Table G" "Table G" "Table G" "Table G" ...
##  $ FEAT_URL  : logi  NA NA NA NA NA NA ...
##  $ SHAPE_Leng: num  1351 2976 3077 3077 3077 ...
##  $ Shape_Le_1: num  1351 2976 3077 3077 3077 ...
##  $ Shape_Area: num  40219 181659 113134 113134 113134 ...
```

The `str()` function, which is part of base `R`, returns data types as well as sample values for each object in your data frame. There are 13 variables classified as `chr` or "charater".

### 11c


```r
class(impairedLakes$YR)
```

```
## [1] "integer"
```

We can use the `class()` function, which is part of base `R`, to identify the type of data that a variable contains. In this case, the variable `YR` contains integer data.


```r
summary(impairedLakes$YR)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    2002    2008    2012    2011    2016    2016
```

The `summary()` function, which is part of base `R`, gives us some basic descriptive statistics about the variable `yr`. These include the minimum and maximum values, which can be used to calculate range. They also include the first and third quartiles, which can be used to calculate interquartile range. Finally, they include the median (50th percentile) and the mean (the average).

The average year that a body of water was listed was 2010. The earliest a body of water was added was 2002. The latest a body of water was added was 2016. Therefore, there is a range of fourteen years. 


```r
yearStats <- tidy(summary(impairedLakes$YR))
yearStats
```

```
##   minimum   q1 median mean   q3 maximum
## 1    2002 2008   2012 2011 2016    2016
```

If we wrap the `summary()` function in the `tidy()` function, which is part of the `broom` package, our data can be neatly saved into a data frame. Here, we've named that data frame `yearStats`. This makes it easy to refer to values later, extract them for further use, or export them to a `.csv` file.

### 11d


```r
class(impairedLakes$WATER_BODY)
```

```
## [1] "character"
```

The `WATER_BODY` variable contains character data.


```r
waterTable <- 
  tidy(table(impairedLakes$WATER_BODY)) %>%
  arrange(desc(Freq))
head(waterTable)
```

```
##                       Var1 Freq
## 1          Table Rock Lake    6
## 2              Forest Lake    4
## 3           Hunnewell Lake    4
## 4                Lake Paho    4
## 5           Weatherby Lake    4
## 6 Bowling Green Lake - Old    3
```

Now, we use the `dplyr` package to "pipe" a number of functions into the object `waterTable`. This allows us to chain commands together, shortening the amount of code we have to write and making that code easier to read. Remember to read the pipe symbol as a synonym for "then". 

In this case, we create a table of the variable `WATER_BODY` and store that table's output in a data frame, *then* we re-arrange the data frame so that it is sorted with the largest values for the variable `Freq` in the first rows. The results of this process are assigned to the object `waterTable`, which we can then call using the `head()` function (which by default gives us the first six rows of the data frame) to identify the modal category, which is "Table Rock Lake".

### 11e


```r
class(impairedLakes$SOURCE_)
```

```
## [1] "character"
```

The `SOURCE_` variable contains character data.


```r
sourceTable <- tidy(table(impairedLakes$SOURCE_))
sourceTable
```

```
##                                                 Var1 Freq
## 1                    Atmospheric Deposition - Toxics   58
## 2 Municipal Point Source Discharges, Nonpoint Source    6
## 3                                    Nonpoint Source    3
## 4                                          Rural NPS   12
## 5                                     Source Unknown    3
## 6                           Terre du Lac Subdivision    1
## 7                          Urban Runoff/Storm Sewers    3
```

The most common source of pollutants is atmospheric deposition. Since there are so few categories in this variable, we do not need to worry about sorting its values like we did in the last question.

### 11f


```r
impairedLakes %>%
  select(YR, WATER_BODY, POLLUTANT) %>%
  head(10)
```

```
##      YR               WATER_BODY                  POLLUTANT
## 1  2014            Bee Tree Lake Mercury in Fish Tissue (T)
## 2  2006      Belcher Branch Lake Mercury in Fish Tissue (T)
## 3  2014 Bowling Green Lake - Old          Chlorophyll-a (W)
## 4  2012 Bowling Green Lake - Old        Nitrogen, Total (W)
## 5  2012 Bowling Green Lake - Old      Phosphorus, Total (W)
## 6  2016        Buffalo Bill Lake Mercury in Fish Tissue (T)
## 7  2006   Busch W.A. No. 35 Lake Mercury in Fish Tissue (T)
## 8  2014          Clearwater Lake          Chlorophyll-a (W)
## 9  2002          Clearwater Lake Mercury in Fish Tissue (T)
## 10 2016          Clearwater Lake      Phosphorus, Total (W)
```

We can use the same piping procedure to extract a select number of columns and observations. This is useful when exploring data to get a snapshot of what, say, the first ten observations look like. We take the `impairedLakes` data frame *then* extract the variables `YR`, `WATER_BODY`, and `POLLUTANT` *then* we print the first ten rows.

## Final Steps
We can export our three tidy sets of output as well using the `write.csv()` function.


```r
write.csv(sourceTable, "Output/lab-02-sourceTable.csv", na="")
write.csv(waterTable, "Output/lab-02-waterTable.csv", na="")
write.csv(yearStats, "Output/lab-02-yearStats.csv", na="")
```

Finally, we'll export our our output. We can process this R Notebook so that all of the commands, output, and narrative are "woven" together into a single Markdown file that GitHub can display. We use the `knit()` function from the `knitr` package to do this. We have to specify both the input and output files.


```r
knit('lab-02-R.Rmd', "Output/lab-02-R.md")
```

```
## 
## 
## processing file: lab-02-R.Rmd
```

```
## output file: Output/lab-02-R.md
```



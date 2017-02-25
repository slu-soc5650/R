---
title: "SOC 4650/5650: Lab-03"
author: "Christopher Prener, Ph.D."
date: "25 Feb 2017"
output: html_notebook
---

## Description
This R Notebook file illustrates basic data cleaning functions in R using data on rivers and streams listed by the State of Missouri under the Clean Water Act.

## Assumptions
This R Notebook assumes that you have your data organized in a single `lab-03` directory. The notebook should be saved in this directory, which is also where your R project file should be saved and where html output that will be generated on each save will be located.

`lab-03` directory should have three sub-directories:
*   `CleanData` - your final clean dataset should be saved here
*   `Output` - save your knit markdown file here along with your cleaned output tables
*   `RawData` - the raw data should be saved here

## Dependencies
This R Notebook uses functions from the `dplyr` and `broom` packages, which are part of the `tidyverse`. We also use the `knit` package, which is another R Studio maintained package for R. The `library()` function is used to load packages.


```r
library(dplyr)
library(broom)
library(knitr)
```

## Question 7

### 7a


```r
impairedStreams <- read.csv("RawData/MO_HYDRO_ImpairedRiversStreams.csv", stringsAsFactors = FALSE)
```

This function loads the data from the raw `.csv` file into a data frame named `impairedStreams`.

### 7b

We'll use the `dplyr` function `select()` to remove unneeded variables from the data frame:


```r
impairedStreams <- 
  impairedStreams %>%
  select(-BUSINESSID, -MDNR_IMPSZ, -EPA_APPRSZ, -UNIT, -WB_EPA, -COMMENT_, -EVENTDAT, 
         -REACHCODE, -RCHSMDATE, -RCH_RES, -SRC_DESC, -FEAT_URL, -FMEASURE, -TMEASURE, 
         -SHAPE_Leng, -Shape_Le_1)
```

Note how we use the `-` (minus sign) before each variable to specify that we want that variable *removed* from the data frame. We could also use the `select()` function to keep variables by listing variables without the minus sign. In that senario, any listed variables would be kept while all unlisted variables would be discarded. Each approach comes down to (typically) what requires less typing - writing out everything you want to keep, or everything you want to discard.

### 7c


```r
impairedStreamsSTL <-
  impairedStreams %>%
  filter(COUNTY_U_D == "St. Louis")
```

The `filter()` function is used to subset data. Here, we subset all of the data that lists St. Louis County as the exclusive county where the polluted body of water is and pipe it into a new object called `impairedStreamsSTL`.

### 7d


```r
impairedStreamsSTL %>%
  distinct(WBID)
```

```
##    WBID
## 1  2188
## 2  3825
## 3  1701
## 4  1781
## 5  1706
## 6  1703
## 7  1704
## 8  3595
## 9  2186
## 10 1842
## 11 2184
## 12 1713
## 13 3592
## 14 3596
## 15 2183
## 16 1700
## 17 3594
## 18 3972
## 19 4051
## 20 5007
## 21 4079
## 22 4097
## 23 4098
```

Note that the `distinct(WBID)` function returns output with 23 rows. This indicates that, out of the 179 observations in `impairedStreamsSTL`, there are 23 distinct values for `WBID`. Therefore, `WBID` does not uniquely identify observations in our data frame.

#### 7e


```r
impairedStreamsSTL %>%
  distinct(PERM_ID)
```

```
##                                   PERM_ID
## 1  {29525EE5-C8C7-4E38-A898-B6806F8B31BC}
## 2  {387C7A17-63E7-4147-8695-24DDA8915775}
## 3  {BFBBA3B4-D8A8-4DA8-9D0B-30104D79A79D}
## 4  {349DADA4-7AFB-44F6-BD56-51FE8D9644FE}
## 5  {CF03D888-9C16-48C4-B78A-7176F11F090A}
## 6  {680AE34A-A46F-4CDC-BF0E-08DA3E8D6FB9}
## 7  {D68F5CFB-8F3F-4B62-B05D-850FB41FBA0F}
## 8  {62FCEDDC-4A60-4AAA-B396-1E50C04AB7AC}
## 9  {EE1B350E-3277-40A4-932B-6BD153E85D08}
## 10 {4FDE6F9C-F79C-4BC4-8A21-54A4F272F33E}
## 11 {BB1C2E0A-6F76-45AA-B623-9C4EF23316C0}
## 12 {CEE5577A-C9EF-4266-8BB6-E6ABA694B07D}
## 13 {4D132A85-F5FB-49B3-AF02-C60F8EEBF3B0}
## 14 {C56ED959-10B9-44F8-B0D8-3D6BF72FC55D}
## 15 {ED87A117-23A9-49FC-99B5-23B41723FFE5}
## 16 {6216135D-7FB7-4F3B-A594-CEDDC1AAA56F}
## 17 {0B5063BC-00FD-4975-814A-4BD48E1836FD}
## 18 {EEC63D56-054D-47D9-9235-09EAA48D685C}
## 19 {CBBC6C16-794E-4B2C-A552-17443B84D4C3}
## 20 {B4FDB7F4-185B-411E-8197-CD787B244D06}
## 21 {2B89AB15-6388-468D-BA66-4531E8976725}
## 22 {0BEDE6B9-9BA1-434C-B9BD-5B6A0E2D8C49}
## 23 {D5DB8788-0336-482E-AD30-F66E8F0A97B1}
## 24 {8B9E9844-B5CD-44BF-AA5D-3058585A9028}
## 25 {E7232DB6-8F20-4E66-9E1C-4061204A0300}
## 26 {DF744A34-0B42-4B51-A955-CA84FD49FB8B}
## 27 {E1C41CC7-6A4D-4E6C-9E0A-E37D711425E3}
## 28 {B32DF6EC-A64D-4E9C-9106-D984F370F81E}
## 29 {73F8A477-C985-4903-B0B7-185568955414}
## 30 {A3F8B26E-97E8-46DF-8947-ADB402DDB825}
## 31 {A9BD5370-40A2-4C64-8243-7FDAB99E7107}
## 32 {D2D4331E-6099-497E-AA56-93B873D1E88D}
## 33 {7C2341DA-174B-46F0-8505-C35C22C3661D}
## 34 {DD4E5792-74B8-4EA4-B15A-36897FBF9A7B}
## 35 {F05BB3D9-E31A-4360-8D62-0E21A512A725}
## 36 {F9A8E27D-BE19-4FB4-8FDD-D26CE6D833E7}
## 37 {3AE200BC-B16E-431B-A560-197207F292E8}
## 38 {7BAB2D5B-6114-418D-B2D7-7DEC2B8C9C63}
## 39 {CB0E9371-B210-4D04-9DC2-1CBF1017F718}
## 40 {D0C44980-44F9-4ED6-B792-B1EFFB585A79}
## 41 {80D99CFF-02E6-465F-9848-BED0D9FE8AC6}
## 42 {5A8DCD75-5A82-4EEA-B521-4D5237DBC6B3}
## 43 {2C6D7A06-9EA0-4CC1-AC98-B046E68F9177}
## 44 {F09FF66A-9F38-4675-95F4-98CBEB77D32A}
## 45 {726C4485-9ED8-4E4A-AC2A-7367BC819984}
## 46 {666A2E58-FD88-4551-84B4-C87EC1B012CE}
## 47 {7FF4AB75-0F9E-44B1-840D-5CD2DE85337F}
## 48 {A9763999-0057-4E24-B216-F3CF3CD5BF54}
## 49 {3CF5DF1C-DDDB-471A-BFA6-6ECD7F614DA2}
## 50 {E312F8E9-743F-4DD2-8835-CE137B79D443}
## 51 {E908B828-A3AA-4AFF-8287-1549968BFADB}
## 52 {FBAF40AB-E341-48A7-82E8-AF9BFD809C95}
## 53 {F8831A76-3440-4EDA-964A-EE9841FD4D8D}
## 54 {E6B874E6-96FB-4274-B934-DF9812484EE8}
## 55 {4C0066E9-3E43-40EF-9594-FF4D9B4605FF}
## 56 {252B26AE-E96C-4F22-9B84-3BDE1D3C4C99}
## 57 {4C7744D9-23A7-42A8-AB29-F7E125CF1A57}
## 58 {19021829-58EA-4E20-ABDE-A061978B8139}
## 59 {92F48E2D-363B-4604-B7E7-FBF7C6618C58}
## 60 {3C862C61-9BC2-4E0F-B800-D256AAF8ED95}
## 61 {595820C2-4D86-4B31-B168-05D7C4526A21}
## 62 {39F3C4CE-F83C-4F8C-B84D-72635DB0C9F5}
## 63 {2809878B-45DF-45B4-8EEE-19EB3670FCFD}
## 64 {1E2BD1CE-9AE6-4F6D-9711-F1897D5A5288}
## 65 {AB8E7E5C-2C53-4C8A-A1F4-1B5EF631EE7B}
## 66 {F578AB75-9EC2-43C0-A43D-D0CB7EA7B29C}
## 67 {44435135-2D54-4042-B68C-8F9B260C746B}
## 68 {B363237F-1DE4-48E0-94AE-9FE965A9D7BF}
## 69 {494CAD5C-74E8-430A-88EE-714C59A5F9F7}
## 70 {120AE41B-A6B2-4D43-81DF-B3CE0565621B}
## 71 {D845EEA4-BFBA-47AE-B5F3-E3889706BB75}
## 72 {967B6A7B-5684-47B1-B780-60DC57BD2832}
## 73 {DCD1D64F-3C01-456D-B512-ECD754D4A182}
## 74 {7F6ADCC3-19B0-470F-AC7B-1BCBA2693DC3}
## 75 {C3CE649B-070C-4124-A8E1-F90136F401D2}
## 76 {FC8E6E86-E12C-48AF-A134-2171306389D3}
## 77 {E245FB70-A8C9-404A-A0C1-AD6F68B92BCA}
## 78
```

The output indicates that, out of the 179 observations in `impairedStreamsSTL`, there are 78 distinct values for `PERM_ID`. Therefore, `PERM_ID` does not uniquely identify observations in our data frame.

#### 7f

```r
tidy(table(impairedStreamsSTL$PERM_ID))
```

```
##                                      Var1 Freq
## 1                                           32
## 2  {0B5063BC-00FD-4975-814A-4BD48E1836FD}    2
## 3  {0BEDE6B9-9BA1-434C-B9BD-5B6A0E2D8C49}    3
## 4  {120AE41B-A6B2-4D43-81DF-B3CE0565621B}    2
## 5  {19021829-58EA-4E20-ABDE-A061978B8139}    2
## 6  {1E2BD1CE-9AE6-4F6D-9711-F1897D5A5288}    2
## 7  {252B26AE-E96C-4F22-9B84-3BDE1D3C4C99}    2
## 8  {2809878B-45DF-45B4-8EEE-19EB3670FCFD}    2
## 9  {29525EE5-C8C7-4E38-A898-B6806F8B31BC}    2
## 10 {2B89AB15-6388-468D-BA66-4531E8976725}    2
## 11 {2C6D7A06-9EA0-4CC1-AC98-B046E68F9177}    1
## 12 {349DADA4-7AFB-44F6-BD56-51FE8D9644FE}    2
## 13 {387C7A17-63E7-4147-8695-24DDA8915775}    2
## 14 {39F3C4CE-F83C-4F8C-B84D-72635DB0C9F5}    2
## 15 {3AE200BC-B16E-431B-A560-197207F292E8}    1
## 16 {3C862C61-9BC2-4E0F-B800-D256AAF8ED95}    2
## 17 {3CF5DF1C-DDDB-471A-BFA6-6ECD7F614DA2}    2
## 18 {44435135-2D54-4042-B68C-8F9B260C746B}    2
## 19 {494CAD5C-74E8-430A-88EE-714C59A5F9F7}    2
## 20 {4C0066E9-3E43-40EF-9594-FF4D9B4605FF}    2
## 21 {4C7744D9-23A7-42A8-AB29-F7E125CF1A57}    2
## 22 {4D132A85-F5FB-49B3-AF02-C60F8EEBF3B0}    1
## 23 {4FDE6F9C-F79C-4BC4-8A21-54A4F272F33E}    1
## 24 {595820C2-4D86-4B31-B168-05D7C4526A21}    2
## 25 {5A8DCD75-5A82-4EEA-B521-4D5237DBC6B3}    1
## 26 {6216135D-7FB7-4F3B-A594-CEDDC1AAA56F}    2
## 27 {62FCEDDC-4A60-4AAA-B396-1E50C04AB7AC}    1
## 28 {666A2E58-FD88-4551-84B4-C87EC1B012CE}    3
## 29 {680AE34A-A46F-4CDC-BF0E-08DA3E8D6FB9}    2
## 30 {726C4485-9ED8-4E4A-AC2A-7367BC819984}    3
## 31 {73F8A477-C985-4903-B0B7-185568955414}    3
## 32 {7BAB2D5B-6114-418D-B2D7-7DEC2B8C9C63}    1
## 33 {7C2341DA-174B-46F0-8505-C35C22C3661D}    2
## 34 {7F6ADCC3-19B0-470F-AC7B-1BCBA2693DC3}    1
## 35 {7FF4AB75-0F9E-44B1-840D-5CD2DE85337F}    3
## 36 {80D99CFF-02E6-465F-9848-BED0D9FE8AC6}    1
## 37 {8B9E9844-B5CD-44BF-AA5D-3058585A9028}    3
## 38 {92F48E2D-363B-4604-B7E7-FBF7C6618C58}    2
## 39 {967B6A7B-5684-47B1-B780-60DC57BD2832}    1
## 40 {A3F8B26E-97E8-46DF-8947-ADB402DDB825}    3
## 41 {A9763999-0057-4E24-B216-F3CF3CD5BF54}    3
## 42 {A9BD5370-40A2-4C64-8243-7FDAB99E7107}    2
## 43 {AB8E7E5C-2C53-4C8A-A1F4-1B5EF631EE7B}    2
## 44 {B32DF6EC-A64D-4E9C-9106-D984F370F81E}    3
## 45 {B363237F-1DE4-48E0-94AE-9FE965A9D7BF}    2
## 46 {B4FDB7F4-185B-411E-8197-CD787B244D06}    2
## 47 {BB1C2E0A-6F76-45AA-B623-9C4EF23316C0}    1
## 48 {BFBBA3B4-D8A8-4DA8-9D0B-30104D79A79D}    2
## 49 {C3CE649B-070C-4124-A8E1-F90136F401D2}    1
## 50 {C56ED959-10B9-44F8-B0D8-3D6BF72FC55D}    1
## 51 {CB0E9371-B210-4D04-9DC2-1CBF1017F718}    1
## 52 {CBBC6C16-794E-4B2C-A552-17443B84D4C3}    2
## 53 {CEE5577A-C9EF-4266-8BB6-E6ABA694B07D}    1
## 54 {CF03D888-9C16-48C4-B78A-7176F11F090A}    2
## 55 {D0C44980-44F9-4ED6-B792-B1EFFB585A79}    1
## 56 {D2D4331E-6099-497E-AA56-93B873D1E88D}    2
## 57 {D5DB8788-0336-482E-AD30-F66E8F0A97B1}    3
## 58 {D68F5CFB-8F3F-4B62-B05D-850FB41FBA0F}    2
## 59 {D845EEA4-BFBA-47AE-B5F3-E3889706BB75}    2
## 60 {DCD1D64F-3C01-456D-B512-ECD754D4A182}    1
## 61 {DD4E5792-74B8-4EA4-B15A-36897FBF9A7B}    2
## 62 {DF744A34-0B42-4B51-A955-CA84FD49FB8B}    3
## 63 {E1C41CC7-6A4D-4E6C-9E0A-E37D711425E3}    3
## 64 {E245FB70-A8C9-404A-A0C1-AD6F68B92BCA}    1
## 65 {E312F8E9-743F-4DD2-8835-CE137B79D443}    2
## 66 {E6B874E6-96FB-4274-B934-DF9812484EE8}    2
## 67 {E7232DB6-8F20-4E66-9E1C-4061204A0300}    3
## 68 {E908B828-A3AA-4AFF-8287-1549968BFADB}    2
## 69 {ED87A117-23A9-49FC-99B5-23B41723FFE5}    1
## 70 {EE1B350E-3277-40A4-932B-6BD153E85D08}    1
## 71 {EEC63D56-054D-47D9-9235-09EAA48D685C}    2
## 72 {F05BB3D9-E31A-4360-8D62-0E21A512A725}    2
## 73 {F09FF66A-9F38-4675-95F4-98CBEB77D32A}    3
## 74 {F578AB75-9EC2-43C0-A43D-D0CB7EA7B29C}    2
## 75 {F8831A76-3440-4EDA-964A-EE9841FD4D8D}    2
## 76 {F9A8E27D-BE19-4FB4-8FDD-D26CE6D833E7}    2
## 77 {FBAF40AB-E341-48A7-82E8-AF9BFD809C95}    2
## 78 {FC8E6E86-E12C-48AF-A134-2171306389D3}    1
```

The first row, which is blank for `Var1`, indicates the number of observations that are missing data for the `PERM_ID` variable. There are 32 observations with missing data here.

#### 7g

```r
impairedStreamsSTL <- 
  impairedStreamsSTL %>%
  select(-PERM_ID)
```

Since the `PERM_ID` is of little value to us right now, we'll drop it from the data frame.

#### 7h


```r
impairedStreamsSTL <-
  impairedStreamsSTL %>%
  rename(source = SOURCE_)
```

This code chunk pipes a renamed variable into our existing data frame.

#### 7i


```r
impairedStreamsSTL <-
  impairedStreamsSTL %>%
  rename(county = COUNTY_U_D)
```

This code chunk pipes a second renamed variable into our existing data frame.

#### 7j


```r
impairedStreamsSTL %>%
  distinct()
```

```
##      YR WBID              WATER_BODY WB_CLS SIZE_
## 1  2012 2188              Antire Cr.      P   1.9
## 2  2012 2188              Antire Cr.      P   1.9
## 3  2006 3825               Black Cr.      P   1.6
## 4  2012 3825               Black Cr.      P   1.6
## 5  2012 1701            Bonhomme Cr.      C   2.5
## 6  2012 1701            Bonhomme Cr.      C   2.5
## 7  2012 1781              Antire Cr.      P   1.9
## 8  2006 1706           Coldwater Cr.      C   6.9
## 9  2016 1706           Coldwater Cr.      C   6.9
## 10 2006 1703         Creve Coeur Cr.      C   3.8
## 11 2006 1703         Creve Coeur Cr.      C   3.8
## 12 2006 1703         Creve Coeur Cr.      C   3.8
## 13 2012 1704       Fee Fee Cr. (new)      P   1.5
## 14 2012 1704       Fee Fee Cr. (new)      P   1.5
## 15 2016 3595              Fenton Cr.      P   0.5
## 16 2012 3595              Fenton Cr.      P   0.5
## 17 2012 2186             Fishpot Cr.      P   3.5
## 18 2008 2186             Fishpot Cr.      P   3.5
## 19 2012 1842                 Fox Cr.      P   7.2
## 20 2006 2184        Grand Glaize Cr.      C   4.0
## 21 2008 2184        Grand Glaize Cr.      C   4.0
## 22 2002 2184        Grand Glaize Cr.      C   4.0
## 23 2006 1713             Gravois Cr.      C   6.0
## 24 2006 1713             Gravois Cr.      C   6.0
## 25 2012 3592              Keifer Cr.      P   1.2
## 26 2012 3592              Keifer Cr.      P   1.2
## 27 2014 3596             Mattese Cr.      P   1.1
## 28 2014 3596             Mattese Cr.      P   1.1
## 29 2016 2183              Meramec R.      P  22.8
## 30 2008 2183              Meramec R.      P  22.8
## 31 2012 1700           Wildhorse Cr.      C   3.9
## 32 2012 3594            Williams Cr.      P   1.0
## 33 2006 3972         River des Peres     US  13.6
## 34 2016 4051 Gravois Creek tributary      C   1.9
## 35 2016 3972         River des Peres      C  13.6
## 36 2016 5007           Spring Branch      C   3.1
## 37 2016 4079           Twomile Creek      C   5.6
## 38 2016 4097 Watkins Creek tributary      C   1.2
## 39 2016 4098 Watkins Creek tributary      C   1.2
##                                       POLLUTANT
## 1                          Escherichia coli (W)
## 2                                        pH (W)
## 3                                  Chloride (W)
## 4                          Escherichia coli (W)
## 5                          Escherichia coli (W)
## 6                                        pH (W)
## 7                                        pH (W)
## 8                                  Chloride (W)
## 9                          Escherichia coli (W)
## 10                                 Chloride (W)
## 11                         Escherichia coli (W)
## 12                        Oxygen, Dissolved (W)
## 13                                 Chloride (W)
## 14                         Escherichia coli (W)
## 15                                 Chloride (W)
## 16                         Escherichia coli (W)
## 17                                 Chloride (W)
## 18                         Escherichia coli (W)
## 19 Aquatic Macroinvertebrate Bioassessments/ U*
## 20                                 Chloride (W)
## 21                         Escherichia coli (W)
## 22                   Mercury in Fish Tissue (T)
## 23                                 Chloride (W)
## 24                         Escherichia coli (W)
## 25                                 Chloride (W)
## 26                         Escherichia coli (W)
## 27                                 Chloride (W)
## 28                         Escherichia coli (W)
## 29                         Escherichia coli (W)
## 30                                     Lead (S)
## 31                         Escherichia coli (W)
## 32                         Escherichia coli (W)
## 33                                 Chloride (W)
## 34                         Escherichia coli (W)
## 35                         Escherichia coli (W)
## 36                         Escherichia coli (W)
## 37                         Escherichia coli (W)
## 38                         Escherichia coli (W)
## 39                         Escherichia coli (W)
##                                                               source
## 1                                          Urban Runoff/Storm Sewers
## 2                                                     Source Unknown
## 3                                          Urban Runoff/Storm Sewers
## 4                                          Urban Runoff/Storm Sewers
## 5                                          Urban Runoff/Storm Sewers
## 6                                                     Source Unknown
## 7                                                     Source Unknown
## 8                                          Urban Runoff/Storm Sewers
## 9                                          Urban Runoff/Storm Sewers
## 10                                         Urban Runoff/Storm Sewers
## 11                                         Urban Runoff/Storm Sewers
## 12                                                    Source Unknown
## 13                                         Urban Runoff/Storm Sewers
## 14                                         Urban Runoff/Storm Sewers
## 15                                                    Source Unknown
## 16                                         Urban Runoff/Storm Sewers
## 17                                         Urban Runoff/Storm Sewers
## 18                                         Urban Runoff/Storm Sewers
## 19                                                    Source Unknown
## 20                                         Urban Runoff/Storm Sewers
## 21                                         Urban Runoff/Storm Sewers
## 22                                   Atmospheric Deposition - Toxics
## 23                                         Urban Runoff/Storm Sewers
## 24                                         Urban Runoff/Storm Sewers
## 25                              Road/Bridge Runoff, Non-construction
## 26                                                         Rural NPS
## 27                                         Urban Runoff/Storm Sewers
## 28                                         Urban Runoff/Storm Sewers
## 29                                                    Source Unknown
## 30                                            Old Lead belt tailings
## 31                                          Rural, Residential Areas
## 32                                                         Rural NPS
## 33                                         Urban Runoff/Storm Sewers
## 34 Municipal, Urbanized High Density Area, Urban Runoff/Storm Sewers
## 35                                         Urban Runoff/Storm Sewers
## 36                                                    Source Unknown
## 37                                         Urban Runoff/Storm Sewers
## 38                                         Urban Runoff/Storm Sewers
## 39                                         Urban Runoff/Storm Sewers
##            IU                                  OU     UP_X    UP_Y
## 1       WBC B             AQL, IRR, LWW, SCR, HHP 712453.5 4264477
## 2         AQL           IRR, LWW, SCR, WBC B, HHP 712453.5 4264477
## 3         AQL           IRR, LWW, SCR, WBC B, HHP 731266.1 4278180
## 4  WBC B, SCR                  AQL, IRR, LWW, HHP 731266.1 4278180
## 5       WBC B             AQL, IRR, LWW, SCR, HHP 709511.6 4282258
## 6         AQL           IRR, LWW, SCR, WBC B, HHP 709511.6 4282258
## 7         AQL           IRR, LWW, SCR, WBC B, HHP 712453.5 4264477
## 8         AQL      IND, IRR, LWW, SCR, WBC B, HHP 735013.7 4299849
## 9  WBC B, SCR             AQL, IND, IRR, LWW, HHP 741425.3 4301794
## 10        AQL           IRR, LWW, SCR, WBC B, HHP 718172.4 4283167
## 11      WBC B             AQL, IRR, LWW, SCR, HHP 718172.4 4283167
## 12        AQL           IRR, LWW, SCR, WBC B, HHP 718172.4 4283167
## 13        AQL           IRR, LWW, SCR, WBC B, HHP 720613.0 4290506
## 14 WBC B, SCR                  AQL, IRR, LWW, HHP 720613.0 4290506
## 15        AQL           IRR, LWW, SCR, WBC B, HHP 724629.0 4265304
## 16      WBC B             AQL, IRR, LWW, SCR, HHP 723864.9 4265429
## 17        AQL           IRR, LWW, SCR, WBC B, HHP 715610.6 4270777
## 18      WBC B             AQL, IRR, LWW, SCR, HHP 715610.6 4270777
## 19        AQL           IRR, LWW, SCR, WBC B, HHP 698955.6 4266805
## 20        AQL           IRR, LWW, SCR, WBC B, HHP 720446.7 4272244
## 21      WBC B             AQL, IRR, LWW, SCR, HHP 720446.7 4272244
## 22        HHP           AQL, IRR, LWW, SCR, WBC B 721056.0 4270200
## 23        AQL           IRR, LWW, SCR, WBC B, HHP 731100.9 4269870
## 24      WBC B             AQL, IRR, LWW, SCR, HHP 731100.9 4269870
## 25        AQL           IRR, LWW, SCR, WBC A, HHP 713475.2 4270033
## 26      WBC A             AQL, IRR, LWW, SCR, HHP 713475.2 4270033
## 27        AQL           IRR, LWW, SCR, WBC B, HHP 733139.1 4260643
## 28      WBC B             AQL, IRR, LWW, SCR, HHP 733139.1 4260643
## 29      WBC A   AQL, DWS, IND, IRR, LWW, SCR, HHP 718255.5 4269401
## 30        AQL DWS, IND, IRR, LWW, SCR, WBC A, HHP 718255.5 4269401
## 31      WBC B             AQL, IRR, LWW, SCR, HHP 699002.4 4276141
## 32 WBC B, SCR                  AQL, IRR, LWW, HHP 716803.6 4268162
## 33        AQL           IRR, LWW, SCR, WBC B, HHP 731228.4 4283838
## 34      WBC B             AQL, IRR, LWW, SCR, HHP 727152.9 4269299
## 35 WBC B, SCR                  AQL, IRR, LWW, HHP 731229.5 4283832
## 36      WBC B             AQL, IRR, LWW, SCR, HHP 711578.7 4270614
## 37      WBC B             AQL, IRR, LWW, SCR, HHP 721591.6 4277889
## 38 WBC B, SCR                  AQL, IRR, LWW, HHP 740624.9 4297157
## 39 WBC B, SCR                  AQL, IRR, LWW, HHP 743158.2 4295677
##       DWN_X   DWN_Y    county
## 1  710076.6 4264450 St. Louis
## 2  710076.6 4264450 St. Louis
## 3  732023.5 4276834 St. Louis
## 4  732023.5 4276834 St. Louis
## 5  711491.0 4284301 St. Louis
## 6  711491.0 4284301 St. Louis
## 7  710076.6 4264450 St. Louis
## 8  741449.3 4301962 St. Louis
## 9  735013.7 4299849 St. Louis
## 10 718454.7 4287491 St. Louis
## 11 718454.7 4287491 St. Louis
## 12 718454.7 4287491 St. Louis
## 13 718639.4 4290795 St. Louis
## 14 718639.4 4290795 St. Louis
## 15 723864.9 4265429 St. Louis
## 16 724629.0 4265304 St. Louis
## 17 718255.5 4269401 St. Louis
## 18 718255.5 4269401 St. Louis
## 19 702112.5 4258893 St. Louis
## 20 721056.0 4270200 St. Louis
## 21 721056.0 4270200 St. Louis
## 22 720446.7 4272244 St. Louis
## 23 735408.2 4269269 St. Louis
## 24 735408.2 4269269 St. Louis
## 25 714844.8 4269588 St. Louis
## 26 714844.8 4269588 St. Louis
## 27 732308.0 4259650 St. Louis
## 28 732308.0 4259650 St. Louis
## 29 731939.2 4252470 St. Louis
## 30 732150.3 4252184 St. Louis
## 31 699384.2 4279922 St. Louis
## 32 716671.7 4269382 St. Louis
## 33 734090.3 4282681 St. Louis
## 34 729315.7 4270942 St. Louis
## 35 734090.8 4282681 St. Louis
## 36 713448.9 4270031 St. Louis
## 37 728707.5 4277778 St. Louis
## 38 741049.1 4295353 St. Louis
## 39 742995.4 4294040 St. Louis
```

The output indicates that, out of the 179 observations in `impairedStreamsSTL`, there are 39 distinct rows.

#### 7k


```r
impairedStreamsSTL <-
  impairedStreamsSTL %>%
  distinct()
```

There were 140 observations dropped after taking the syntax from question **7j** and assigning the results to the `impairedStreamsSTL` data frame.

#### 7l
If you were to scroll through the table, you'll notice that "Fee Fee Creek" is named `Fee Fee Cr. (new)` in the variable `WATER_BODY`. This is the only body of water that has this characteristic. 


```r
impairedStreamsSTL$WATER_BODY <-
  ifelse(impairedStreamsSTL$WATER_BODY == "Fee Fee Cr. (new)", "Fee Fee Cr.", 
         impairedStreamsSTL$WATER_BODY)
streamTable <- tidy(table(impairedStreamsSTL$WATER_BODY))
streamTable
```

```
##                       Var1 Freq
## 1               Antire Cr.    3
## 2                Black Cr.    2
## 3             Bonhomme Cr.    2
## 4            Coldwater Cr.    2
## 5          Creve Coeur Cr.    3
## 6              Fee Fee Cr.    2
## 7               Fenton Cr.    2
## 8              Fishpot Cr.    2
## 9                  Fox Cr.    1
## 10        Grand Glaize Cr.    3
## 11             Gravois Cr.    2
## 12 Gravois Creek tributary    1
## 13              Keifer Cr.    2
## 14             Mattese Cr.    2
## 15              Meramec R.    2
## 16         River des Peres    2
## 17           Spring Branch    1
## 18           Twomile Creek    1
## 19 Watkins Creek tributary    2
## 20           Wildhorse Cr.    1
## 21            Williams Cr.    1
```

Here, we use the `ifelse()` function from base `R` to evaluate each observation in the `impairedStreamsSTL` data frame. If the evaluation is TRUE (i.e. the observation's value for `WATER_BODY` is `Fee Fee Cr. (new)`), the value for that observation's `WATER_BODY` is changed to `Fee Fee Cr.`. If the evaluation is FALSE (i.e. the observation's value for `WATER_BODY` is anything else), the original text of `WATER_BODY` is retained. 

We can see the results when a table summarizing the number of bodies of water is stored as the object `streamTable` and printed.

## Final Steps
We can export our tidy sets of output and data using the `write.csv()` function.


```r
write.csv(impairedStreamsSTL, "CleanData/lab-03-impairedStreamsSTL.csv", na="")
write.csv(streamTable, "Output/lab-03-streamTable.csv", na="")
```

Finally, we'll export our our output. We can process this R Notebook so that all of the commands, output, and narrative are "woven" together into a single Markdown file that GitHub can display. We use the `knit()` function from the `knitr` package to do this. We have to specify both the input and output files.


```r
knit('lab-03-R.Rmd', "Output/lab-03-R.md")
```

```
## 
## 
## processing file: lab-03-R.Rmd
```

```
## output file: Output/lab-03-R.md
```

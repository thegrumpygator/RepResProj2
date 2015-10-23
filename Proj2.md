# Health and Economic Impact of Storm Damage
Brian L. Fuller  
October 20, 2015  

Synopsis
-----

Your data analysis must address the following questions:

1. Across the United States, which types of events (as indicated in the EVTYPE
variable) are most harmful with respect to population health?
2. Across the United States, which types of events have the greatest economic
consequences?



Data Processing
-----

I'll begin by downloading the bz2 file if necessary and reading directly from
it. Based on the questions to be answered, I will not need all of the columns. 
I will specifically need event type (EVTYPE) and the columns which denote
population health (23 FATALITIES, 24 INJURIES) and economic consequence (25 PROPDMG,
26 PROPDMGEXP, 27 CROPDMG, and 28 CROPDMGEXP). I will also include several other
columns (7 STATE, 5 COUNTY, 6 COUNTYNAME, 2 BGN_DATE) to aid with analysis.


```r
library(readr)

URL <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
DestFile = "repdata_data_StormData.csv.bz2"

# download bz2 version of input data file if needed, and unzip
if(!file.exists(DestFile)){
     library(R.utils) 
     dlMethod <- "curl"
     if(substr(Sys.getenv("OS"),1,7) == "Windows") dlMethod <- "wininet"
     download.file(URL,destfile = DestFile, method = dlMethod, mode = "wb")
     ##bunzip2(DestFile, "StormData.csv")
}
```

```
## Loading required package: R.oo
## Loading required package: R.methodsS3
## R.methodsS3 v1.7.0 (2015-02-19) successfully loaded. See ?R.methodsS3 for help.
## R.oo v1.19.0 (2015-02-27) successfully loaded. See ?R.oo for help.
## 
## Attaching package: 'R.oo'
## 
## The following objects are masked from 'package:methods':
## 
##     getClasses, getMethods
## 
## The following objects are masked from 'package:base':
## 
##     attach, detach, gc, load, save
## 
## R.utils v2.1.0 (2015-05-27) successfully loaded. See ?R.utils for help.
## 
## Attaching package: 'R.utils'
## 
## The following object is masked from 'package:utils':
## 
##     timestamp
## 
## The following objects are masked from 'package:base':
## 
##     cat, commandArgs, getOption, inherits, isOpen, parse, warnings
```

```r
myData <- read_csv(DestFile,
                   n_max = 902298, col_names = TRUE,
                   col_types = "_c__nccc______________nnncnc_______c_")
```

In order to make the date column (BGN_DATE) useful, convert character date
representation to an actual date representation.

```r
myData$BgnDate <- as.Date(myData$BGN_DATE, "%m/%d/%Y")
```

This is what our slightly conditioned dataset looks like

```r
head(myData)
```

```
##             BGN_DATE COUNTY COUNTYNAME STATE  EVTYPE FATALITIES INJURIES
## 1  4/18/1950 0:00:00     97     MOBILE    AL TORNADO          0       15
## 2  4/18/1950 0:00:00      3    BALDWIN    AL TORNADO          0        0
## 3  2/20/1951 0:00:00     57    FAYETTE    AL TORNADO          0        2
## 4   6/8/1951 0:00:00     89    MADISON    AL TORNADO          0        2
## 5 11/15/1951 0:00:00     43    CULLMAN    AL TORNADO          0        2
## 6 11/15/1951 0:00:00     77 LAUDERDALE    AL TORNADO          0        6
##   PROPDMG PROPDMGEXP CROPDMG CROPDMGEXP REMARKS    BgnDate
## 1    25.0          K       0       <NA>    <NA> 1950-04-18
## 2     2.5          K       0       <NA>    <NA> 1950-04-18
## 3    25.0          K       0       <NA>    <NA> 1951-02-20
## 4     2.5          K       0       <NA>    <NA> 1951-06-08
## 5     2.5          K       0       <NA>    <NA> 1951-11-15
## 6     2.5          K       0       <NA>    <NA> 1951-11-15
```

Results
-----







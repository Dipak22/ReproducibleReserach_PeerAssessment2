## **Analysis of different storm types to find it's effect on Health and Economy.**

 **Synopsis**

This document explains the consequences of different storm types on the human health and US economy.
Here we have explored the U.S. National Oceanic and Atmospheric Administration's (NOAA) storm database. This database tracks characteristics of major storms and weather events in the United States, including when and where they occur, as well as estimates of any fatalities, injuries, and property damage.
In this document we present an analysis of the last 60 years storms that occured in the United States. We highlight storm types that have the highest human and economic consequences.


 **Data Processing**


```r


## Here we are reading the .csv provided from the Assignment link.

## First download the .bz2 file from the link.

## unzip data
library(R.utils)
```

```
## Loading required package: R.oo
## Loading required package: R.methodsS3
## R.methodsS3 v1.6.1 (2014-01-04) successfully loaded. See ?R.methodsS3 for help.
## R.oo v1.18.0 (2014-02-22) successfully loaded. See ?R.oo for help.
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
## R.utils v1.32.4 (2014-05-14) successfully loaded. See ?R.utils for help.
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
fileData <- "repdata_data_StormData.csv"
bunzip2(filename = "repdata_data_StormData.csv.bz2", destname = fileData, overwrite = TRUE)

# read data
data <- read.csv(fileData, header = TRUE, sep = ",")

```


We have separated the different storms into two groups one which has caused casualties in the past and the ones which have not caused any harm until now.



```r

EvntCasualties <- aggregate(cbind(data$INJURIES, data$FATALITIES) ~ data$EVTYPE, 
    data, sum)

## Events consists of storms type which has resulted in Injuries/Fatalities.

NoCasualties <- EvntCasualties[, 2] == 0 & EvntCasualties[, 3] == 0
Events <- EvntCasualties[!NoCasualties, ]
names(Events) <- cbind("type", "injuries", "fatalities")

```



**Results**

1.Across the United States, which types of events (as indicated in the EVTYPE variable) are most harmful        with respect to population health?


2. Across the United States, which types of events have the greatest economic consequences?




```r
library(ggplot2)
library(reshape2)

Storm_Injuries <- order(Events[, 2], decreasing = T)
Storm_Fatalities <- order(Events[, 3], decreasing = T)

# extracting the top 10 storms.

storm_plot <- melt(Events[Storm_Fatalities, ][1:10, ], "type")

## Plot showing the effect of different storms on human life.

ggplot(storm_plot, aes(x = type, y = value, fill = variable)) + geom_bar(position = "dodge", 
    stat = "identity") + theme(axis.text.x = element_text(angle = 60, hjust = 1)) + 
    labs(title = "Top 10 fatal storm types", x = "Storm type", y = " TotalNo.ofpeople injured and killed")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

```r

## Below list shows the storms which have proved to be destructive for US
## Economy.

events <- sort(unique(data$EVTYPE[data$PROPDMGEXP == "B" | data$CROPDMGEXP == 
    "B"]))
print(events)
```

```
##  [1] DROUGHT                    FLASH FLOOD               
##  [3] FLOOD                      FREEZE                    
##  [5] HAIL                       HEAT                      
##  [7] HEAVY RAIN/SEVERE WEATHER  HIGH WIND                 
##  [9] HURRICANE                  HURRICANE OPAL            
## [11] HURRICANE OPAL/HIGH WINDS  HURRICANE/TYPHOON         
## [13] ICE STORM                  RIVER FLOOD               
## [15] SEVERE THUNDERSTORM        STORM SURGE               
## [17] STORM SURGE/TIDE           TORNADO                   
## [19] TORNADOES, TSTM WIND, HAIL TROPICAL STORM            
## [21] WILD/FOREST FIRE           WILDFIRE                  
## [23] WINTER STORM              
## 985 Levels:    HIGH SURF ADVISORY  COASTAL FLOOD ... WND
```


**Please get the below details in case of reproducing it.**

The complete detail can be found here : https://github.com/Dipak22/ReproducibleReserach_PeerAssessment2

The data was provided here : https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Fcsv.csv.bz2

The analysis was performed with the below system configuration


```r

sessionInfo()
```

```
## R version 3.0.2 (2013-09-25)
## Platform: x86_64-redhat-linux-gnu (64-bit)
## 
## locale:
##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
##  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8   
##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
## [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] reshape2_1.4      ggplot2_0.9.3.1   R.utils_1.32.4    R.oo_1.18.0      
## [5] R.methodsS3_1.6.1 knitr_1.5        
## 
## loaded via a namespace (and not attached):
##  [1] colorspace_1.2-4 digest_0.6.4     evaluate_0.5.5   formatR_0.10    
##  [5] grid_3.0.2       gtable_0.1.2     labeling_0.2     MASS_7.3-29     
##  [9] munsell_0.4.2    plyr_1.8.1       proto_0.3-10     Rcpp_0.11.1     
## [13] scales_0.2.4     stringr_0.6.2    tools_3.0.2
```



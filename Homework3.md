# Homework 3 Code
Dennis Murray  


```r
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
library(tidyr)
library(ggplot2)
library(readxl)
library(ggthemes)
library(magrittr)
```

```
## 
## Attaching package: 'magrittr'
```

```
## The following object is masked from 'package:tidyr':
## 
##     extract
```



```r
#read in a list of stores
StoreList <- read_xlsx("Data/StoreList.xlsx")
colnames(StoreList) <- c("store", "address", "city", "state")
```



```r
#setting seed for reproducibility
#select a random sample of 20 stores
n_Samples = 20
set.seed(12939); SampleStores <- sample_n(StoreList, n_Samples) %>% arrange(store)

head(SampleStores, 20)
```

```
## # A tibble: 20 x 4
##    store                      address             city state
##    <dbl>                        <chr>            <chr> <chr>
##  1  2754    3820A MECHANICSVILLE TPKE         RICHMOND    VA
##  2  3830   1104 NORTH WAY PO BOX 1571           DARIEN    GA
##  3  4614     235 HILLCREST DR STE TBD          LAURENS    SC
##  4  4820            3155 HIGHWAY 61 E         LUTTRELL    TN
##  5  7155             215 S STEVENS ST     SHINGLEHOUSE    PA
##  6  7297         4189 STATE ROUTE 235              ADA    OH
##  7  7853          7600 THREE NOTCH RD           MOBILE    AL
##  8  8044   706 N ALABMA ST PO BOX 953          BROXTON    GA
##  9 10425 275 W MAIN STREET P O BOX 68           SUTTON    WV
## 10 10447     125 HARDY COURT SHOPPING         GULFPORT    MS
## 11 10651         1004 RIVERBURCH PKWY           DALTON    GA
## 12 11502          2495 FORESTBROOK RD     MYRTLE BEACH    SC
## 13 11559           10141 HIGHWAY 17 N   MCCLELLANVILLE    SC
## 14 11630      1703 OLD WILKESBORO HWY      STATESVILLE    NC
## 15 11836   13 S WALNUT ST P O BOX 219 COTTONWOOD FALLS    KS
## 16 12704            4479 SAVANNAH HWY            NORTH    SC
## 17 14937       9950 S NOGALES HIGHWAY           TUCSON    AZ
## 18 15329           1210 W. VISALIA RD           EXETER    CA
## 19 16025           324 W. MAIN STREET          COLCORD    OK
## 20 16204          627 N COUNTY HWY 22           COWDEN    IL
```



```r
#select a systematic sample
n_Samples = 20

#random starting point between 1 and 1000
set.seed(210201);StartingPoint<-sample(x=1:1000, 1)


#divide remaining chunk of stores by number of samples needed
SampleEvery <- round((NROW(StoreList) - StartingPoint)/n_Samples,0)

TakeTheseSamples<-seq(from=StartingPoint, to=NROW(StoreList), by=SampleEvery)

#take every nth store
SampleStoresSystematic<- StoreList[TakeTheseSamples,]

head(SampleStoresSystematic, 20)
```

```
## # A tibble: 20 x 4
##    store                   address         city state
##    <dbl>                     <chr>        <chr> <chr>
##  1  1074      1653 CAMP JACKSON RD      CAHOKIA    IL
##  2  2050        514 W VAN BUREN ST      CLINTON    IL
##  3  3050 9355 GREENWELL SPRINGS RD  BATON ROUGE    LA
##  4  3972         1221 S SCHOOL AVE FAYETTEVILLE    AR
##  5  4853           1001 E OMEGA ST    HENRIETTA    TX
##  6  6626           5033 THEATRE DR   EVANSVILLE    IN
##  7  7471          207 US HWY 271 N       GILMER    TX
##  8  8361      11900 BUCHANAN TRL W  MERCERSBURG    PA
##  9  9302            2631 RAMADA RD   BURLINGTON    NC
## 10 10173            5530 NC HWY 24      NEWPORT    NC
## 11 10987             901 N HALL RD        ALCOA    TN
## 12 11743          410 S COMMERCIAL      CROCKER    MO
## 13 12499 1824 MARTIN LUTHER KING J       ALBANY    GA
## 14 13292         3102 MERLE HAY RD   DES MOINES    IA
## 15 14053             5474 169TH ST LAKE WISSOTA    WI
## 16 14822   6576 LAUREL ISLAND PKWY    KINGSLAND    GA
## 17 15591            927 TRACY LANE  CLARKSVILLE    TN
## 18 16400       31484 US HIGHWAY 70     RINGLING    OK
## 19 17195       5306 W ILLINOIS AVE      MIDLAND    TX
## 20 17994           4571 DAYTON AVE   GRAYSVILLE    TN
```

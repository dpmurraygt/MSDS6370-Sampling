# Lab 3 Work


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

#Exercise 1: Taxpayer Data


```r
TaxPayerData<-read_xlsx("Lab3_Files/Taxpayerdata.xlsx", sheet="Sheet1", skip=2)
```

Audit will take sample size of 2 and determine actual income for 2 taxpayers
Taxpayer 8 will be one of the two included

Exercise 1: find all combinations of 8 taxpayers, where #8 is always in the selection


```r
#Find all combinations

FirstTaxpayer <-1:7
ThirdTaxPayer <-8

a<-as.data.frame(combn(FirstTaxpayer, 2))
b<-gather(a, Iteration, Member, 1:ncol(a))

colnames(TaxPayerData)<-c("Member", "ActualIncome", "ReportedIncome")

c<-inner_join(b, TaxPayerData, by="Member")

d<- c %>% group_by(Iteration) %>%mutate(Present=1) %>% mutate(SampleMean = mean(ActualIncome), SampleVar = var(ActualIncome)) %>%
  subset(select = c(Iteration, Member, Present, SampleMean, SampleVar)) %>% spread(Member, Present, fill=0)

sum(d$SampleMean)
```

```
## [1] 1926
```

```r
sum(d$SampleVar)
```

```
## [1] 14432
```

```r
mean(d$SampleMean)
```

```
## [1] 91.71429
```

```r
mean(d$SampleVar)
```

```
## [1] 687.2381
```


```r
#overall population mean
mean(TaxPayerData$ActualIncome[1:7])
```

```
## [1] 91.71429
```

```r
#overall population variance
var(TaxPayerData$ActualIncome[1:7])
```

```
## [1] 687.2381
```


```r
#Histogram of all possible two-way means

d %>% ggplot(aes(x=SampleMean)) + geom_histogram(binwidth = 10) + theme_light() + ggtitle("Sampling Distribution of Two Person Samples of Actual Income")
```

![](Lab3_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


```r
#Histogram of all possible two-way means sampling variance

d %>% ggplot(aes(x=SampleVar)) + geom_histogram(bins = 10) + theme_light() + ggtitle("Sampling Distribution of Variance in Two Person Samples of Actual Income")
```

![](Lab3_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

#Exercise 2

```r
e <- d %>% mutate(Stratum2 = 0.125*200) %>% mutate(OverallMean = .875*SampleMean + Stratum2)

sum(e$OverallMean)
```

```
## [1] 2210.25
```

```r
mean(e$OverallMean)
```

```
## [1] 105.25
```



```r
mean(TaxPayerData$ActualIncome, na.rm=TRUE)
```

```
## [1] 105.25
```


```r
e %>% ggplot(aes(x=OverallMean)) + geom_histogram(binwidth = 10) + theme_light() + ggtitle("Sampling Distribution of 2 Stratum Sample of Actual Income") + scale_x_continuous("Sample Mean")
```

![](Lab3_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

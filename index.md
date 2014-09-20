---
title       : Modeling on a KM Dataset using Regression
subtitle    : First impressions 
author      : Joao Pedro Albino
job         : Sao Paulo State University
logo        : km_logo.jpg
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : zenburn        # 
url:
  lib:   ./libraries 
  assets: ./assets
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

In the discussion presented here, we'll use regression analysis for estimating the relationships among variables.
We'll use a R dataset (SubPerfData.Rda) with 81 observations and 26 variable.
The data were gathered and tabulated using a questionnaire applied in a number of Brazilian firms.




```r
# Loading data file
SubPerfData <- readRDS(file="SubPerfData.Rda")
```

We extract the interested columns, taking the ones necessary for performance calculations.
Now we have a dataset with 81 observations and 7 variables.


```r
##Exploring dataset variables
subdata <- with(SubPerfData, data.frame(NumFunc, ReceitaB, SP,SL,SC,ST,SM))
```

The variables stands for:
.NumFunc: Number of employees
.ReceitaB: Gross Income
.SP: Performance in Process
.SL: Performance in Leadership
.SC: Performance in Culture
.ST: Performance in Technology
.SM: Performance in Metrics 

---

We want to fit a model using Regression Analysis.
To understand which among the independent variables are related to the dependent variable.
To explore the forms of these relationships.
First, let's look at the correlation between all the variables in the subset.


```r
round(cor(subdata, use = "pairwise.complete.obs"), digits = 2) 
```

```
##          NumFunc ReceitaB   SP   SL   SC   ST   SM
## NumFunc     1.00     0.77 0.05 0.10 0.06 0.21 0.08
## ReceitaB    0.77     1.00 0.23 0.34 0.19 0.38 0.28
## SP          0.05     0.23 1.00 0.81 0.72 0.60 0.83
## SL          0.10     0.34 0.81 1.00 0.77 0.63 0.86
## SC          0.06     0.19 0.72 0.77 1.00 0.62 0.73
## ST          0.21     0.38 0.60 0.63 0.62 1.00 0.58
## SM          0.08     0.28 0.83 0.86 0.73 0.58 1.00
```
Now, with the results above, lets make plots for two sets of strong variable correlation 


```r
fitRN<-lm(ReceitaB ~ NumFunc, data=subdata)
fitS<-lm(SL ~ SM, data=subdata)
```

---

![plot of chunk ploting_one](assets/fig/ploting_one.png) 

Picking the "SL" variable, let's look the correlation, now selecting this variable as result and the other as predictors: 

---


```r
round(cor(subdata,subdata$SL), digits = 2)
```

```
##          [,1]
## NumFunc  0.10
## ReceitaB   NA
## SP       0.81
## SL       1.00
## SC       0.77
## ST       0.63
## SM       0.86
```

We can observe that SL and SM are most profitable.
Lets fit our model, now using SL as response variable and SM as predictor.


```r
fitf<-lm(SL ~ SM, data=SubPerfData)
fitf$coef
```

```
## (Intercept)          SM 
##      2.1581      0.7381
```

This gives us an equation of: SL =  2.1582 +  0.7381(SM).


# Prediction Assignment - Machine Learning project

Author: Jayarama Ajakkala

Date:  April 24, 2016

## Introduction

Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement - a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it. In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants. They were asked to perform barbell lifts correctly and incorrectly in 5 different ways. More information is available from the website here: http://groupware.les.inf.puc-rio.br/har

## Data Source
The data for this project come from this source: http://groupware.les.inf.puc-rio.br/har. Credit goes to Groupware@LES: group of research and development of groupware technologies for the data used in the prodiction data analysis.

Training data is available here
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

Test data is available here
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv



```r
suppressPackageStartupMessages(library(caret))
trainData <- read.csv("./pml-training.csv", na.strings=c("NA","","#DIV/0!"))
testData <- read.csv("./pml-testing.csv",na.strings=c("NA","","#DIV/0!"))
```
## Cleaning Data
Remove columns 1 to 7 from training data which is not related to the features. Also remove the data which contains NA


```r
dim(trainData)
```

```
## [1] 19622   160
```

```r
trainData <- trainData[,-c(1:7)]
fdata<-apply(!is.na(trainData),2,sum)>19621
trainData<-trainData[,fdata]
dim(trainData)
```

```
## [1] 19622    53
```

```r
dim(testData)
```

```
## [1]  20 160
```

```r
testData <- testData[,-c(1:7)]
testData <- testData[,fdata]
dim(testData)
```

```
## [1] 20 53
```

## Prediction
The goal of your project is to predict the manner in which they did the exercise. This is the "classe" variable in the training set. 

```r
# traget outcomes
outcome <- trainData[,"classe"]
levels(outcome)
```

```
## [1] "A" "B" "C" "D" "E"
```
We will be using Random Forest method to build prediction alogrithm for this machine learning problem.

```r
suppressPackageStartupMessages(library(randomForest))
set.seed(223)
model <- randomForest(classe~., data = trainData)
print(model)
```

```
## 
## Call:
##  randomForest(formula = classe ~ ., data = trainData) 
##                Type of random forest: classification
##                      Number of trees: 500
## No. of variables tried at each split: 7
## 
##         OOB estimate of  error rate: 0.28%
## Confusion matrix:
##      A    B    C    D    E  class.error
## A 5577    3    0    0    0 0.0005376344
## B   12 3782    3    0    0 0.0039504872
## C    0    9 3411    2    0 0.0032144944
## D    0    0   18 3196    2 0.0062189055
## E    0    0    1    4 3602 0.0013861935
```
OOB estimate of error rate is only 0.28%. Prediction model has 99% accuracy.

## Result

Now applying our prediction model to test data.

```r
result <- predict(model,testData,type="class")
print(result)
```

```
##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
##  B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
## Levels: A B C D E
```

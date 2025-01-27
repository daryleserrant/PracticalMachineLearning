# Classifying Barbell Lift Performance

###Summary

This report describes the analysis performed on data collected from six individuals who performed
weight lifting exercises while wearing accelerometers on their belt, forearm, arm, and dumbbell.
The data for this analysis can be found on <http://groupware.les.inf.puc-rio.br/har>

###Overview

Six young healthy participants were asked to perform on set of 10 repetitions of unilateral dumbbel bicep curls in five different fashions: exactly according to the specification (Class A), throwing the elbows to the front (Class B), lifting the dumbbell only halfway (Class C), lowering the dumbbell only
halfway (Class D), and throwing the hips to the front (Class E). The subjects in the experiment each wore accelerometers on the belt, arms, forearms, and dumbbell. Data was collected from these sensors, while the subjects performed the exercises. The goal of this analysis is use this data a model that can predict the matter in which the exercises were performed.

###Data Exploration and Covariate Creation

The raw data collected from the experiment contained 160 variables. Below is the structure of the dataset, obtained by using the `str` command in R:

```{r, echo=FALSE}
setwd("F:\\References\\Data Science\\R scripts\\PracticalMachineLearning")
trainData <- read.csv("pml-training.csv", na.strings=c("NA",""))
str(trainData)
```

As shown above, the data contained a number of fields that are populated with missing data. These fields were excluded from the analysis as they would produce erroneous results during model building.
The fields *X*,*user_name*, *raw_timestamp_part_1*, *raw_timestamp_part_2*, *cvtd_timestamp*, and *new_window* were also excluded as they do not provide information that's relevant to the analysis.

During exploration of the data, several pairs of predictors were discovered to be collinear:

* *gyros_arm_x* and *gyros_arm_y*
* *magnet_arm_x* and *accel_arm_x*
* *magnet_arm_z* and *magnet_arm_y*
* *magnet_belt_z* and *magnet_belt_y*
* *magnet_arm_z* and *accel_arm_z*
* *magnet_dummbell_y* and *magnet_dumbbell_x*
* *magnet_forearm_y* and *accel_forearm_y*

Multicollinearity injects unnecessary noise in the data, which during model building increases the 
variability of the model's prediction. To prevent this only one variable from each pair of predictors
will be excluded. These variables are *gyros_arm_y*, *accel_arm_x*, *magnet_arm_y*, *magnet_belt_y*, *accel_arm_z*, *magnet_dumbbell_x*, and *magnet_forearm_y*

The covariates used for prediction were therefore the 45 variables that weren't eliminated during the analysis.

###Model Building and Classification Performance
The classfication model was built using the Random Forest algorithm. The random forest was implemented with 10 trees and was evaluated using 10 fold cross validation. Based on the results of the cross-validation, the expected out of sample accuracy for this model is 99%.

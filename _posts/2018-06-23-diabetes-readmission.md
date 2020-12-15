---
title: 'Diabetes Readmission'
date: 2018-06-23
permalink: /posts/2018/06/23/diabetes/readmission
---


# Diabetes Readmission


## Overview
<p>In the United States readmission rates can define the quality of treatment and effectiveness of our 
hospital patient care. 
Patients with a serious illness such as
 diabetes have a readmission rate that has
  caused some concern in our healthcare system. 
  It is necessary to analyze key features of these 
  readmission rates to provide an understanding of how to
   provide better care and to reduce financial implications. 
   This project focuses on using key features to build predictive models to provide a tool to use to determine the probability on the basis of readmission.</p>

## Data
<p>From 1999 to 2008 clinical care data was 
captured from 130 US hospitals and 
integrated delivery networks and held
 in the Cerner health facts database.
  Each encounter was extracted and verified
   that it was an inpatient encounter,
    it was a diabetic encounter, 
    the length of stay was at least
     1 day and at most 14 days, lab
      test were performed during the encounter and medication was administered during the encounter.</p>

## Analysis Process
### Data Acquistion

<p>I loaded the necessary libraries and downloaded the file containing the CSV file and the data dictionary
from UCI Repository. After analyzing the data I decided to use all of the data to perform my analysis.</p>

### Exploratory Data Analysis

<p>During this step I performed some high level analysis and determined the target variable. Since I want to predict readmission I used the readmission status as my target variable. I used
a histogram to visualize the distribution of the data to assist in parameter tuning.
</p>



### Cleaning the Data
<p>Data can be messy. 
Prior to doing any analysis 
I needed to clean the data set and 
prepare it for modeling.  
My primary task in this process was to turn the 
categorical data into classifiable data by converting the values into integers.</p>



### Pre-processing

<p>The majority of predictive model algorithm tend to do well 
when the data is on the same scale. This is called feature scaling. I performed normalization using MinMaxScaler() from the scikit learn module
This allows the data to fit better and process quicker.
 It was critical that I examine and
pre-process the dataset before inputing 
the data into any algorithms.</p>

### Partitioning Data and Selecting Features
<p>Feature selection eliminates noise and 
unnecessary data within the dataset. Using only the most important 
features enables the accuracy to increase as well as increasing the speed of the algorithms. I used the 
random forest algorithm to assess the import features to find the most important features which were the number of lab procedures, can be defined as the number of 
procedures (other than lab tests) performed during the encounter.</p>

![features](/assets/img/features.png)

<p>The data was divided into training and test sets. It is a common practice to ensure that
 our predictive models performs well on the training set and also be
 generalized to the input of new data. For this project, I felt that it was appropriate to divide the training and test data to 60/40. 
 The features were stored in variables "features_train" and
  "features_test" and the targets are in "target_train" and "target_test".  </p>

  
### Modeling

<p>The models used in the project are Logistic Regression, Decision Tree, KNN, Navies Bayes, SVM, Gradient Boost, SDG, ANN and Ensembles. 
The range of predictive models are simple to complex.
There are various parameters that can provide a better accuracy score. 
I got a good indication of how the data fits with the different models I used to 
determine the best performing model. In the table below, 
I've provided the predictive model, the justification on using the model and the initial 
parameters used for the models. </p>

![Models](/assets/img/model_table.png)


### Evaluation & Tuning

<p>

To evaluate the performance of the models, the accuracy score, precision and recall metrics are used to determine the best performing model. 
The accuracy score gives us the number of correct predictions made by the total number of predictions made. 
Precision is the number of true positives divided by the number of true positive and false positives which measures how exact the model is. 
The recall is the number of true positives and the number of false negatives which show how complete the model is. 
Evaluating the performance goes back to understanding the overall goal of the project which is to develop a predictive model to determine the probability a patient will be readmitted to the hospital based on a number of inputs. 
Understanding the intent of the models and the metrics are important in evaluating the performance. 
Given the accuracy scores in the figure below, AdaBoost is the best model for this data set.

</p>

![Scores](/assets/img/scores.png)

<p>I tuned the model to verify if the original models could perform better. For example, I added a cost penalty of 3 to the SVM model and it improved the accuracy
by 7%.
 </p>



# Final Product


![Final](/assets/img/ezgif.com-optimize.gif)


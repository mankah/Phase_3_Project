# Phase_3_Project
 
**Authors**: *Manav Kahlon, Anthony Conte, Sanjit Varma, Ben Bowman*
  
## Overview
- [Business Problem](#Business-Problem)
- [Data](#Data)
   - [Well Data](./data)
   - [Data Dictionary](./Data_Dictionary.txt)
- [Methods](#Methods)
- [EDA Results: Notable Features](#EDA-Results-Notable-Features) 
- [Modeling Results](#Modeling-Results)
- [Conclusions](#Conclusions)
- [For More Information](#For-More-Information)
- [Repository Structure](#Repositroy-Structure)
  

## Business Problem
24 million people in Tanzania are affected by the water crisis. The President of Tanzania Samia Suluhu Hassan has tasked us with determining the status of water wells within her country to see where the country's limited resources need to be sent to provide its people with clean drinking water. [1](#sources)
 
## Data
We examined close to 60k records of well data collected from Tanzania between 2002 and 2013.  This dataset included 40 different features relating to whether the well was functional, non-functional, or in need of repair. 

 #### Data from Driven Data [2](#sources)
    * Training_set_values.csv
    * Test_set_values.csv
    * Training_set_labels.csv
 #### Non-engineered data
    * X_train.csv
    * y_train.csv
    * X_test.csv
    * y_test.csv
 #### Engineered data
    * X_train_eng.csv
    * X_test_eng.csv
    * y_train_eng.csv
    * y_test_eng.csv
   
    
    
## Methods
We first merge the two data sets into one data set on `id`. After this, we drop columns that were either irrelevant or had values that were the same. We clean the data by imputing 'missing' for categorical columns with missing data. Since our data set contained about 60,000 rows we decide to make a holdout data set with 10% of the data that we will test our two best models on. After that, we make some inferential plots to show if there was any relationship between a column and the status of a well. After this, we create a dummy model that would only predict the most frequent well type. Then we began a process of creating KNN, SVM, XGBoost, and RFC models to see which one would be the best at predicting which wells were either non-functional or need repairs. For each model, we use the split training and test data, cross-validate, and calculate the accuracy and log loss.  

    
## EDA Results Notable Features
A majority of wells are functional (about 54%), though there is a large percentage (38%) that are non-functional.  About 7% of wells need repair.  

![Status_group_dist](https://user-images.githubusercontent.com/82840623/128512999-1b11c1c2-9ca5-48b4-b71c-9c810f98dae5.png)

We can see that older wells have higher percentage chance of being non-functional. 

![Decade](https://user-images.githubusercontent.com/82840623/128513133-126cf54b-00b0-45a6-8ee9-b408d265e164.png)

We note that some installers seem to have higher percentages of non-funtional wells.  For example 'Government' and 'Central Government' (we don't know if these are the same category, or whether 'Government' might include regional authorities vs. the central government) installed wells have more of a chance at being non-functional than functional.  

![Installers](https://user-images.githubusercontent.com/82840623/128513185-dc4d9aa4-00a9-4db4-9f27-899886690269.png)

Likewise, the type of water source affects the chances that the well is non-functional.  You can see from the following chart, for example, that springs have higher percentages of non-functional wells versus rivers, shallow wells higher than springs, and so forth.

![Source](https://user-images.githubusercontent.com/82840623/128513236-d345c4d0-3aea-4f51-9238-9ac5a537ab44.png)


 
## Modeling Results
We run five different classifiers and perform a GridSearch on each. Our metrics for selecting the best model are based on the business problem at hand. We are interested in overall model accuracy, but we want to focus on wells that need repair or are non-functional. Therefore we pay close attention to how well the models predict the ‘needs-repair’ and ‘non-functioning’ wells (we are less concerned with false positives since there is less harm in misidentifying a working well than in missing a faulty one). We create a new metric which is the sum of the recalls of ‘needs-repair’ and ‘non-functioning’ wells as a guide for selecting models. Here are the model performances:

![image](https://user-images.githubusercontent.com/82840623/128362806-79864f15-fd27-4da8-bec5-67c79d6b3dca.png)

The default Support Vector Classifier does best with our custom metric, identifying nearly 70% of the ‘needs-repair’ wells (in the test data) and 68% of the non-functioning ones (even though, compared to other models, it has a lower overall accuracy due to weaker performance identifying functional wells).

The tuned Random Forest model also does well, predicting 56% of ‘needs-repair’ wells and 76% of the non-functioning wells, making it slightly less predictive than the SVC model. It does have the advantage, however, of higher overall accuracy due to its better predictions of functioning wells.


  
    
## Conclusions
We recommend the SVC model due to its superior ability to identify the two well types that require servicing. SVC is especially good at identifying wells in need of repair, compared to other models, a feature that might appeal to the government if it wants to prioritize broken wells over non-functioning ones.  We are willing to trade some false positives for this benefit. This recommendation would change, however, if the Government signals that visiting functional wells (when expecting to repair or replace one) is too inefficient and costly. In that case, we would be more concerned with false positives and recommend the Random Forest model.

    
    
## For More Information
Please review our full analysis in different notebooks [Data Processing Notebook](./01_data_preparation.ipynb), [First Set of Models Notebook](./02_logistic_regression_knn_svm.ipynb), [Random Forest Model Notebook](./03_random_forest_models.ipynb), [XGBoost Notebook](./04_xgboost.ipynb), [Feature Engineering Notebook](./05_feature_engineering.ipynb), [Visualizations Notebook](./06_visualizations.ipynb), and our [Final Notebook](./07_svm_rfc.ipynb) or our [Presentation](./Presentation.pdf).    
    
## Repositroy Structure
 ```
├── data                                  <- Sourced from an external source
├── images                                <- Images that were used in the presentation and notebooks                                           
├── gitignore                             <- python files to ignore 
├── 01_data_preparation.ipynb             <- Data Prep Notebook
├── 02_logistic_regression_knn_svm.ipynb  <- Logistic Regression, KNN, and SVM Models Notebook
├── 03_random_forest_models.ipynb         <- Random Forest Models Notebook
├── 04_xgboost.ipynb                      <- XGBoost Models Notebook
├── 05_feature_engineering.ipynb          <- Attempted Feature Engineering Notebook
├── 06_visualizations.ipynb               <- Visualizations Notebook
├── 07_svm_rfc.ipynb                      <- Final Models Notebook
├── Presentation.pdf                      <- PDF of our project presentation  
├── Data Dictionary.txt                   <- Data Dictionary
└── README.md                             <- The README.md

```
#### Sources
1) https://lifewater.org/blog/tanzania-water-crisis-facts/
2) https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/page/25/#labels_list

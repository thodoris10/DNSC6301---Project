# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Baiyang, Theodoros Pateros, Yeobeen, 'gnby38540903@gwu.edu', 'tpateros@gwu.edu', 'yeobeen.yun@gwu.edu'
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [GWU_DSNC_6301_G17.ipynb](GWU_DNSC_6301_G17.ipynb)


### Intended Use
* **Primary intended uses**: This model is an example probability of default classifier. It is designed to determine eligibility for a credit line increase. 
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 
  * Python version: 3.7.13
  * sklearn version: 1.0.2
* **Hyperparameters or other settings of your model**: 
```
best_model.get_params (ccp_alpha: 0.0, class_weight: None, criterion: 'gini',
                       max_depth: 6, max_features: None, max_leaf_nodes: None,
                       min_impurity_decrease: 0.0, min_samples_leaf: 1,
                       min_samples_split: 2, min_weight_fraction_leaf: 0.0,
                       random_state: 12345, splitter: 'best')`
```
### Quantitative Analysis

#### Correlation Heatmap
![Correlation Heatmap](download.png)

* The figure above illustrates a negative correlation between the the variable "RACE" and the target variable "DELINQ_NEXT". 

#### Iteration Plot
![Iteration Plot](https://user-images.githubusercontent.com/111533925/186820882-6f0565ba-81b7-4d6d-a9de-94a69738e444.png)

* The iteration plot above shows the relationship between "Training AUC", "Validation AUC", and "Hispanic-to-White AIR" with different levels for the tree depth. We have decided that a tree depth of 6 is most accurate because at that point the "Validation AUC" is at its highest point, "Training AUC" is at a satisfactory level, and "Hispanic-to-White AIR" is at 0.83. 

* AUC 
 
|      | Training Data | Validation Data | Test Data |
| ---- | ------------- | ---------------- | ---------- |
|**AUC**| 0.783722 | 0.749610 | 0.7438 |

* AIR

|      | hispanic-to-white | black-to-white | asian-to-white | female-to-male |
| ---- | ----------------- | ---------------| -------------- | -------------- |
|**AIR**| 0.83| 0.85 | 1.00 | 1.02 |


### Ethical considerations
* **Potential negative impacts of using the model**:
  * Math or software problems: The performance of the model needs to be monitored to ensure that the model will function properly in the future. Currently, its accuracy is standing at 70% which is not an ideal outcome since there is a 30% probability that the model will make an innacurate decision.
  * Real-world risks(bias): The model is subjected to real-world risks such as bias due to the presence of demographic variables in the data set. This introduces the risk of correlation with demographic infromation which is not a desirable feature for this model.

* **Potential uncertainties relating to the impacts of using the model**:
  * Math or software problems: Even if the model looks good today, we canot preditct how the model will perform in the future so it needs to be monitored. There is no way to determine the upper limit of the data the model can train. Furthermore, the model does not include an explicit column that will allow it to take time as a variable for consideration. More specifically, the training data need to include the older data of our dataset. The validation data are expected to be more recent than the training data, and lastly, the testing data must include the most recent data of our dataset. The fact that this model does not take timing into consideration can lead to future uncertainties with regards to the model's effectiveness.
  * Real-world risks(Securities and privacy): The users of the model are required to upload data for the model to function properly. In situations where the data are of a more sensitive nature in terms of privacy and importance, data security concerns could arise about the model. 
  
* **Unexpected results**: 
  * The absence of missing values is the first unexpected result of our model.
  * Secondly, as illustrated in the figure below, the variable "PAY 0" appears to have a substantive variable importance for our model which also classifies as an unexpected result.

![pay 0](https://user-images.githubusercontent.com/111533925/186976921-4a74fca7-6813-451f-b370-0ab0ce15f968.png)

# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Baiyang, Theodoros, Yeobeen, 'gnby38540903@gwu.edu', 'tpateros@gwu.edu', 'yeobeen.yun@gwu.edu'
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_Example_Project.ipynb](DNSC_6301_Example_Project.ipynb)

### Intended Use
* **Primary intended uses**: This model is an example probability of default classifier, with an example use case for determining eligibility for a credit line increase.
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
DecisionTreeClassifier(ccp_alpha: 0.0, class_weight: None, criterion: 'gini',
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

*The iteration plot above shows the relationship between "Training AUC", "Validation AUC", and "Hispanic-to-White AIR" with different levels for the tree depth. We have decided that a tree depth of 6 is most accurate because at that point the "Validation AUC" is at its highest point, "Training AUC" is at a satisfactory level, and "Hispanic-to-White AIR" is at 0.83. 

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
  * Math or software problems:  The system might not work well in the future and it needs to be monitored. The accuracy only makes 70% and 30% will make wrong decisions. 
  * Math or software problems: The system needs to be monitored to ensure that it will still produce appropriate results in the future. Today, the software is rated with an AUC of 0.7438 for the test data, which means that it will only be correct 74 percent of the time. While it is very challenging to produce a model that will always be accurate, the possibility of producing the wrong outcome 26 percent of the time definitely classifies as potenital negative impact of using the model. 
  * Real-world risks(bias): Breaking all people into simple groups introduces bias right a way. Some race groups receive the credit disadvantages. The model output can be corelated with the demographic information such as gender and race and will show different accuracies and validity.
  * Real-world risks(bias): The model is subjected to real-world risks such as bias due to the presence of demographic variables in the data set. This introduces the risk of correlation with the demographic infromation which is not a desirable feature for this model.

* **Potential uncertainties relating to the impacts of using the model:**:
  * Math or software problems: Even if the model looks good today, we canot preditct how the model will perform in the future so it needs to be monitored. There is no way to determine the upper limit of the data the model can train. Trainging dataset has to come in the earlier time and the validation data has to come next in time and testing data has to the most recent data, but there is no explicit column about the time of dataset, which will make wrong decisions. 
  * Math or software problems: #####STILL NEED TO WORK ON THIS ONE##### 

  * Real-world risks(Securities and privacy): Users need to upload data to the model for training. If some data is more important or private, it will cause user data leakage to a certain extent, resulting in data security issues
  * Real-world risks(Securities and privacy): The users of the model are required to upload data for the model to function properly. In situations where the data are of a more sensitive nature in terms of privacy and importance, data security concerns could be raised about the model. 
  
* **Unexpected results**: 
  * The absence of missing values is the first unexpected result of our model.
  * Secondly, as illustrated in the figure below, the variable "PAY 0" appears to have a substantive variable importance for our model. 
  * Lastly, the correlation between the variable "RACE" and the target variable "DELINQ_NEXT" is also an unexpected result since we would not expect race to be a factor that determines whether a person is likely to default on its credit obligations. 

![pay 0](https://user-images.githubusercontent.com/111533925/186976921-4a74fca7-6813-451f-b370-0ab0ce15f968.png)

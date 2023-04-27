# 1 Background
The internet has revolutionized the way we shop. With the advent of online shopping, we have experienced comfort and freedom of shopping almost anything, at any time, and from anywhere. But, as is the case with every technological advancement there are cons of online shopping. Disadvantages like — risk of fraud, unprecedented delays in receiving the products, lack of customized attention are to name a few.

To mitigate some of the cons, a large number of IoT (Internet of Things) sensors being used in the supply chain management these days. Supply chain is an entire system of producing and delivering a product or service, from sourcing the raw materials to the final delivery of the product or service to end users. IoT devices have added a layer of information to this management system by — tracking the location of products, both at rest and in motion; capturing real-time shipment and inventory visibility and tracking; identifying issues with goods getting lost or delayed; etc. This supply chain data generated by the IoT devices, when churned efficiently, can help the companies to make - predictions, recommendations, identify hidden patterns for better decision making in the future. However, with a lot of options available to analyze data, it often becomes difficult to decide which machine learning (ML) model to use.

# 2 Project Plan
- Use different ML models and assess their performances to perform Fraud Detection.
- Demonstrated data preprocessing steps.
- Leveraged various feature selection methods — Filter method, Wrapper method (Step-wise), Embedded (Lasso and Elastic Net) to deduce the most significant set of predictors.
- Implemented classification models to detect fraudulent transactions — Principle Component Analysis (PCA), Bagging, Random Forest, Decision Tree, K-nearest neighbor (KNN), Boosting.
- Evaluated the performance measures of these models and have preseneted a comparative study.

# 3 Dataset Description
The data set used in this project is maintained transparently with the Creative Commons 4.0 license through the Mendeley data repository. The data set consists of roughly 180k transactions from supply chains used by the company DataCo Global for over a period of three years.

### 3.1 Source
https://data.mendeley.com/datasets/8gx2fvg2k6/5

### 3.2 Description
The data set consists of 3 files:
- Transaction Data: Structured supply chain transaction data 'DataCoSupplyChainDataset.csv’.
- Log Data: Unstructured transaction log data 'tokenized access logs’.
- Column Description: Structured data 'DescriptionDataCoSupplyChain.csv’.
The ‘Transaction Data’, which has some 53 columns, has a mix of categorical and quantitative values. The primary objective is to build a fraud detection using this data set.

### 3.3 Data Visualization
-  Proportion of ‘SUSPECTED FRAUD’ is much lower than the order statuses. The data imbalance is likely to cause incorrect prediction. Therefore, up-sampling or down-sampling would be required to mitigate the data imbalance.
- Africa has the lowest number of transactions. This could be a potential market for the company.
- Maximum transaction types are ‘Debit’ and the minimmum is ‘Cash’ transactions.
- From the sales data we also find that most of the deliveries made by DataCo Global are ‘Late Delivery’. This might be a potential setback for the company’s business and turnover.

<div align="center">
  <img src="https://user-images.githubusercontent.com/82466266/234984866-bcb11d9d-601e-4ca5-b970-27f1177231cd.JPG" width=80% height=40%>
  <img src="https://user-images.githubusercontent.com/82466266/234986008-a3537f1a-8e22-4060-af2c-1293a42d2cf9.JPG" width=80% height=40%>
</div>

# 4 Data Preprocessing
The entire data set was assessed to identify missing values, NA values, irrelevant data points etc. After data preprocessing there were 38 explanantory variables and 1 response variable remaining.

- __Identify Columns Missing with Data__: Columns with more than 60% of the data missing are removed from this study. Columns with significantly large missing values will not contribute toward the prediction model. Also, imputing data for large number of missing values may lead to incorrect interpretations. Columns: ‘Order Zipcode’ and ‘Product Description’.
- __Identify Columns with Zero and Near-zero Variance__: Columns with fixed values (zero variance) or near-zero variance are identified and assessed. The zero variance columns were excluded from the data set however the near-zero variance columns were retained. Since these columns contain customer related information, they may help to identify certain trends and hence can be useful for this study.
- __Merge and Remove Redundant Columns__: Separate columns for customer first name and last name are not required. Therefore, these columns were merged. Column ‘Product Image’ was also removed since it contained the urls.
- __Create New Feature Columns__: Order and Shipping date time values were split to extract corresponding year, month, day and hours. This was done to help identify which months or days or hours are contributing toward predicting fraudulent transactions. A new column is created to label the fraud and non-fraud orders — ‘response fraud’. response fraud = (= 1, if Order Status == ’SUSPECTED FRAUD’= 0, Otherwise )
- __Training and Test set__: Data split into 70-30 ratio for training and test data set.
- __Down Sampling for Data Imbalance__: To mitigate the issue of data imbalance down sampled.

# 5 Methods & Implementation
### 5.1 Feature Selection
It is a technique to identify the most significant explanatory variables and reduce the model size by eliminating the redundant and irrelevant variables. Feature Selection helps to build a useful and a constructive model which is easier to interpret and trains faster. FS helps with lowering the error due to variance (introduced by large number of predictors) while keeping the bias error relatively low. This technique also helps to minimize the issue of overfitting.
- __Filter Method__: This technique comprises methods that collect the fundamental properties of the features that are measured through univariate statistics instead of using cross-validation performance, that is the Filter method does not use the classifier. These methods are quicker and less expensive computationally. While dealing with high-dimensional data, it is computationally cheaper to use filter methods.
- __Wrapper Method__: This technique considers all possible subsets of features, assessing a classifier with that feature subset, and evaluating their quality by learning. The wrapper methods usually provide a better predictive accuracy than filter methods. One major downside of the Wrapper technique is they are computationally heavy which is a greater problem when handling high-dim data set.
- __Embedded Method__: Has the advantages of both filter and wrapper methods by not only comprising interactions of features but also by retaining a reasonable computational cost. Embedded methods combine the process of feature selection while building the classifier, therefore, are much faster than the wrapper technique.

#### 5.1.1 Using Filter Method
- _Using filterVarImp()_ — The function filterVarImp() is used to rank the importance of each predictor evaluated individually using a ‘filter’ approach.
- Heat Maps for Correlation: The correlation coefficients of the data set is computed and represented in the form of a heat-map.
<div align="center">
  <img src="https://user-images.githubusercontent.com/82466266/235005243-e6fb1cb6-625c-4109-9664-7cf0dd1ddbc0.JPG" width=60% height=40%>
</div>

#### 5.1.2 Using Wrapper Method
Prior to using the wrapper, embedded FS methods and the subsequent classification models, the data set was down sampled to handle data imbalance. 
- _Step-wise Selection_ (or sequential replacement) was used which is a combination of forward and backward subset selections.

#### 5.1.3 Using Embedded Method
- _LASSO_: Using 5-folds cross-validation approach, λ1SE, λmin were computed. The λmin was used to perform prediction on the test data set. Lasso identified 2 columns as the most significant for fraud detection — ‘Type’ and ‘Order Country’.
- _Elastic Net_: Elastic Net was used in similar fashion to that of Lasso. The best α = 0.73 whereas for Lasso α = 1. Elastic Net identified only 1 column as  significant — ‘Type’.

### 5.2 Classification Models for Fraud Detection
#### 5.2.1 Decision Tree
Decision tree builds classification or regression models in the form of a tree structure. For this study, classification decision tree is used to categorize the fraud and non-fraud transactions. the final tree structure shows only one significant variable — ‘Type’. Test Error, Accuracy and F1 scores are calculated for the decision tree model. 

#### 5.2.2 Bagging and Random Forest
In Bagging (Bootstrap + Aggregating) method we generate B bootstrap samples using all the predictors and generate B# of tree structures. These trees are then aggregated to produce a single resultant tree. In contrast to Bagging, Random Forest uses only a subset of the predictors when creating the splits in the tree. The subset count is given and the most common value is √p where p is the total number of features in the original data. This selection of √p predictors is done randomly for each split. Important set of variables deduced using Bagging and random Forest models are highlighted below.
<div align="center">
  <img src="https://user-images.githubusercontent.com/82466266/235006682-182b6bdd-41a1-42c4-a872-396c7855cc45.JPG" width=70% height=20%>
</div>

#### 5.2.3 Boosting
Boosting is a type of ensemble method where several weak learners are created at each step. These weak learner models keep adding to produce the next model. Continuing this process leads to a strong additive model.

#### 5.2.4 k-Nearest Neighbors- KNN
KNN algorithm is a non-parametric, supervised learning classifier, which uses proximity to make classifications or predictions about the grouping of an individual data point. It is typically used as a classification algorithm, working off the assumption that similar points can be found near one another. KNN model is implemented to predict the ‘response fraud’ for the test data set and the performance measures are captured.

#### 5.2.5 Principal Component Analysis - PCA
PCA is a popular technique for analyzing large data sets containing a high number of dimensions/features per observation, increasing the interpretability of data while preserving the maximum amount of information, and enabling the visualization of multidimensional data. For this study, I have used 6 PCs which contributes for approximately 52% of the total variance. Note that reduction in the number of variables of a data set naturally comes at the expense of accuracy.
<div align="center">
  <img src="https://user-images.githubusercontent.com/82466266/235005861-1513743d-65e7-4232-a717-a983636a19ad.JPG" width=30% height=30%>
</div>

# 6 Code (in R)
R Code: https://github.com/ShilpikaB/Assess-Machine-Learning-Models-for-Fraud-Detection/blob/main/Math748_CapstoneProject.R


# 7 Results & Discussion
All analyses and implementations were done using the R programming language with base packages and the subsequent analysis-specific packages. 
- Variable ‘Type’ was the most significant variable indenitifed using the feature selection models. 
- Among the 8 classification models used to detect fraudulent transactions, Boosting with an interaction depth = 4 performs the best. Performance measures — lowest Test Classification Error, highest Accuracy and highest F1 score.
<div align="center">
  <img src="https://user-images.githubusercontent.com/82466266/235002599-a7ec5b3f-e1cc-455b-b103-1807c8f45f9b.JPG" width=50% height=40%>
</div>

# 8 References
- https://data.mendeley.com/datasets/8gx2fvg2k6/5

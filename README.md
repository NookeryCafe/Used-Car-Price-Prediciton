# Used Car Price Prediciton
Price Prediciton Using Explainable Boosting Machines

Date: March, 2024

Name: 
Forough Mofidi
Bonny Mathew
Shaojie Chen
Naoki Tsumoto

## Table of Content
  Context and Objectives
  Overall Project Design
  Model Preparation
  Linear Model Approach
  Non-Linear Model Approach
  Findings with Causal Inference Method
  Challenges and Limitations
  Conclusions and Future Study


## Chapter 1: Context & Objectives

Context: The used car market exhibits complex dynamics influenced by various factors. Understanding these dynamics is crucial for both consumers and businesses in the automotive industry.
Problem Statement: Develop a predictive model for used car prices that not only achieves high accuracy but also provides insights into the factors influencing these prices and the causal relationships between them.

## Chapter 2: Overall Project Design
### Model Preparation 
#### Dataset & Description
Dataset: US Used Cars Dataset from Kaggle
Description:
Context: The dataset contains 3 million real world used cars details.
Content: This data was obtained by running a self made crawler on Cargurus inventory in September 2020.

#### Preprocessing & EDA
What we did or Key points:
EDA: Took a snapshot of 100k observations for the 3mn dataset to complete this step. Inspected the dataset using histograms, ran a summary on the numerical variables to understand the summary statistics and checked for missing values. The target variable, price was also converted to its log value to remove skewness in the variable.
Missing Values: There were about 66 variables, out of which approx 9 of them had more than 50% of the data missing , hence we got rid of those variables. Even after this, there were many observations with missing values which called for Imputation. For the numerical variables we imputed using the Multivariate Imputation by Chained Equations (MICE) Approach. For the categorical variables we used the Feature Engine library in Python.
Feature Engineering: The pairwise correlation of the variables revealed strong correlations between some of them. Therefore, we ran a Principal Component Analysis (PCA) on those highly correlated variables to reduce the dimensionality of the dataset and remove any multicollinearity in the dataset. 
Final Dataset: After the EDA and Data Preprocessing we were left with 42 variables to run the model on.

### Linear Model Approach

Conducted modeling with GLM (Generalized Linear Model)
Graphs illustrating the characteristics of the dataset
Choosing the link function and the distribution family:
Link Function: GLMs utilize a link function to model the relationship between the linear predictor and the mean of the dependent variable. For a wide range of positive values like used car prices, a log link function is commonly used. This ensures that the predicted prices are always positive.
Distribution Family: Regarding the prediction of used car prices, if the response variable represents positive continuous values with a potential for asymmetric distribution, Gamma regression could be a suitable choice. Gamma regression is particularly useful for data where the variance depends on the mean, which might align with the distribution of used car prices. This is because the prices could be skewed, with higher variability in the higher price range compared to the lower price range.

#### Linear Model Result
The Linear Model was unable to adequately explain used car prices, indicating that a non-linear model is more suitable for this dataset
Overlay of dataset and prediction graphs
Demonstrates low performance metrics, such as R^2, RMSE, MAE

### Non-Linear Model Approach

A literature review revealed best practices for Explainable Boosting Machines.
The chosen method for this project is from an XAI perspective, Explainable Boosting Machines, and from a Causal Inference perspective, Propensity Score Matching.

#### Explainable Boosting Machines
A conceptual diagram to explain the mechanism. The strengths = Reasons why this model is appropriate are shown in text
Fundamental Principle: Explainable Boosting Machine (EBM) is a tree-based, cyclic gradient boosting Generalized Additive Model with automatic interaction detection. EBMs are often as accurate as state-of-the-art blackbox models while remaining completely interpretable. EBM is a glassbox model, designed to have accuracy comparable to state-of-the-art machine learning methods like Random Forest and Boosted Trees, while being highly intelligible and explainable. EBM is a generalized additive model.
Strengths: EBM’s light memory usage and fast predict times makes it particularly attractive for model deployment in production. EBM has a few major improvements over traditional GAMs. EBM learns each feature function using modern machine learning techniques such as bagging and gradient boosting. The boosting procedure is carefully restricted to train on one feature at a time in round-robin fashion using a very low learning rate so that feature order does not matter. It round-robin cycles through features to mitigate the effects of co-linearity and to learn the best feature function for each feature to show how each feature contributes to the model’s prediction for the problem.

### Findings with Causal Inference Method
#### Propensity Score Matching
A conceptual diagram to explain the mechanism. The strengths = Reasons why this model is appropriate are shown in text
Fundamental Principle: PSM is a statistical technique used to estimate the effect of a treatment, policy, or intervention. It involves calculating the probability (propensity score) of each unit receiving the therapy based on observed characteristics. PSM matches units with similar propensity scores from treatment and control groups, creating a balanced dataset with similar characteristics. This allows for a more accurate treatment effect estimation by reducing bias from confounding variables. PSM is based on the Rubin Causal Model, which frames causal inference regarding potential outcomes. By matching units with similar propensity scores, PSM emulates a randomized experimental design within an observational study, making it valuable when conducting randomized controlled trials is not feasible. The propensity score is usually estimated using logistic regression to balance covariates between treated and control groups.
Strengths: PSM reduces selection bias in observational studies by matching units based on propensity scores. This mimics randomization and isolates the treatment effect. It also simplifies analysis by reducing the dataset size, making it beneficial for large datasets. PSM is flexible and adaptable to different data and study designs, providing a robust methodological framework for causal inferences in non-experimental settings. It handles the dimensionality problem in multivariate analysis by condensing covariates into a single score, facilitating clearer comparison between groups and understanding the treatment effect.

### Explainable Boosting Machines Result
Graphs showing which features contribute to used car prices.
Graphs explaining the interactions between features.
The following analysis scenario was created using these features for detailed analysis with Propensity Score Matching (subject to change based on the features that emerge):
The premium pricing of hybrid cars.

#### Propensity Score Matching Result
The premium pricing of hybrid cars (subject to change based on the features from Page 9):
Evaluating how much more expensive hybrid cars are compared to non-hybrid cars and whether the majority of this price difference is due to the hybrid characteristic or other features (such as year, mileage, brand).
Assessing the extent to which the hybrid characteristic contributes a premium (or discount) to the price of used cars.

### Challenges and Limitations
#### Dataset Considerations: 
The data is up-to-date as of September 2020, not reflecting the most recent trends.
Additionally, due to imperfections in the dataset itself, about half of the data was deleted before model construction, meaning the models do not necessarily reflect the entire market data.
Model Selection Challenges: Explainable Boosting Machines (EBM)
Computational Cost: While EBM is relatively lightweight, training can be time-consuming, especially with large datasets or when the number of features with complex interactions is high. The computational cost becomes more significant in such scenarios.
Risk of Overfitting: EBM learns individually for each feature, necessitating appropriate regularization and cross-validation to prevent overfitting. Managing model complexity is key to optimizing accuracy while maintaining high interpretability.
Analysis Method Limitations: Propensity Score Matching (PSM)
Limitations in Application: PSM is a powerful tool for causal inference from observational data, but its effectiveness depends on observed covariates. Biases from unobserved covariates cannot be addressed, risking misinterpretation of the true causal relationships.
Substitute for Randomized Controlled Trials: While PSM is sometimes used as an alternative to randomized controlled trials, it is impossible to achieve complete randomization between matched groups with PSM. Therefore, caution is needed when interpreting the results of PSM, as the estimated effects are not definitive evidence of causality.

## Chapter 3: Conclusions and Future Study
### Conclusions and Future Study
#### Conclusions: 
It was found that using non-linear models is necessary to maintain high prediction accuracy in predicting used car prices in the US market, due to the complex relationships within the market.
Particularly, being a hybrid vehicle is an important feature, and it has been confirmed using XAI methods that it brings a premium price of XXX USD to the market.
This result implies a latent need in the US market for high fuel efficiency, meaning there is a demand to reduce gasoline expenses and the frequency of visits to gas stations for regular commutes.
Future Study
The data used in this study is somewhat outdated, being only up to 2020, and thus less influenced by recent environmental shifts towards carbon neutrality. Therefore, analyzing with the latest data considering current trends, such as electric vehicles (EVs), is believed to lead to a more fundamental understanding of the market.

# Framingham Cardiovascular Risk Prediction

<details>
<summary>Table of Contents</summary>

1. [About the Project](#about-the-project)
2. [Dataset Description](#dataset-description)
3. [Hypothesis Testing](#hypothesis-testing)
4. [Feature Engineering and Data Pre-processing](#feature-engineering-and-data-pre-processing)
    + [Handling Missing values](#handling-missing-values)
    + [Feature Manipulation](#feature-manipulation)
    + [Handling outliers](#handling-outliers)
    + [Iterations](#iterations)
    + [Data Splitting, Balancing and Scaling](#data-splitting-balancing-and-scaling)
5. [Model Implementation](#model-implementation)
6. [Model Evaluation](#model-evaluation)
9. [Libraries Used](#libraries-used)
10. [Contact](#contact)
</details>


## About the Project

Cardiovascular disease (CVD) is a major cause of death worldwide. It is a group of diseases that includes heart disease, stroke, and other vascular diseases that affect the heart and blood vessels. The Framingham Heart Study, one of the most extensive epidemiological studies of CVD risk factors, is an ongoing cardiovascular study on the residents of the town Framingham, Massachusetts.

The objective of this project is to develop a classification model to predict the risk of Coronary Heart Disease (CHD) in 10 years for any given person. It also focuses on concluding an ideal set of methods at pre-processing of the data available in this context, to implement Machine Learning analyses.

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

## Dataset Description

The Framingham dataset consists of medical, behavioural and demographic data on 3390 residents from the town of Framingham, Massachussets. Each one of the features are considered a possible factor for prediction of a Coronary Heart Disease in the next 10 years. The following are the features for a particular resident:
*   **id**: Personal identification number (Unique)

Demographic:
*   **sex**: Male or Female (Nominal)
*   **age**: Age of the patient (Continuous)
*   **education**: no information provided (**Ordinal assumed**)

Behavioral:
*   **is_smoking**: Whether or not the patient is a current smoker (Nominal)
*   **cigsPerDay**: Number of cigarettes smoked by the person per day on average (Continuous)

Medical information:
*   **BPMeds**: Whether or not the patient is on blood pressure medication (Nominal)
*   **prevalentStroke**: Whether or not the patient previously had a stroke (Nominal)
*   **prevalentHyp**: Whether or not the patient was hypertensive (Nominal)
*   **diabetes**: Whether or not the patient has diabetes (Nominal)
*   **totChol**: Total cholesterol level in mg/dL (Continuous)
*   **sysBP**: systolic blood pressure in mmHg (Continuous)
*   **diaBP**: diastolic blood pressure in mmHg (Continuous)
*   **BMI**: Body Mass Index (Continuous)
*   **heartRate**: Heart rate (Continuous)
*   **glucose**: glucose level in mg/dL (Continuous). **Assumed fasting glucose**

Target variable to predict:
*   **TenYearCHD**: 10 year risk of coronary heart disease - (Nominal)

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

## Hypothesis Testing

Three hypothetical statements were tested on this dataset to better understand the sample space and the population, and to discern the relationship between the categorical variables. They were:
1. 50% of men and 44% of women have more than the optimum Blood Pressure values
2. The mean systolic and diastolic Blood Pressures are the optimum values of 130/80 mmHg respectively
3. The categorical variables which are factors for risk prediction, are not dependent on the variable to be predicted

The tests were performed using the **one-sample proportion test**, **one-sample t-test** and **two-sample chi-squared test** respectively. The **Shapiro test** was also utilised for the final statement

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

## Feature Engineering and Data Pre-processing

Before model implementation, the data is made to go through a set of pre-processing methods. Five different datasets were generated from this, based on the different methods of pre-processing - one main dataset and four other iterations to check the sensitivity of model prediction power with respect to method of pre-processing.

### Handling missing values

Missing values were imputed realistically for minimal amount of bias into the dataset. For example, among the missing values in **BPMeds** which is a binary feature, only those were imputed with 1 (indicating they take medications) who have a more than optimum systolic or diastolic blood pressure level, the optimum level being defined on the basis of diabetes according to the [NCBI](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2560868/#:~:text=For%20most%20people%2C%20blood%20pressure%20readings%20should%20be,80%20mmHg%20when%20measured%20in%20the%20doctor%E2%80%99s%20office.)

### Feature Manipulation

To reduce dimensionality and/or improve multicollinearity within the dataset, certain features are deemed redundant and some are combined/transformed to create new features, to form a final set of features on which rest of the analysis is performed. The final set of features chosen in this dataset were - **age, cigsPerDay, BPMeds, totChol, BMI, heartRate, MAP,** and **diabetes_grade**.

*   **Mean Arterial Pressure** was created combining the systolic and diastolic blood pressures
*   **glucose** levels were categorised to create diabetes_grade with four classes, to handle the extreme outliers in the feature
*   **education, is_smoking, prevalentStroke, prevalentHyp,** and **diabetes** were deemed redundant after thorough analysis of the features

### Handling outliers

While all of the data was in the range of possible and realistic values for each feature, some datapoints indicated extremely rare cases which are also medical emergencies. These "outliers" could hamper the model prediction power. Hence, to avoid loss of data, outliers were limitted to maximum and minimum values, through **Winsorising**.

### Iterations

The many hypotheses in all the above pre-processing steps were also tested by creating new datasets whenever an assumption is made. All-in-all, four new datasets were created, and each was considered an iteration to be finally compared with the original dataset. The iterations were:
1. Dropping all data with missing values
2. Using KNN Imputer for imputing the missing values of **glucose**
3. Using Regression Imputer for imputing the missing values of **glucose**
4. Trimming of the outliers instead of Winsorising

### Data Splitting, Balancing and Scaling

After all the above feature engineering, the dataset was split into train and test sets. A 80-20 split ratio was chosen since the total amount of data is less and more training data is provided to the models to learn from

Since the classes in the variable to be predicted were heavily imbalanced, SMOTE was used to balance the classes. Undersampling and Random Oversampling were avoided to prevent loss of data and overfitting of the models respectively

Finally, the datasets were scaled using the MinMaxScaler fitted to the training set.

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

## Model Implementation

Seven models were implemented on the scaled data. GridSearchCV was performed to tune the hyperparameters. RepeatedStratifiedKFold was used for Cross Validation. The models were:
1. Logistic Regression
2. Naive Bayes
3. Decision Tree
4. K-Nearest Neighbours
5. Support Vector Machine
6. Random Forest
7. XGBoost

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

## Model Evaluation

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

## Libraries Used
For handling and manipulating data

<a href="https://pandas.pydata.org/" target="_blank"><img src="https://img.shields.io/badge/Pandas-black?style=flat-square&logo=Pandas&logoColor=white&link=https://pandas.pydata.org" alt="Pandas" width="84" height="25"></a>
<a href="https://numpy.org/" target="_blank"><img src="https://img.shields.io/badge/NumPy-4d77cf?style=flat-square&logo=Numpy&logoColor=white&link=https://numpy.org/" alt="Numpy" width="84" height="25"></a>

For Visualisation

<a href="https://matplotlib.org/" target="_blank"><img src="https://img.shields.io/badge/Matplotlib-afc6d3?style=flat-square&logo=matplotlib&logoColor=white&link=https://matplotlib.org/" alt="Matplotlib" width="78" height="25"></a>
<a href="https://seaborn.pydata.org/" target="_blank"><img src="https://img.shields.io/badge/Seaborn-7db0bc?style=flat-square&logo=seaborn&logoColor=white&link=https://seaborn.pydata.org/" alt="Seaborn" width="65" height="25"></a>
<a href="https://ipython.org/" target="_blank"><img src="https://img.shields.io/badge/IPython-5781b3?style=flat-square&logo=ipython&logoColor=white&link=https://ipython.org/" alt="IPython" width="65" height="25"></a>
<a href="https://graphviz.org/" target="_blank"><img src="https://img.shields.io/badge/Graphviz-9be1f5?style=flat-square&logo=graphviz&logoColor=white&link=https://graphviz.org/" alt="Graphviz" width="70" height="25"></a>

For Hypothesis testing, Pre-processing and Model training

<a href="https://www.statsmodels.org/stable/index.html" target="_blank"><img src="https://img.shields.io/badge/Statsmodels-3f51b5?style=flat-square&logo=statsmodels&logoColor=white&link=https://www.statsmodels.org/stable/index.html" alt="Statsmodels" width="98" height="25"></a>
<a href="https://scipy.org/" target="_blank"><img src="https://img.shields.io/badge/SciPy-0053a1?style=flat-square&logo=scipy&logoColor=white&link=https://scipy.org/" alt="SciPy" width="75" height="25"></a>
<a href="https://imbalanced-learn.org/stable/#" target="_blank"><img src="https://img.shields.io/badge/Imbalanced Learn-c48c4c?style=flat-square&logo=imblearn&logoColor=white&link=https://imbalanced-learn.org/stable/#" alt="Imbalanced Learn" width="140" height="25"></a>
<a href="https://scikit-learn.org/stable/" target="_blank"><img src="https://img.shields.io/badge/Scikit%20Learn-f79939?style=flat-square&logo=scikit-learn&logoColor=white&link=https://scikit-learn.org/stable/" alt="Scikit Learn" width="115" height="25"></a>
<a href="https://shap.readthedocs.io/en/latest/index.html" target="_blank"><img src="https://img.shields.io/badge/SHAP-a84989?style=flat-square&logo=shap&logoColor=white&link=https://shap.readthedocs.io/en/latest/index.html" alt="SHAP" width="45" height="25"></a>

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

## Contact

<a href="https://www.linkedin.com/in/aditya-a-p-507b1b239/" target="_blank"><img src="https://img.shields.io/badge/Linkedin-0078b7?style=flat-square&logo=linkedin&logoColor=white&link=https://www.linkedin.com/" alt="Linkedin" width="85" height="25"></a>
<a href="mailto:apaditya96@gmail.com" target="_blank"><img src="https://img.shields.io/badge/Gmail-red?style=flat-square&logo=Gmail&logoColor=white" alt="Gmail" width="70" height="25"></a>

<div align = "right">    
  <a href="#framingham-cardiovascular-risk-prediction">(back to top)</a>
</div>

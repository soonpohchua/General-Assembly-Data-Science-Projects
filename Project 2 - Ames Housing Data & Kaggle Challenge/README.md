# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 2: Ames Housing Data & Kaggle Challenge

---

## 1. Problem Statement

This project is meant to predict the sales price of houses in [Ames](https://en.wikipedia.org/wiki/Ames,_Iowa), a city based in the state of Iowa of United States from 2005 to 2010. Its main objective is to identify the best predictors of house prices in Ames over the period of prolonged recession.

In summary, the project shall :-
1. perform data cleaning without compromising data integrity;
2. engineer new data from existing data;
3. conduct statistical tests to eliminate unhelpful data; and
4. identify the best predictors of house prices through extensive linear regression modeling.

## 2. Background

From 2005 to 2010, the United States was experiencing an economy downturn. Based on the [data](https://data.worldbank.org/indicator/NY.GDP.MKTP.KD.ZG?end=2021&locations=US&start=2000) from the world bank, the GDP of the United States has been on a downward trend from 2004 to 2009 before making a recovery in 2010. Part of the downward trend was labelled [The Great Recession](https://www.cnbc.com/2020/04/09/what-happened-in-every-us-recession-since-the-great-depression.html), which lasted for about 1.5 years. Despite the recent recession of the United States economy due to the Covid-19 pandemic that lasted for merely two months in 2020, the United States has been [predicted](https://fortune.com/2022/09/21/long-ugly-recession-dr-doom-nouriel-roubini/) to suffer another 'long and ugly' recession like The Great Recession. Hence, to predict the impact of the next recession, the project is inclined to examine the period from 2005 to 2010 as the most recent prolonged recession.

Against this backdrop, the project narrows its scope to the housing market of the city of Ames. Interestingly, Ames was one of those cities which withstood the [general downward trend](https://fred.stlouisfed.org/series/MSPUS) of housing prices over the recession. Despite the drop in median sales price of houses sold in the United States, the house prices in Ames remained [constant](https://fred.stlouisfed.org/series/ATNHPIUS11180Q). The resillence of house prices in Ames was likely due to [sharp increase in population](https://worldpopulationreview.com/us-cities/ames-ia-population) since 2000.

---

## 3. Dataset

The original data dictionary of the dataset can found [here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt).

Alternatively, I have also created a [document](https://www.dropbox.com/s/baklfylvkgngcpt/Proj%202%20Dataset%20Unique%20Values.pdf?dl=0) listing all the unique values of the dataset for a better overview.

The data itself is taken from [Kaggle](https://www.kaggle.com/c/dsi-us-11-project-2-regression-challenge). It comprises of two parts: training dataset & test dataset (used for Kaggle score).

Below is the overview of the dataset used for analysis after Exploratory Data Analysis (EDA).

| Index | Continuous | Discrete | Ordinal | Nominal
|:---:|:---:|:---:|:---:|:---:|
| 1 | Lot Frontage | *Age |  Street  | MS SubClass |
| 2 | Lot Area | *Bsmt Bath | Alley  | MS Zoning |
| 3 | Mas Vnr Area | *Bath | Lot Shape | Land Contour |
| 4 | Total Bsmt SF | Bedroom AbvGr | Utilities | Lot Config |
| 5 | Bsmt Unf SF | Kitchen AbvGr | Land Slope | Neighborhood |
| 6 | *BsmtFin Area | TotRms AbvGrd | Overall Qual | Condition 1 |
| 7 | *Floors SF | Fireplaces | Overall Cond | Condition 2 |
| 8 | Low Qual Fin SF | Garage Yr Blt | Exter Qual | Bldg Type |
| 9 | Gr Liv Area | Garage Cars | Exter Cond | House Style |
| 10 | Garage Area | Mo Sold | Bsmt Qual | Roof Style |
| 11 | Wood Deck SF | --- | Bsmt Cond | Roof Matl |
| 12 | *Size of Porch | --- | Bsmt Exposure | Exterior 1st |
| 13 | Pool Area | --- | BsmtFin Type 1 | Exterior 2nd |
| 14 | Misc Val | --- | BsmtFin Type 2 | Mas Vnr Type |
| 15 | ^SalePrice | --- | Electrical | Foundation |
| 16 | --- | --- | Heating QC | Heating |
| 17 | --- | --- | Kitchen Qual | Central Air |
| 18 | --- | --- | Functional | Garage Type |
| 19 | --- | --- | Fireplace Qu | Misc Feature |
| 20 | --- | --- | Garage Finish | Sale Type |
| 21 | --- | --- | Garage Qual | Yr Sold  |
| 22 | --- | --- | Garage Cond | --- |
| 23 | --- | --- | Paved Drive | --- |
| 24 | --- | --- | Pool QC | --- |
| 25 | --- | --- | Fence | --- |

Note: *Created variables, ^Dependent Variable (Y)

## 4. Key Points from EDA 

1. The training dataset has 2051 rows and 81 columns.
2. 26 columns were found to have missing values. Based on information from data dictionary, the values were all imputed with 0. 
3. Two columns 'ID' and 'PID' were dropped from the dataset as each of the value is unique. 
4. A further 14 columns were dropped to create new features for analysis.
5. As indicated in the diagram above, there are 15 continuous, 10 discrete, 25 ordinal and 21 nominal variables.
6. No changes were required for continuous and discrete variables.
7. Ordinal variables were scaled using numbers from 0 to 8 with 0 indicating that the feature is not present.
8. 17 variables (including the 'Saleprice' target variable) are noted to have outliers (+/- 3 standard deviation). Among them, 2 with error in data entry are corrected. 
9. While outliers are known to skew findings, the inclination is to retain the information as it is highly possible for houses with exceptional areas, sales price etc.

## 5. Key Points from Feature Engineering

1. The training dataset were split accordingly to numerical and nominal features for further processing. 
2. 8 numerical columns were dropped due to stastistically insignificant with p-value more than 0.05.
3. A further 8 numerical columns were dropped due to multi-colinearity with other features due to strong correlation of more than 0.7.
4. 21 nominal features were one-hot encoded to 155 features.
5. 24 one-hot encoded features were dropped due to multi-colinearity with other features due to strong correlation of more than 0.7 or less than -0.7.
6. The result is that training dataset has 164 features, while the test dataset has 154 features.
7. In order to integrate both datas for regression analysis, a further 18 features were dropped from the training dataset and a further 8 features were dropped from the test dataset to ensure that they have the same features.
8. The result is that both training and test datasets have 146 features.

---

## 6. Best Model

After using hyperparameter tuning, the best model is as follows:

| Model Name | Alpha Value | L1 Ratio Value | Mean CV R<sup>2</sup> Score | R<sup>2</sup> Train Score | R<sup>2</sup> Test Score| RMSE Train Score| RMSE Test Score
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| HyperPara: Lasso Regression | 100.927 |  -  | 0.841 | 0.874 | 0.896 | 28194.816 | 25241.134 |

- A total of 70 features are being used. 76 features have been removed.

## 7. Kaggle Score

The above model is able to achieve the following scores on Kaggle submission. Nevertheless, the result still has room for improvement.

##### - Final Kaggle Private RMSE Score: 25190
##### - Final Kaggle Public RMSE Score: 31440

---

## 8. Conclusion

### 8.1 Possible Areas for Improvement

Broadly speaking, regression analysis can always be improved by using more data. However, given the context of our best regression model, there is underfitting. Hence, to improve the results, there is room for the regression increase in complexity. This can be achieved by using a more complex regression model or simply creating more complex features. One possibility is to use the polynomial feature engineering to create more sophisticated features for regression analysis. Another possiblity is also to harness the power of lasso and ridge regression more. A personal learning point in the course of the analysis is that methods of dropping features, such as statistical significant of features with targeted variable and multi-colinearity between features, might not always improve the result of regression. 

### 8.2 Limitation

The analysis has the following limitations: 
1. Outdated dataset from 2006-2010.
2. Data limited to the City of Ames, Iowa. Hence, data cannot be generalised.
3. Data limited to micro factors. Other macro factors such as bank interest rate, demographics of population, population growth, Gross Domestic Product (GDP) of nation/ state, crime rate, government policy etc. should be taken into consideration to make the analysis more robust and holistic.

Nevertheless, as indicated under the problem statement and background, the project aims to analysis the best predictors for Ames during the period of The Great Recession. With a looming recession in view, this project hopes that the findings would be especially useful for prediction during the period even though this will require further research on the extent of its relevance.

### 8.3 Summary

In summary, this project wants to identify the best predictors of house prices through extensive linear regression modeling. This first requires the project to first produce a helpful regression model. The project settles on a best model using the lasso regression. The following are the features that are considered the best predictors of house prices: -

1. Size (Floor*, Garage*, Masonry Veneer*, Porch*)
2. Quality (Overall*, Kitchen*)
3. Age of House^
4. Neighbourhood (Stone Brook*, Northridge Heights*, Northridge*, Edwards^)
5. Building Type (Townhouse End Unit^, Townhouse Inside Unit^)
6. Roof Material (Wood Shingles*)
7. Exterior of House (Stucco^)

Note: Strong positive relationship predictors*, Strong negative relationship predictors^
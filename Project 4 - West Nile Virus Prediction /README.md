<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">

## **Project 4: West Nile Virus Prediction** 
by Soon Poh, Wafir, Desmond and Jonathan

---

### **Contents:**
- Problem Statement and Background
- Data Cleaning
- EDA
- Pycaret: To get best model
- Model Tuning
- Spray Cost Analysis
- Conclusion

### **Problem Statement:**
This project aims to predict West Nile Virus (WNV) in mosquitoes across the city of Chicago for the missing years 2008, 2010, 2012 and 2014. It will make use of the dataset provided by [Kaggle](https://www.kaggle.com/competitions/predict-west-nile-virus/data) consisting of years 2007, 2009, 2011 and 2013.

In summary, the project shall :-

1. perform data cleaning on the datasets provided;
2. provide exploratory data analysis (EDA) on the datasets; 
3. merge the datasets for further analysis;
4. produce classification models to predict the missing years;
5. identify model with the best AUC score through extensive hyperparameter tuning; and 
6. present a cost-benefit analysis on the usage of pesticides to control the population of mosquitoes.

### **Background:**
We are a team of data scientists working for the Disease And Treatment Agency, division of Societal Cures In Epidemiology and New Creative Engineering (DATA-SCIENCE). Due to the rise of WNV cases, we had been tasked to predict WNV in mosquitoes across the city of Chicago so as to derive an effective plan to deploy pesticides throughout the city.

WNV is the most widespread mosquito-borne virus in the United States. Every year thousands of people in United States falls victim to WNV. Among them, [about 5%](https://www.cdc.gov/westnile/statsmaps/cumMapsData.html) lost their lives as a result. Despite its ease of transmission (simply through [the bites of infected mosquitoes](https://www.cdc.gov/westnile/transmission/index.html)), there is no vaccine available to date. Majority of those infected are asymptomatic. About 1 in 5 will develop fever alongside other symptoms such as headache, body aches, joint pains, vomiting, diarrhea, or rash. [Recent studies](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8474605/#:~:text=West%20Nile%20virus%20(WNV)%20is,memory%20loss%2C%20and%20motor%20dysfunction.) have also established that those who survived the infection are often present with long-lasting neurological changes, which includes  depression, memory loss, and motor dysfunction, that can severely impede their lives.

In order to control the spread of WNV, the Chicago Department of Public Health (CDPH) does annual spraying of [Zenivex pesticide](https://www.chicago.gov/content/dam/city/depts/cdph/Mosquito-Borne-Diseases/Zenivex.pdf). Apart from that, CDPH also conducts a comprehensive mosquito surveillance program, which includes placing larvicide in catch basins to limit the number of mosquitoes that can carry the virus, and trapping mosquitoes throughout the city and testing them for the West Nile virus. By using data, the city targets high-risk areas for the virus and keep the residents safe.


### **Data Sets**

There are a total of 4 datasets provided by Kaggle:-

* `train.csv`: Indicating the mosquito traps placed for odd years from 2007 to 2013 with the indication of number of mosquitoes caught and number of WNV instances found
* `test.csv`: Indicating the mosquito traps placed for even years from 2008 to 2014 
* `spray.csv`: Indicating the time and locations of pesticides
* `weather.csv`: Indicating the weather in Chicago from 2007 to 2014

**Data Dictionary**

| Feature | Type | Dataset | Description |
|---------|------|---------|-------------|
| Id      | int  | train.csv/test.csv | ID Number of the record|
| Date    | datetime | train.csv/test.csv | date the WNV test is performed
| Address | datetime | train.csv/test.csv | approximate trap address sent to Geo Coder |
| Species | str | train.csv/test.csv | mosquito species in trap |
| Block   | str | train.csv/test.csv | block number of address |
| Street | str | train.csv/test.csv | street of address |
| Trap | str | train.csv/test.csv | ID number of the trap |
| AddressNumberAndStreet | str | train.csv/test.csv | approximate address retrieved from GeoCoder |
| Latitude | float | train.csv/test.csv | latitude retrieved from GeoCoder |
| Longitude | float | train.csv/test.csv | longitude retrieved from GeoCoder |
| AddressAccuracy | int | train.csv/test.csv | accuracy of information returned from GeoCoder |
| NumMosquitoes | int | train.csv/test.csv | number of mosquitoes in a sample | 
| WnvPresent | int | train.csv/test.csv | whether or not WNV is present in a sample (1 = present; 0 = absent) |
| Date | datetime | spray.csv | date of spray |
| Time | datetime | spray.csv | time of spray |
| Latitude | float | spray.csv | latitude of spray |
|Longitude | float | spray.csv | longitude of spray |
| Station | int | weather.csv | weather station (1 or 2) |
| Date | datetime | weather.csv | date of measurement |
| Tmax | int | weather.csv | maximum daily temperature (F) |
| Tmin | int | weather.csv | minimum daily temperature (F) |
| Tavg | int | weather.csv | average daily temperature (F) |
| Depart | int | weather.csv | departure from normal temperature (F) |
| Dewpoint | int | weather.csv | average dewpoint (F) |
| WetBulb | int | weather.csv | average wet bulb |
| Heat | int | weather.csv | heating degree days |
| Cool | int | weather.csv | cooling degree days |
| Sunrise | int | weather.csv | time of sunrise (calculated) |
| Sunset | int | weather.csv | time of sunset (calculated) |
| CodeSum | str | weather.csv | code of weather phenomena |
| Depth | int | weather.csv | unknown |
| Water1 | int | weather.csv | unkown |
| SnowFall | int | weather.csv | snowfall (inch) |
| PrecipTotal | str | weather.csv | total daily rainfall (inch) |
| StnPressure | int | weather.csv | average atmospheric pressure (inch Hg) |
| SeaLevel | int | weather.csv | average sea level pressure (inch Hg) | 
| ResultSpeed | float | weather.csv | resultant wind speed (mph) |
| ResultDir | int | weather.csv | resultant wind direction (degrees) |
| AvgSpeed | int | weather.csv | average wind speed (mph) |

**Data Cleaning**

The usual data cleaning of checking for shape, data types, missing values and duplicates are done.

Special Mentions are:

**Train Data Set:**

Based on data description provided by Kaggle, a new row entry will be duplicated for every 50 mosquitoes. Hence, there is no concern if the duplicated rows indicate 50 mosquitoes. However, if the duplicated rows do not have 50 mosquitoes, it is likely to be a case of genuine duplication. We combine the rows to aggregate the number of mosquitoes and the number of west nile virus reported. Using groupby, we shall aggregate the number of mosquitoes and the number of west nile virus reported for those rows indicating 50 mosquitoes. However, we will only be using this aggregated dataset for EDA since we do not want to unnecessary reduce the number of rows for modelling. This will also maintain consistency between the train and test datasets; and drop irrelvant features that are not useful for further analysis.

Irrelevent features dropped: Address, Addressaccuracry, Block, Street and Trap. The above columns are removed as they are not useful. `'address'` captures the location of the trap. `'addressaccuracy'`, `'block'`, `'street'`, `'trap'` and `'addressnumberandstreet'` are the outputs of the address sent to Geocoder. As the information of these columns is captured under the `'latitude'` and `'longtitude'` columns, we can proceed to drop them. However, for the purpose of EDA, `'addressnumberandstreet'` will be retained.

The number of rows dropped from 10400 to 8475 after aggregating the number of mosquitoes and the number of west nile virus reported.

In summary, 5 columns and 106 duplicated rows were dropped from the train dataset. A further column will be removed post-EDA.

**Spray Data Set:**

There is the possibility that the duplicated rows in the spray dataset represents multiple spraying on the same day of the same area. However, a closer examination of the data reveals that the time indicated is the same down to the seconds. Hence, it is advisable to drop these duplicated rows. 

Based on the [official website](https://www.chicago.gov/city/en/depts/cdph/provdrs/healthy_living/news/2021/august/city-to-spray-insecticide-wednesday-to-kill-mosquitoes.html) of the Chicago's Public Health, it is indicated that timing of sprays will happen from night time to beyond midnight. The data confirms this. Nevertheless, with the date of sprays indicated, the information of sprays timing is not required for further analysis. Hence, we shall drop the column. 

We checked for the locations in which sprays were administered. Given that there are spray locations that extend beyond the train and test datasets, we shall remove those rows of data that falls out of the range.

In summary, 1 column and 149 rows were dropped from the spray dataset.

**Weather Dataset**

Based on the data dictionary provided, there are the following missing data :-
- 'M' represents missing data
- '-' represents missing data for sunrise/ sunset
- 'T' represents unquantifiable data

It is observed that the `'codesum'` column is actually with missing data. Furthermore, different values for missing data such as ' ' and '  T' were also discovered.

Based on prelimary analysis of missing data, the following features shall be dropped:-
1. `'water1'`,  `'snowfall'`, `'depth'`, `'codesum'` and `'depart'` columns due to the high proportion of missing data;
2. `'sunset'` and `'sunrise'` columns as the timings of sunset and sunrise are unlikely to influence the presence of WNV in mosquitoes; and
3. `'wetbulb'` column as the humidity is also captured by `'dewpoint'` column, which does not have missing data. 
4. `'heat'` and `'cool'` columns as they are more relevant for calculating energy needs for industrial usage rather than the risk of mosquito breeding.

For the rest of the missing data, the following imputation has been done:-
1. `'tavg'` column: Imputed values using mean value of max and min temperatures
2. `'avgspeed'` column: Imputed values based on a different station
3. `'sealevel'` column: Imputed sea level using mean value of previous and next day
4. `'stnpressure'` column: Imputed pressure using mean value of previous and next day
5. `'preciptotal'` column: Imputed values based on medium value

Given that there are two sets of weather data daily from each weather station in Chicago, we shall compute and use the mean values of the weather readings from both since they are relatively close to one another.

As the [life cycle of mosquitoes takes about 8-10 days](https://www.cdc.gov/dengue/resources/factsheets/mosquitolifecyclefinal.pdf ), we shall lag the weather dataset by the average of 9 days.

In summary, 11 columns and 1481 rows were dropped from the weather dataset. Of the 1481 rows, 1472 were due to the combination of data from both weather stations and 9 from lagging the data by 9 days.

**Feature Engineering**

Train:
- Create Year, Month, Week of Year Columns: This breaks down the date into more time-related features.
- Create Hot Spot Column: Based on DBSCAN model, we are able to identify 122 clusters and 51 outliers. The silhouette score of more than 0.85 gives us confidence that these clusters are well defined. We shall proceed to analysis the clusters and outliers in capturing mosquitoes and identifying WNV cases. Of the 122 clusters (excluding the cluster of outliers), 95 clusters have at least more than 1 WNV case while 75 clusters have recurring WNV cases. Of note is cluster 109 which has the highest number of 56 WNV cases among the clusters. Interestingly, the 51 outliers collectively have the highest number of WNV cases. As a cut-off, we shall only consider those clusters with more than 3 WNV cases, which amounts to at least 1 WNV case on average annually, as hot spots. Given that not every cluster has an area of impact (i.e. the coordinates of all the samples in the cluster are the same), we shall use the mean from the maximum and minimum of the latitude and longtitude respectively of the other clusters to compute the area of impact for these clusters. Interestingly, some of the outliers belong to the same location. The reason why they are not clustered together is due to difference in terms of number of mosquitoes captured (recall that the DBSCAN consists of three dimensions - longitude, latitude and the number of mosquitoes captured). Hence, there is a need to combine those outliers with the same latitude and longitude.

Weather:
- Create Humidity Column: As mosquitoes tend to grow in humid weather, relative humidity has been featured-engineered with reference to the [humidity calculator](https://www.calcunation.com/calculator/humidity-calculator.php).
- Create Year, Month, Week of Year Columns: This breaks down the date into more features.

Spray:
- Create Year, Month, Week of Year Columns: In summary, the following features have been created for each dataset:-
* `train`: Created `'year'`, `'month'`, `'weekofyear'` and `'cluster'` columns. Total: 4
* `weather`: Created `'year'`, `'month'`, `'weekofyear'` and `'relativehumidity'` columns. Total: 4
* `spray`: Created `'year'`, `'month'` and `'weekofyear'` columns. Total: 3

Note: Any changes applied to `train` dataset will be applied to `test` dataset subsequently.

**EDA**

After cleaning & combining our datasets, we carry out an initial exploratory analysis to identify some information markers from the data by various plots and graphs to visualize the features better. We used a series of charts for data visualation such as plotly for the map charts, and scatter plots for relationships (eg number of mosquitoes captured and wnv virus). We also plotted the weather features to look at its significance to mosquitoes and wnv.
The Spray dataset was combined with train and was plotted using plotly to give us a visual of the areas with high number of mosquitoes caught, wnv pressense and the spray area.


### **Modelling:**

We used Pycaret to assist us in deciding our model to use on the dataset. 
Our train and test datasets have gone through `OneHotEncoding` for the categorical features in the 1st notebook. Pycaret is able to scale our data using the `normalize=True` parameter and we will be using z-score which is the same as `StandardScaler` (or Z-score normalization). Continuous features like `'relativehumidity'` has a wide range of values and due to differences in the magnitude, our model may neglect units of higher/lower numbers. 

Pycaret is able to detect and classify the features of the dataset. For example, it was able to detect `'year'` as the categorical feature. However, the features that `OneHotEncoding` had transformed was detected to be of also categorical in nature instead. Furthermore, Pycaret will do categorical data encoding if feature is classified as such. To bypass this, we had to list out these features to override the classification before running the dataset through Pycaret environment.

In addition, since the dataset in class imbalance as mentioned earlier, we set the `fix_imbalance=True` parameter in which Pycaret will help alleviate this problem by using Synthetic Minority OverSampling Technique (SMOTE).

The `compare_models` also returned the best performing models based on AUC scores with the `n_select=2` parameter which gives us the top 2 highest performing models with respect to Accuracy, AUC and Recall scores are **Gradient Boosting Classifier** and **Ada Boost Classifier**. Although Light Gradient Boosting Machine has the best AUC score of 0.84 and has Accuracy of 0.90, it's Recall score of 0.37 is far from good to be chosen as a model to generate. In contrast, the 2 models mentioned has a good balance of scores in the 3 targeted scoring metrics for our potential final model.

After narrowing down to these two models, we will attempt to increase its performance by tuning its hyperparameters. Pycaret also has the function to optimize the models generated by using the `tune_model` that helps tune the hyperparameters of the models.

**Gradient Boosting**

We analyzed the performance of the Gradient Boosting Classifier (GBC) model by using the `evaluate_model()` function which displays a user interface for all of the available plots for a given model. It internally uses the plot_model() function. The main focus in this portion is to evaluate the model based on the ROC-AUC score and graph. With an AUC score of 0.85, it suggests the 'west nile virus' and 'no west nile virus' populations are well separated, and the model is nearly as good as it can get. It also shows that the model is better at predicting 'no west nile virus' populations at at lower decision threshold, with the ROC for class 0 being higher and steeper up to around 0.1 False Positive Rate.


From the markdown table below, we see the improvement in the model after the hyperparameter tuning as well:
| Model (parameter) | Train Accuracy Score | Test Accuracy Score | Train AUC | Test AUC | Train Recall | Test Recall | Kaggle Score (Public) |
|---|---|---|---|---|---|---|---|
| Baseline (default)  | - | - | - | - | - | - | - |
| Gradient Boosting Classifier (default) | 0.8281 | 0.8315 | 0.8385 | 0.8474 | 0.5970 | 0.6788 | - |
| Gradient Boosting Classifier (tuned) | 0.8195 | 0.8174 | 0.8456 | 0.8481 | 0.6332 | 0.7030 | 0.7742 |

After our submission to Kaggle with the predictions from the tuned GBC model, the final public score was **0.7742**.

**Ada Boost Classifier**
| Model (parameter) | Train Accuracy Score | Holdout Test Accuracy Score | Train AUC | Holdout Test AUC | Train Recall | Holdout Test Recall | Kaggle Score (Public) |
|---|---|---|---|---|---|---|---|
| Baseline (default)  | - | - | - | - | - | - | - |
| Ada Boost Classifier (default) | 0.8060 | 0.8026 | 0.8314 | 0.8331 | 0.6391 | 0.7152 | - |
| Ada Boost Classifier (tuned) | 0.8158 | 0.8158 | 0.8397 | 0.8391 | 0.6571 | 0.7212 | 0.7793 |

After our submission to Kaggle with the predictions from the tuned Ada Boost model, the final public score was 0.7793.

Below table summarised the final scores for the models for comparison:
| Model (parameter) | Train Accuracy Score | Test Accuracy Score | Train AUC | Test AUC | Train Recall | Test Recall | Kaggle Score (Public) |
|---|---|---|---|---|---|---|---|
| Baseline (default)  | 0.9471 | 0.9471 | 0.5000 | 0.5000 | 0.0000 | 0.0000 | - |
| Gradient Boosting Classifier (default) | 0.8281 | 0.8315 | 0.8385 | 0.8474 | 0.5970 | 0.6788 | - |
| Gradient Boosting Classifier (tuned) | 0.8195 | 0.8174 | 0.8456 | 0.8481 | 0.6332 | 0.7030 | 0.7742 |
| AdaBoost Classifier (default) | 0.8060 | 0.8026 | 0.8314 | 0.8331 | 0.6391 | 0.7152 | - |
| AdaBoost Classifier (tuned) | 0.8158 | 0.8158 | 0.8397 | 0.8391 | 0.6571 | 0.7212 | 0.7793 |

After our submission to Kaggle with the predictions from the tuned ADA model, the final public score was *0.7793*.
Thus, since the ADA model bested both the baseline and GBC model in all scoring metrics, we have chose the AdaBoost Classifier as the final model for our project.

### **Cost-Benefit Analysis of Spraying**

**Cost of Spray**

From our Cluster Analysis, the mean average area of each hot zone cluster is **0.1576 sq km. (0.44536km length and 0.3539km breath)**. 

From external research, the insecticide used to control the adult mosquitoes is [Zenivex™, and it is applied at a rate of 1.5 fluid ounces per acre.](https://abc7chicago.com/archive/9206273/)It is approved for use by the U.S. Environmental Protection Agency and is used to control mosquitoes in outdoor residential and recreational areas.

For its cost, its estimated value is approximately [USD$300 per gallon.](https://www.gfmosquito.com/wp-content/uploads/2017/07/2017-ND-Mosq.-Control-Quotes-Tabulation.pdf)

For 1.5 fluid ounce, it costs around **USD$3.52 per acre**.

For each hotzone cluster, it costs around **USD$136** per cluster, at a conversion rate of 1 acre equals 0.00404686 sq km. 

As our current model defines hotspot as 4 or more cases(i.e. 1 case per year on average), we have identified 59 hot spots. With a cost of **USD$8,024** to spray all the hot spots.

From our clustering analysis, once a cluster is sprayed, it takes approximately 17 days before a new WNV presence to surface again. Therefore, our recommended frequency for spraying at the hot-zones will be every fortnightly. 

As the spraying will be done on a fortnightly basis, we will take the period of June to August - which is approximately 14 weeks, and the total cost for the insecticides for these 14 weeks is **USD$56,168**. 

For a more conservative spraying of every week, the total for spray these hot zone cluster is **USD$112,336**.

There will be additional cost for hiring of trucks and manpower to spray these areas, areas where a large number of mosquitoes are detected through weekly tests, and also for summers which extends deeper into September. 


**Cost of Medical Care**

About 1 in 5 people who are infected with the virus will develop a fever with other symptoms such as headache and joint pains, but about 1 in 150 of those infected develop a serious nervous system illness such as encephalitis or meningitis that typically requires hospitalization.

Mean costs attributable to WNV were USD\\$1177 (95% CI) for acute infection, USD\\$180 (95% CI) for continuing care, USD\\$11,614 (95% CI) for final care - acute death, and USD\\$3199 (95% CI) for final care - late death. Expected 1-year costs were USD\\$13,648, adjusted for survival. Three hundred seventeen infected subjects were diagnosed with at least one neurologic syndrome and greatest healthcare costs in acute infection were associated with encephalitis (USD$710, 95% CI). ([source](https://bmcinfectdis.biomedcentral.com/articles/10.1186/s12879-019-4596-9))

Using this information, a study from a research team from the Centers for Disease Control and Prevention(CDC), calculated the [costs of these subset of patients with additional related medical care and missed worked incurred in the 5 years after the initial infection](https://www.sciencedaily.com/releases/2014/02/140210184713.htm). To estimate the total cost of WNV disease to the nation, the research team extrapolated those costs to the total number of hospitalized cases of WNV disease reported to CDC since 1999. These findings suggest an annual burden of **$56 million** in the United States.

Patients who were hospitalized with acute flaccid paralysis, a partial- to whole-body paralysis caused by WNV infection, had the largest initial and long-term medical costs. All of them required long-term care to help regain lost function, which increased costs. Patients who were hospitalized for meningitis and those hospitalized for fever incurred similar costs of initial hospitalization (median $7,500). Most meningitis and fever patients did not require long-term care.

**Cost/Benefit**

Zenivex, the spray being used, is not harmful to people and pets. And is cheap to purchase. 

The potential medical costs and loss of lives severely outweighs the costs for spraying. Without proper spray control, the West Nile Virus could worsen, and the economic damage to the United States will increase. The benefits can be measured in terms of savings from medical expenses. [A typical household with employer health coverage spends about USD$800 a year in out-of-pocket costs](https://www.chicagobusiness.com/health-care/health-insurance-costs-surpass-20000-year-hitting-record). Benefits would also mean lesser long-term treatment for those who suffer from more serious infection, like [inflammation of the brain (encephalitis) or of the membranes surrounding the train and spinal cord (meningitis)](https://www.mayoclinic.org/diseases-conditions/west-nile-virus/symptoms-causes/syc-20350320#:~:text=In%20less%20than%201%25%20of,High%20fever).

### **Conclusion**

The final model we built was successful in answering our problem statement (predicting the West Nile Virus (WNV) in mosquitoes across the city of Chicago for the missing years 2008, 2010, 2012 and 2014). With a ROC-AUC score of 0.77 from Kaggle based on unseen data, an accuracy score of 0.82, AUC score of 0.84 and Recall score of 0.72 based on holdout test sets, our model is sufficient in its prediction. Thus, we are able to generally predict areas/hotspots with potential WNV presence and with that information, can introduce potential preventive measures to cull the mosquito population and the WNV in Chicago.

Furthermore, based on our cost-benefit analysis, we have deduced that the cost of spraying is not expensive and yet the spraying conducted in 2011 and 2013 in Chicago only covered 1.6% of the cluster hotspot in which we identified. Thus, the cost of spraying is not justified if the effect of spraying does not cover the heavily affected area. 

Unfortunately, this model does not directly achieve the bigger goal of eradicating the West Nile Virus from Chicago. Most of the features in the model are beyond our control, so inference would not directly help in suggesting ways to eradicate the virus. Furthermore, our model is only effective under certain weather conditions (for example hailstorms and fogs) might throw the predictions off. As mentioned, we have no control over the weather and due to climate change, certain weather patterns that may be seen in some years will not be seen in the next (for example in 2013 we found that the humidity was lower and temperature was higher than in 2011). Nevertheless, there are some aspects from our project that may assist in reducing the numbers to the brink of eliminating the virus in the future. Our findings have shown that the effect of spraying has a significant impact on reducing the number of mosquito clusters and WNV since its introduction in 2011 and we do have control over this, unlike the weather.

# Optimizing Cost Efficiency for Project Wolbachia Implementation
**Problem Statement**<br>
Dengue fever is a disease caused by the dengue virus which is transmitted to humans via the bite of an infective mosquito.The risk of dengue has been [persistent](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6085773/#:~:text=The%20first%20outbreak%20of%20dengue,Singapore%20was%20reported%20in%201901.&text=An%20outbreak%20of%20dengue%20hemorrhagic,cases%20was%20recorded%20in%201960.&text=Following%20then%2C%20large%20epidemics%20occurred,mostly%20the%20pediatric%20age%20group.) and [costly](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10021432/#:~:text=We%20estimated%20that%20the%20average,21%2C262%20DALYs%20from%202010%E2%80%932020.) especially to a tropical country like Singapore which might face more severe consequences with the effects of climate change.Project Wolbachia have been a promising and successful project through the release of Wolbachia infected mosquito to suppress and replace the Aedes population. However, it have been [costly and labour intensive to carry out](https://www.channelnewsasia.com/watch/project-wolbachia-sees-success-manpower-and-cost-issues-slowing-down-expansion-video-3340961).

Therefore to expand the scale and thus its effectiveness of the project, NEA will want to strategically time the release of these mosquitos ahead of dengue outbreaks to cut back on cost to up to 80% and also to maintain its effectiveness. To do so, NEA have tasked a group of data scientist to predict approximately 3 to 4 months (i.e 12 to 16 weeks) ahead of alarming dengue cases so that there is [sufficient time for the Wolbachia mosquitos to take effect](https://www.straitstimes.com/singapore/health/about-200m-wolbachia-aedes-mosquitoes-released-from-mosquito-factory-nea).

Objective: To reduce cost by 80% (measured using Cost Benefit Analysis)
- [Cost](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10021432/#:~:text=3.2-,Cost%2Deffectiveness%20of%20Wolbachia%20interventions,-Assuming%20steady%20state): Estimated, under the assumption of steady state, would be **$22.7 million USD per year**.
- Benefit: Averted economic cost from dengue cases which is based on [efficacy levels](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10021432/#:~:text=40%25%20efficacy7,189.25) (calculated as cost averted per [DALYS](https://www.who.int/data/gho/indicator-metadata-registry/imr-details/158#:~:text=Definition%3A-,One%20DALY%20represents%20the%20loss%20of%20the%20equivalent%20of%20one,health%20condition%20in%20a%20population.))
 
- Thus to reduct cost by 80% (under assumption of 80% efficacy), the necessary MAPE is neded to satisfy this equation:
    - `(1-MAPE)X(Median cost averted) = steady-state cost X 1.8 `

- Summary across different level of efficacy that Project Wolbachia have achieved historically

|Efficacy of Project Wolbachia|Median annual Cost(based on 2010 to 2020) averted in USD million|Min. MAPE|
|-|-|-|
|`60%`|`$41.95m`|`2.60%`|
|`70%`|`$48.94m`|`16.51%`|
|`80%`|`$55.93m`|`26.94%`|

**Notebooks**<br>

01_DataCleaning&EDA<br>
02_Modeling<br>


**README Overview**<br>
1. Data<br>
2. Approach/Performance<br>
3. Conclusion<br>

# 1. Data
These are the datasets used for EDA and Modeling:
1. [Daily weather Dataset](https://www.visualcrossing.com/weather/weather-data-services/Singapore/metric/) on the weather climate in Singapore ([Weather data dictionary](https://www.visualcrossing.com/resources/documentation/weather-data/weather-data-documentation/)). 
2. [Dengue Cluster Dataset](http://outbreak.sgcharts.com/data) from SGcharts for geographical location of dengue clusters in Singapore
3. [Weekly Dengue cases](https://data.gov.sg/dataset/weekly-infectious-disease-bulletin-cases) on the amount of reported dengue cases in Singapore.
4. [Weekly Google trend search](https://trends.google.com/trends/explore?date=today%205-y&geo=SG&q=%2Fm%2F09wsg) on the topic of dengue



Summary of dataframe that is mainly worked on:
|Feature/Column|Description|
|---|---|
|No. case|Weekly number of dengue cases|
|temp|Aggregated to weekly by taking mean of daily temperature|
|humidity|Aggregated to weekly by taking mean on the daily amount of water vapor present in the air compared the maximum amount possible for a given temperature, expressed as a mean percentage|---|---|
|precip|Aggregated to weekly by taking mean of daily precipitation that fell or is predicted to fall in the specified time period|---|---|
|precipcover|Aggregated to weekly by taking mean of daily proportion of time for which measurable precipitation was record during the time period, expressed as a percentage|---|---|
|windspeed|Aggregated to weekly by taking daily mean speed of windspeed |---|---|
|winddir|Aggregated to weekly by taking daily mean of the direction of wind|---|---|
|sealevelpressure|Aggregated to weekly by taking daily mean of atmospheric pressure, meansured in millibars|---|---|
|solarenergy|Aggregated to weekly by taking daily mean of sun build up over an hour|---|---|<br>


# 2. Approach/Model performance:

|Approach|Description|
|:---|:---|
|Data size|9 columns, 470 rows|
|Data Cleaning & Processing|<ul><li>Overall duplications check</li><li> Individual feature data check</li><li>Drop redundant and irrelevant features</li><li>Checking for null values</li><li>Renaming of columns</li><li>Aggregating weekly values on weather dataset</li>|
|EDA|<ul><li>Distribution of data</li><li>Monthly trends of weather features</li><li>Correlation between dengue cases and weather features</li><li>Correlation between dengue cases and google trend search</li><li>Geographical visualisation of dengue clusters|
|Modelling|<ul><li>Approach 1: Baseline ARIMA model <ul><li>Use of ARIMA as benchmark model to have base understanding on metrics</li><li>Evaluation/plot of forecasted prediction</li><li>Diagnostics of ARIMA model residuals</li><li>MAPE: 0.31(Do not meet minimum MAPE threshold)</li></ul><li>Approach 2: SARIMAX model</li><ul><li>Exogeneous factors: Weather features</li><li>Evaluation/plot of forecasted prediction</li><li>Diagnostics of ARIMA model residuals</li><li>MAPE: 0.63(Do not meet minimum MAPE threshold)</li></ul><li>Approach 3: Pycaret's ARIMA model <ul><li>Use of 'sliding' cross validation to train model</li><li>Evaluation/plot of forecasted prediction</li><li>MAPE: 0.15(2nd best model, acceptable MAPE with tolerance of 70% efficacy)</li></ul><li>Approach 4: Pycaret's SARIMAX model <ul><li>Use of 'sliding' cross validation to train model</li><li>Exogeneous factors: Weather features</li><li>Evaluation/plot of forecasted prediction</li><li>MAPE: 0.13(Best model, acceptable MAPE with tolerance of 70% efficacy/ additional net benefits of US$7.8million)</li></ul>|

# 3. Concluding statements:

**Discussion**
- Further readings have contributed population density, type of housing and government policies as potential influential factors on the number of dengue cases which might explain why weather factors despite being important, is not as impactful.Explore other factors such as population growth/density to be use for training of the prediction model.

- The model have been predicted mostly on periods without shocks thus the degree of performance loss have yet to be evaluated. Thus, it is needed to regularly train the model with recent data so the early information of shocks is learned by the model which might be able to predict an upcoming shock.

- Removal of the common tokens that appear the most in hateful/non-hateful comments to reduce ambiguity for the model in classifying. This would likely to improve both Precision and Recall scores as it minimizes both False positives and False negatives which will acheive our initial goal of having an auto-classifier without the need of manual filtering.

- The weather features used for training are currently lagged by 2 weeks. Since the targeted prediction is in 12 weeks (i.e 3 months), it implies there is a need to use 10 weeks for forecasted weather data for predictions. Therefore the prediciting capability of the model is dependent also on the accuracy of the 10-day weather forecast.
    
- For more inclusive and accurate cost averted calculation, can collect more information on the economic cost of DHF, a more severe case of dengue fever, and also other related mosquito transmitted diseases such as Yellow fever and Zika.
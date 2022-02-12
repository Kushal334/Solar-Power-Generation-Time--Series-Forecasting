## Solar Power Generation Using Time Series Forecasting
 <br>
With only 12 years left to save ourselves from irreversible climate change damage, we found it to be of utmost importance to carry out research on whether we can predict at a future point in time, the amount of renewable energy generated so we can extrapolate and estimate how much energy we would receive within the limited time that we have and if that would be enough.<br>
Additionally unlike conventional sources of energy, there is an uncertainty in the generation of solar power due to its dependency on weather conditions. One of the most effective ways to cope with the uncertainty of solar power production, is to forecast its expected power. The solar forecasts can then be used by many stakeholders including grid operators, electric utilities, energy traders and facility managers.<br>

## Goal 
<br>
In this project, we aim at exploring various methods for forecasting solar power generation. We focus on short-term forecasting (1 hour or 1 day ahead), using the dataset of aggregated solar power generated collected for Germany, a country that has been implementing an aggressive policy of energy transition (the Energiewende) with a goal of producing all energy from renewable source by the end of the current decade.
<br>

## Data <br>
Source : (https://open-power-system-data.org/data-sources)
<br>
For this project, we have used weather and solar data for this prediction with variables of: <br><br>
**utc_timestamp** : Start of timeperiod in Coordinated Universal Time<br>
**DE_actual_solar_generation** : Sctual solar generation in MW (megaWatts)<br>
**DE_solar_capacity** : Slectrical capacity of solar in Germany in MW<br>
**DE_solar_profile** : Share of solar capacity producing in Germany<br>
**utc_timestamp** : Start of time-period in Coordinated Universal Time<br>
**DE_temperature** : Temperature in Celsius<br>
**DE_radiation_direct_horizontal** : Direct solar radiation in W/m2<br>
**DE_radiation_diffuse_horizontal** : Diffuse solar radiation in W/m2<br>
**DE_precipitation** : Total precipitation in mm/hour<br>
**DE_cloud_cover** : Fraction of cloud cover [0-1] scale<br>
**DE_air_density** : Air density in Kg/m3<br><br>
This dataset consists a total of **64000** rows.<br>

## Data Analysis <br>
Data Analysis on this dataset allowed us to understand the temporal structure of the data and how the generation of solar power varies with time. In particular we were focused on studying the trends and seasonal components of our data.

### Solar Power Generation Over the Years (2012 - 2019)<br>
![Solar Power Generation](https://github.com/Aishwarya4823/Solar-Power-Generation-Time-Series-Forecasting/blob/main/Images/Solar%20Power%20Generation%20Image.JPG)
<br>
<br>

We clearly observe a yearly pattern in solar power production: each year starts with a low production in solar power, then this production starts to increase to reach its peak at the half of the year (summer months) and then it decreases again to reach its lowest value. This is expected as the solar power production depends on solar energy received, which in its turn depends on the season. We also observe more solar power that is being produced over the years, which translates the increasing effort of Germany to produce more solar power.<br>

### Autocorrelation of the Time Series of Solar Power Generation<br>
![Time Series Autocorrelation Image](https://github.com/Aishwarya4823/Solar-Power-Generation-Time-Series-Forecasting/blob/main/Images/Autocorrelation%20Image.JPG)
<br>
<br>
The figure above shows the scatter plots between solar power values and some of its lagged values.We see a strong linear relationship when the lag is 1 hour and this relationship becomes weaker as the lag value starts to increase.<br>

## Data Modelling<br>
### Forecast With Only Weather Features<br>

We first start with exploring how our given weather features can help in forecasting the 1 hour ahead of solar power generation, without including any lagged values. In other words, we address the following question, if at a given hour t, we know the following weather features: solar radiation (direct and diffuse), temperature, cloud cover and precipitation, can we estimate the total solar power that will be produced at t? To enrich our model, we add the following time features: hour, year, month and day.<br>
<br>
![Model 1](https://github.com/Aishwarya4823/Solar-Power-Generation-Time-Series-Forecasting/blob/main/Images/Model1.JPG)
<br>
We see that the random forest and xgboost performed the best between all the models, and had similar performances in terms of average R-squared.
<br>
<br>
### Forecast With Weather Features & Two Lagged Values<br>

We now explore enriching our models with more features: lagged values. We already observed how a value at time t highly correlates with some of its past values. We again use the same features of the previous model and add to them two lagged values. We now focus on the two lagged values (at t-1 and t-2 because they showed the strongest correlation with solar power generation), we will mention in a later section if the addition of more lagged values can help or not.<br>
![Model 2](https://github.com/Aishwarya4823/Solar-Power-Generation-Time-Series-Forecasting/blob/main/Images/Model2.JPG)
<br>
The performance of all models improved and random forest is the winning model in terms of both metrics.<br><br>

### Forecast Without Weather Features, Including Two Lagged Values<br>
We want now to check if we don’t have weather features, will time features and lagged values still able to forecast the next hour solar power production? The reason we’re considering removing the weather features is because in real life, we do not have the real weather features, we have instead their forecasts.<br>
We remove the weather features, keep the time features and test our models. We obtain the following
results:<br>

![Model 3](https://github.com/Aishwarya4823/Solar-Power-Generation-Time-Series-Forecasting/blob/main/Images/Model3.JPG)
<br>
Surprisingly, we see how random Forest and XGBoost have very similar performance (even slightly better here) to when we had weather features. This might suggest that for short term forecasting (1 hour ahead), the lagged values as well as the time features contain enough information to predict for the next hour, so that we don’t rely on weather features. Thus we select this as our **final model**.<br><br>

## Conclusion<br>
In this project, we considered machine learning approaches to perform short term forecasting (1-hour and 24-hour ahead). We noticed that for 1-hour ahead forecasting, time features and lagged values can be used to perform 1-hour ahead forecasting without the need of weather features. However, this is not the case when we wanted to perform multi-step forecasting, where we needed to incorporate weather features.<br><br>
For future works, we would like to investigate the following:<br><br>
• enhance the multi-step forecasting model by examining what additional features could be
added or by proposing different models for each hour; consider 6-hour ahead forecasting as
well<br><br>
• try LSTM (a recursive neural network model) or a hybrid modeling approach<br><br>
• explore the solar power generation for each station in Germany and examine if our findings
are still valid when we consider solar power generated at each station<br><br>
• use the forecast values of the weather features instead of the actual weather features<br><br>
• explore how the predicted values can be used in price or load forecast<br><br><br>

I would like to extend thanks to the **Data Science For All Women Summit 2020** and my team members who I thoroughly learned from and improved along the way:<br>
**Hawraa Salami**<br>
**Fairy Gandhi**<br>
**Arusha Kelkar**<br>






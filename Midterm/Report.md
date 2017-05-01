## Background and Introduction:

The problem under consideration is to be able to accurately predict the precipitation for the  time range between August 2014 and July 2016. This is an assignment problem and does not have an immediate real life implication to the respective government authority; however, the problem is interesting in a sense that it provides the beginners (such as me and other students in this class) an opportunity to wrangle, analyze, model, and predict the precipitation and compare it against the real life data of precipitation. This problem and the Kaggle delivery/evaluation format allows us to practice and hone our modelling skills so that, one day, we will be able to predict the real precipitation with higher accuracy and make a significant impact to the world.

Precipitation data is used in many different industries for a wide varieties of applications. Some interesting applications of the precipitation data are laid out by NASA as, “precipitation data from the Tropical Rainfall Measuring Mission (TRMM) and the Global Precipitation Measurement (GPM) mission reveals new information on hurricane eyewalls and intensity, measures hazard- triggering rainfall events, provides input into climate and land surface models, and offers new insights into agricultural productivity and world health.” In addition to these applications, precipitation data are used by the population to plan their day, vacations, and other activities that is likely to be affected by the chances of rain. An accurate modelling of precipitation data impacts the general population, scientific community, and other entities. Therefore, it is imperative for the model output to be as close to the actual as possible.

The dataset for this exercise was provided by our professor via Kaggle competition post. However, the original data is collected and produced by Global Historical Climatology Network (GHCN). GHCN is a database that collects the daily temperature, precipitation, and snow records of the global land areas. GHCN data is a composite of the data formed by merging and validating data from different sources. There are over 40 variables in the GHCN-daily data set that replaced the older NCDC maintained data sets. GHCN is the world’s largest collection of daily climatological data with over 1.4 billion data values.

However, the dataset we received for this modelling exercise was a dataset with only two columns – Date and Precipitation. The task in hand was to use the historical precipitation data from September 1946 to July 2014, and forecast the precipitation from August 2014 to July  2016.

## Types of Model:

In order to forecast the precipitation from August 2014 to July 2016, we need to build models based on the historical precipitation data, as that is the only variable we have available.  However, since the dataset extends from 1946, we can identify significant trends and seasons in the pattern of precipitation data. Based on these cues, I developed the following different models:
 

#### Model 1: Multiple regression model with monthly and yearly data (Highest Performing Model)

Staying within the limitations and the environment presented in above mentioned data description and constraints, I developed a linear model to forecast the precipitation based on the month and year data. The reason I picked linear model with month and year data is because we had only two columns and rich historical time frame. Linear model with PRCP, Month, and Year variables allowed me to forecast the precipitation with those limited variables.

I started by importing the original data set in R Studio console, and then parsing the data into ‘Month’ and ‘Year’ format. Upon parsing the data, I developed a linear model with Precipitation data and year + month data (PRCP ~ Year + Month). The linear model was then fitted to a plot to visualize and was assigned a prediction interval to understand the fluctuation in prediction. A new data frame was then built to fit the precipitation data for the time interval between August 2014 and July 2016. Upon building the data frame a forecast model was assigned to the data frame by using the linear model built above. ‘Forecast’ package in R was used to perform the forecast. The model resulted in the following output:

### Residuals:

Min	1Q	Median	3Q	Max
-0.1281 -0.0870  -0.0653    -0.0454  4.5916

### Coefficients:
Estimate	Std. Error	t value	Pr(>|t|)
(Intercept)	54.868165	38.106620	1.440	0.150

Residual standard error: 0.3594 on 812 degrees of freedom Multiple R-squared:  0.005263, Adjusted R-squared: 0.002813
F-statistic: 2.148 on 2 and 812 DF, p-value: 0.1174

CV	            AIC	           AICc	           BIC	              AdjR2
1.298567e-01  -1.662857e+03   -1.662808e+03   -1.644044e+03     2.812978e-03

![image](https://cloud.githubusercontent.com/assets/26909910/25594832/56204862-2e90-11e7-8a64-e79fb28e7a18.png)

The model performed fairly well. The indicators above – Adj. R-Squared, AIC, BIC, AICc etc. – show that the forecast performed relatively well given the data set only had dates and precipitation data as the predictor variables. Given more variables, I am sure we can include some of them in the model and optimize the model. Hence, the major limitation of this model is the number of predictor variables. I would like to gather more variables from GHSN for the  above dates, and include the new variables in the model by going through an extensive variable selection procedure. Upon finalizing the variables to be used in the model, I would ensure that the model performed better than what it did with just the dates and precipitation data.

#### Model 2: Multiple regression model with trend and seasonal precipitation

Since the previous model was deficient in number of predictor variables but rich in the extent of historical data, it was worth to test to see if there exist any trends or seasonal patterns. Therefore, I picked a similar linear model but with a variation to include seasonal and trend data. In addition, I also decided to plot the predicted data all the way from the beginning to see how it held against the historical actuals. Finally, I forecasted the data in a plot and also decomposed to see how random, seasonal, trend and observed data lined up. The forecasting model I used was astoc2_ts ~ trend + season, where ‘astoc2_ts’ is the time series data of the original dataset. Below are the output and plots produced by this forecast:

#### Residuals:
 Min	       1Q	       Median	   3Q	     Max
-0.1867   -0.0888   -0.0605    -0.0355  4.5241

#### Coefficients:
   #####       Estimate       Std.	Error	  t value	    Pr(>|t|)  
(Intercept)    7.677e-02	     4.889e-02	   1.570	      0.1167
trend	        -7.048e-05      5.370e-05    -1.313      0.1897
season2        6.742e-02      6.185e-02    1.090       0.2760

Residual standard error: 0.3606 on 802 degrees of freedom
Multiple R-squared:  0.01095,Adjusted R-squared:  -0.003846
F-statistic: 0.7401 on 12 and 802 DF,  p-value: 0.7125

CV	           AIC	            AICc	         BIC	          AdjR2
1.325121e-01  -1.647532e+03	 -1.647007e+03	-1.581687e+03	-3.846425e-03

![image](https://cloud.githubusercontent.com/assets/26909910/25595062/603c8b34-2e91-11e7-9210-0fcfc18fa010.png)

![image](https://cloud.githubusercontent.com/assets/26909910/25595066/636f6664-2e91-11e7-9005-527908e1d758.png)

As evident in the actual versus the predicted data analysis chart, the above model is limited in its ability to forecast the peaks and seasonal trends. The model seems to be smooth throughout whereas the actual has a seasonal pattern to it. Therefore, I am skeptical in adopting the forecast of this model. The prediction interval is also significantly lower than the seasonal peaks. My skepticism was confirmed when I used the output of this model to upload in Kaggle, the score was lower than what I had with the first linear model. In order to further investigate the model, I performed model decomposition and received the following:

![image](https://cloud.githubusercontent.com/assets/26909910/25595089/7e3f7344-2e91-11e7-9648-0fcbcd1fcda9.png)

The furthest I went with this model is forecasting the precipitation up until July 2016 and uploading the forecast data in Kaggle scoreboard to see how it performed in comparison to my previous model. This particular model, as I expected, did not perform well against the previous model. Hence, I dropped the data and used the previous model instead. From this model, I learned that not always the seasonal and trend data result in a better model fit. Actually, in this particular dataset, the linear model without seasonal and trend data performed much well than with the seasonal and trend data.

#### Model 3: Baseline models

I did not, up until this point, split the data into training and test data set and just used the  dataset as a whole. I did, however, want to test to see how a model would perform within the known data set. Therefore, I divided the dataset into 70-30 split, and used the baseline model to train the 70% data and test using the remaining 30% data. I used a similar data preparation steps as I did with the previous models, and used the training and test data sets to perform the forecasting. Upon plotting the training and test dataset, I was able to create a baseline model with the training data, and test the training data against the test data to see how it performed with the in sample data. Below is a chart that shows the distribution of the training and test data:

![image](https://cloud.githubusercontent.com/assets/26909910/25595116/9966ed14-2e91-11e7-96a4-975abc3ac1e8.png)

I used two baseline models – average base line and log linear regression baseline -- to forecast on the training data set and use it on the test data set. The RMSE from average base line model was 0.3252629, and the RMSE from log linear regression baseline was 2.360121. I, obviously, decided to use the 0.3252629 RMSE from average base line model and used the same average for all other months and uploaded in Kaggle. The score, however, was horrible. I am not sure what went wrong here as the RMSE as indicated in my R-studio console is below 0.5 and what I got in Kaggle is over 5. I could not, unfortunately, upload the results from the log linear model to test how it performed because of the number of upload limitations in Kaggle. Please review and run my code below to see what I did and what I could have done different.

Hence, the limitation of the two base line models I tried were that I could not decipher if they were accurate, as the RMSE I received in R-studio console were very good but performed poorly when uploaded in Kaggle. The future work on these models would be to identify what caused  the discrepancy and what could be fixed to resolve the discrepancy. I will explaining the overall learning in the summary section below; however, in this particular model, I learned that what works in R-studio console might not necessarily match to the Kaggle score. In addition, I learned to split the data in training and test data set and run it in a model the way I used to in SAS. That was the biggest take away from this particular model exercise.

## Summary:

In conclusion, even with a simple dataset with just the dates and precipitation data as the two variables, this assignment led me to try out different modelling techniques and study different resources available online. Not to mention, this was my first exposure to R and Kaggle. Overall, I had a great experience and learned a great deal through this exercise. If I were to do it all over again, I would first start with the literature on what is the best way to pursue this, as I did not review the literature until after I built two models. The second step would then be to transform and wrangle data to different forms. I would then use google, R-blogger, Stactoverflow and other sites to see how others have approached a similar modelling problem. I would then jump right into building and testing models. Immediately after building one model, I would export the data to Kaggle to score and compare my score against my peers. I would then iterate this process  until I am satisfied with my score/standings.


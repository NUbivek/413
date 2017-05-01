## Introduction/Background:

The problem under consideration is to be able to accurately predict the sales price  of the houses in the Ames housing dataset. This is an assignment problem and does not have an immediate real life implication to the home buyers in Ames; however, the problem is interesting in a sense that it provides a good medium to practice data and modelling skills acquired overtime in the MSPA program. This dataset acts as an extended version of the popular Boston housing dataset, and provides an opportunity to wrangle, analyze, model, and predict the price of the houses. This problem and the Kaggle delivery/evaluation format allows us to practice and hone our modelling skills so that, one day, we will be able to feature engineer and predict the real prices of the houses with higher accuracy for personal and professional use.

Housing data is significant for many personal as well and professional buyers, government entities, real estate agencies and other companies. Some interesting applications of the housing prices include valuation, risk measurement and management, credit applications, taxation, collateral assessment and financing, among others. Therefore, it is imperative for the model output to be as close to the actual as possible.

The dataset for this exercise was provided by our professor via Kaggle competition link. However, the original data is compiled and produced by Dean De Cock for use in data science education.

## Data Preparation:

This report explains the different models for the home sale price of a defined population of houses using the housing data of Ames population from 2,919 observations. There were a total of 79 variables – including continuous, nominal, ordinal, and discrete – involved in this study.

I have made various transformations to enhance the predicted sale prices. For example, for the purpose of this study I have limited the population to small family houses -- eliminating apartment complex, warehouses, and condominiums data. There are many factors affecting the sale price of the houses in this study as indicated by the list of variables. However, I will be taking multiple approaches to select the appropriate variables, and test to see which combination of variables perform better and which ones don’t. I will be building multiple models with multiple selection of variables to test the fit and predict the sale price of the houses.  I used the goodness of fit (GOF), compared different models and tested the fit. In conclusion, based on my analysis, I was able to deduce a model that worked the best and other models that didn’t work as great for our response variable (y).

For the seven linear models, I used the automated variable selection techniques  such as: forward, stepwise, and backward selection techniques, among other techniques, to test and compare the models’ attributes and performance based on different sets of variables. After selecting the variables, I computed the R-square, AIC, BIC, MSE, and MAE to determine which of the models fit. Finally, I assigned a grading matrix to grade the predictive accuracy of the models and determine material significance.

![image](https://cloud.githubusercontent.com/assets/26909910/25595262/22c00190-2e92-11e7-889f-66558fab609e.png)

In addition, I built Random forest, ARIMA, SVM, and Neural Net models to test how they fit and compare to the linear models above. I determined the individual fit of all the models and reviewed their performance based on AICc, BIC, MSE, RMSE, MAE, Adj. R-
squared, etc..

As stated above, we went through several data transformation steps to refine and tweak the data to meet our modelling objectives. The first data cleaning that I did was to address the missing values by identifying and imputing the missing values. The sample population for this study then gained other generic transformations such as: filtering the data to only include one family houses, houses in residential low density zoning classification, houses that include all public utilities, houses with gentle slope, and houses that have typical functionality. In addition to removing the outliers, and low-volume entries, we checked for duplicate entries using the (unique) function and addressed the entries to make sure the data are consistent across the board. Upon narrowing down the features and creating the feature set we would like to use, the features were then defined for the purpose of this study to fit the requirement of a “typical” sample.

I then calculated the total square ft. of the houses by combining all square ft. variables, and then log transformed to smoothen the curve. Upon log transforming the total sq. ft., I plotted the total sq. ft. data alongside the sales to see how it aligns. I also, then, created a histogram of log sale price and monthly sale price data to understand the normality and overall data. Below are some of my data visualization outputs:

![image](https://cloud.githubusercontent.com/assets/26909910/25595267/26b6e61a-2e92-11e7-9822-274ddb32f11e.png)

![image](https://cloud.githubusercontent.com/assets/26909910/25595271/29f1ed16-2e92-11e7-8884-e5ba6726b914.png)

In order to avoid outliers, I then filtered the data to only include total sq. ft lower than 10,000. I also checked for duplicates, and used the following command to identify and capture categorical and numerical variables separately:

    #Identifying categorical features 
    cat_var <- which(sapply(dfraw, is.factor))
    #Identifying numerical features 
    numeric_var <- which(sapply(dfraw, is.numeric))
    #Capture data with categorical features 
    dfraw_cat <- dfraw[,cat_var]
    #Capture data with numerical features 
    dfraw_cont <- dfraw[,numeric_var]

In addition, I identified how each variable is correlated to other with a correlation function. I then created dummy variables to address the correlation findings. For example, I created a dummy variable for enclosed porch to account for the negative correlation it has on the sale price. I also created factors to identify and address seasonality as shown in the home sales by month histogram above.

Finally, I engineered and selected features based on the above analysis and created a final dataset to use for model building. I prefer to create a .csv dataset and use it as a fresh slate to build models.

## Model type, formulation and performance:

I built a total of 10 models – seven variations of linear regression models and other time series models. However, I had difficulty executing ARIMA and Neural Net models on the given dataset. I am listing the erroneous ARIMA and Neural Net just  as a reference in here. Among all of the models, reviewing the model output and performance matrices, decision tree Rpart model performed the best.

![image](https://cloud.githubusercontent.com/assets/26909910/25595563/2ba0faf2-2e93-11e7-96d7-b96c362fe6e5.png)

![image](https://cloud.githubusercontent.com/assets/26909910/25595567/2e56aff8-2e93-11e7-8fe3-38067f1ba5fe.png)

Above listed diagrams are the general output from the stepwise multivariate model, and their fit, we will discuss each of the models below in their respective sections.  To gauge the overall performance of the models, I used a combination of indicators.

### Model 1: Multivariate Regression W Forward/Backward/Stepwise Selection

Upon clearing up the data and identifying different set of variables to be used, I created a logarithmic linear regression model with select variables using all three automatic variable selection techniques. Below listed is the model output:

#### Forward:

![image](https://cloud.githubusercontent.com/assets/26909910/25595624/6ca70456-2e93-11e7-9b65-dd539eb2e5c8.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25595627/6fd70c20-2e93-11e7-9ea0-b168d9187261.png)

#### Stepwise:

This model performed similar to the forward selection model. Below are the model and its output:

![image](https://cloud.githubusercontent.com/assets/26909910/25595656/81278400-2e93-11e7-82dd-5dbec89da11c.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25595661/84cef11a-2e93-11e7-958d-7f4c73c489e2.png)

#### Backward:

All three selection techniques produced a similar output. Below is the output for backward selection technique:

![image](https://cloud.githubusercontent.com/assets/26909910/25595673/93e53a60-2e93-11e7-841b-13bb12c69839.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25595677/96fc8802-2e93-11e7-8439-f0042624ff72.png)


### Model 2: Decision Tree Rpart Model

I used the decision tree based rpart model to predict the sale price of the houses. I had not built any decision tree models before; therefore, I took it upon myself as a challenge to try out the rpart model and to my surprise this model performed the best in Kaggle, among the ones that I was able to upload. Below are the outputs from the model:

      library(rpart)
      rpartmodel <- rpart(log(total_model$SalePrice[1:1460])~., data = total_model[1:1460,]) 
      rpartprediction <- predict(rpartmodel, total_model[1461:2919,])
      print(rpartprediction)

      pred6transform <- exp(rpartprediction) print(pred6transform) 
      write.csv(pred6transform, "#6Rpart.csv") summary(rpartmodel)

![image](https://cloud.githubusercontent.com/assets/26909910/25595717/d58a4d66-2e93-11e7-9a8d-ebeb41b5dc96.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25595720/d8b3ec7c-2e93-11e7-9ef9-89f64582f8b6.png)

### Model 3: Single variable (Logtotalsf) regression model

I tested different models to experiment and learn along the way. In the quest of my test, I created a total sq. ft. variable by adding all sq. ft. variables, and then log transformed the total sq. ft. to smoothen the curve. Upon log transforming, I built a single variable regression model and tested the fit. The fit was horrible as I expected; however, it was interesting to see how it aligned. Below is the output of the indicators:

![image](https://cloud.githubusercontent.com/assets/26909910/25595756/00d09552-2e94-11e7-96e6-b9a4b89f6390.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25595760/042e9e06-2e94-11e7-9531-7a9d788f1f07.png)

### Model 4: Support Vector Machine (SVM) Model

Upon trying out the decision tree rpart model, I was encouraged to learn another new model. Hence, I wanted to try to use the SVM model and see how it matched up against other outputs. Below are the model and its output:

    library(e1071)
    svmmodel <- svm(log(total_model$SalePrice[1:1460])~., data=total_model[1:1460,]) 
    svmpred <- predict(svmmodel, total_model[1461:2919,])
    print(svmpred) 
    svmfitted <- svmmodel$fitted
    pred7transform <- exp(svmfitted) print(pred7transform) 
    write.csv(pred7transform, "#7SVM.csv") summary(svmmodel)

### Model 5: Random Forest Model

I used the code supplied by Professor to generate the random forest model, but for some reason my random forest did not perform as well as it did for some of my classmates. Below are the codes I used for Random Forest:

    library(randomForest)
    myforest <- randomForest (y=total_model$SalePrice[1:1460],
    x = total_model[1:1460, c(-75)], ntree=1000, replace=TRUE, do.trace = 100, importance = TRUE)
    mypredict <- predict(myforest, total_model[1461:2919,]) print(mypredict)
    write.csv(forestsubmit, "#9myforest.csv") summary(myforest)

In addition to these models, I also prepared a manual selection ensemble model, and attempted to use the time series models (ARIMA and Neural Net) and have attached my codes for your perusal in the codes section.

### Summary – Limitations, future work, and learning:

I used this opportunity to bring together what I learned in 410, 411, and 413, among other classes, and challenge myself to try out different (some new) modeling methods. I learned and used decision tree, and random forest that I had not used before. Also, I leveraged this assignment to translate the data cleaning steps I had learned in 410 and 411 in SAS to the syntax in R. Granted, I ‘googled’ a lot, I also learned to create correlations, dummy variables, features, and categorical separations of the variables, among other things. Therefore, it has been a great experience to bring together everything I learned across classes and programming languages in this exercise. Having said that, however, I was not able to successfully execute ARIMA and Neural Net with this dataset, and that is something that I would like to learn in the future. I am taking machine learning next quarter and I hope to use this foundation to learn more about modeling.

Overall, I had a great experience and learned a great deal through this exercise. If I were to do it all over again, I would first start with cleaning the data up as best as I could, as I had to go back and forth between model creation and data cleaning this time around. The second step would then be to transform and wrangle data specifically for each model. I cleaned the data and transformed the data early on, to have to go back and fix time and again. This was very inefficient. I would also like to get more exposure to Kaggle after this class. I built several different models but despite trying multiple times, I could not quite get in the format Kaggle would like the data to be presented in. Therefore, I hope to fix some of these issues in upcoming class and be a better modeler overtime.

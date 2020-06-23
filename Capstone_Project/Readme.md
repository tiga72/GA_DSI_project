### Capstone Project : Time Series Analysis of Financial Asset Price



#### Executive Summary

A time series is a sequence of numerical data points in successive order. In investing, a time series tracks the movement of the chosen data points, such as a security’s price, over a specified period of time with data points recorded at regular intervals

Time series analysis can be useful to see how a given asset, security, or economic variable changes over time. It can also be used to examine how the changes associated with the chosen data point compare to shifts in other variables over the same time period.

In this project, I looked into using time series models to predict the price of gold and stock market indices. For gold, price prediction is based on ARIMA based models and stock market indices is based on Vector Autoregression model.

There are 3 notebooks in this project, but overall the project can be viewed in 2 parts:

1. Predicting gold price (Notebook 1 and 2)
2. Predicting stock market indices (Notebook 3)

### Define the problem.

COVID-19 pandemic  have caused unprecedented volatility and fear in the financial markets. In mid March  2020, US Fed slashes rates to near zero, rolls out massive stimulus in an emergency response to the COVID-19 threat. Market uncertainty and lower economic growth is expected to drive precious metal demand

On another context, previous research has found a high level of interdependence among national stock markets indices. It investigates whether traders in Asian or European indices futures contracts can profit from these interactions by following the performance in the U.S. equity market.

Our Hedge fund company is trying to re-balance our port-folio on precious metals and stock market indices. Building a price prediction model on these 2 financial asset can help us draw insights to better strategize our port-folio position sizing  

### Getting and cleaning data

Predicting the price of gold in part 1, uses past 50 year(1971 to 2020, about 600 counts), monthly data.  While predicting the price of stock market indices in part 2(final part), uses past 3 year(1971 to 2020, about 660 counts),  daily data.

Most of the data were obtained from yahoo finance, some were obtained from
Federal Reserve Economic Data(FRED) and QuantConnect(algorithmic trading platform)

For Time Series data, it is best practice to set the 'Date' column as the index column and set datatype to Datetime.  In addition, there is also a need to specify the correct frequency of of the dataset. (Example: if data is monthly, set the frequency using keyword 'MS')

### EDA

Augmented Dickey Fuller test (ADF Test) is a statistical test used to test  whether a given Time series is stationary or not. It is implemented across all 3 notebooks to check the data for stationarity

The *Granger causality test*(GCT) is a statistical hypothesis *test* for determining whether one time series is useful for forecasting another. In this project, when modelling with SARIMAX, GCT is used to gauge if the exogenous variable would be suitable in the model. This is also the same case in modelling with Vector Auto-Regression when we are pairing the different sets of time series data together.

The ETS decomposition of time series is a statistical task that deconstructs a time series into several components: trend, seasonality, and noise components.  This is tool that is used in this project to understand underlying trends(it's pattern), seasonal(it's cyclical effects) and residual (the error of prediction)

### Models

<u>Forward stepwise model selection</u><u>(For ARIMA based models and fbProphet)</u>

Part 1 of this project seeks to predict the price of gold (in notebooks 1 and 2) using ARIMA based model and fbProphet. Here, the essence of the modelling is forward stepwise model selection, starting from the most basic model to the most feature rich model, and in the process, accessing the individual models along the process.

Part 2 or the final part seeks to predict the price of stock market indices (in notebooks 1 and 2) using Vector Autoregression.

Here is a quick rundown of these models:

<u>ARIMA based models</u>

Typical Regression uses external factors (independent) as an explanatory variable for the dependent value. ARIMA on the other hand is a time series technique in which a series own history is used as an explanatory variable. ARIMA is a uni-variate model (working with one variable only). SARIMAX is an extension to ARIMA that enhances the model analysis in two ways: First, by adding seasonal component ; Second, the ability to add external influencing factors know as exgenous variables.

<u>FbProphet</u>

The FbProphet is a flexible specification that has a straightforward human interpretation for each of the parameters. A time series forecasting model designed to handle the common features of business time series. It is also designed to have intuitive parameters that can be adjusted without knowing the details of the underlying model.

<u>Vector Autoregression</u>

Vector Autoregression (VAR) is a forecasting algorithm that can be used when two or more time 
series influence each other.  That is, the relationship between the time series involved is bi-directional. It is useful when one is interested in predicting multiple time series variables using a single model

<u>Model parameter tuning</u>

For ARIMA based models, auto ARIMA function is mainly relied upon as a more straightforward and quantitative means to obtain optimal model parameter values. Even with that said, the conventional means such as using ACF and PACF plots are also used to re-affirm the values derived from the auto ARIMA function. 

For fbProphet, SKlearn's parameterGrid is used as tool for parameter hyper-tuning. Essentially, a for-loop runs through the parameterGrid object, fitting different models and the respective RMSE score each time. The best model is the one with the lowest RMSE score.

In VAR models, a for-loop runs through, fitting different models and the respective AIC score each time. The best model is the one with the lowest AIC score.

<u>Time differencing and reverse time differencing</u>

Forward and reverse time differencing is automatically addressed within the ARIMA and fbProphet models but this process is entirely different with regards to VAR. In VAR, the time differencing aspect has to handled manually by code. 

<u>Loss function Evaluation</u>

Statsmodel's RMSE (Root Mean Square Error) is the choice of the loss function across all models in this project. In addition, I looked into and included Prophet's' functionality for time series cross validation to measure forecast error. In essence, it provides an expensive(takes time to run) but in depth error measurement by dividing historical data into different 'intervals/folds', making multiple error measurements depending on the number of 'folds' specified. Concept is quite similar to sklearn's cross val score

<u>Model  Evaluation using AIC</u> 

The Akaike Information Criterion (AIC) test how well your model fits the data set 
without over-fitting it. A lower AIC score is preferred. AIC scoring is the benchmark used. Unfortunately, this was a functionality that was missing in fbProphet model. 



#### Findings & Recommendations

On SARIMAX models(fitted with exogenous variables), the choice to fitted which exogenous variables is based on GCT scores. Overall, model 'sx2'(refer to table below) and 'sx3' have almost on par results with sx2 have slightly better RMSE scores but then poorer AIC(could indicate overfit) 

The fbProphet model with 3 regressors is the equivalent of the SARIMAX model(with 3 exogenous variables). Both statsmodel and febProphet's RMSE scores was lowest amongst all fbProphet models

On Vector Autoregression models, the sp500 and ftse pair performed the best with the lowest RMSE and AIC scores

The models score are as follows:

<u>ARIMA based models</u>

![image-20200621152333761](../../../../AppData/Roaming/Typora/typora-user-images/image-20200621152333761.png)

<u>FbProphet</u>

![image-20200621152223227](../../../../AppData/Roaming/Typora/typora-user-images/image-20200621152223227.png)

<u>Vector Autoregression</u>

![var_summary_table](../misc/var_summary_table.JPG)



<u>Limitations and other considerations:</u>

- The prediction of any financial asset is a classical problem and the factors that affect the price can be broadly fundamental or technical depending on angle of analysis. But many other factors can also come into play, such as market sentiments, macro-economics, news, making predictions ever as challenging
- Gold price prediction fare mediocrely across ARIMA and fbProphet models, when trying to predict into a 24-month period(2018-2020). This could be attributed to somehow an out-of-sample event: Coincidentally, gold price spiked massively from mid 2019, onto levels only seen once in 2013, across the 50 year sample period. Sporadic and erratic price fluctuations makes it extremely challenging to predict.
- Picking an appropriate test period is also a major consideration in any prediction. Pick a shorter period and getting better results could simply be just out of luck. Conversely,  testing on a too long time frame result in the model being not trained properly and with high bias.
- Time series based models come in many different variations from simple AR, MA, ARMA, ARIMA and more complicated ones like SARIMAX. Not Forgetting, variance focused models like ARCH and GRACH models. Lastly, there are also a number of VAR based models.  The wide ranging types of model and possibilities makes it challenging to do a true in-depth analysis
- For an even detailed VAR initial analysis, besides using Granger Causality test, the inclusion of Johanson's Cointegration Test could be use to check two or more time series for cointegration. Results from both tests should be consistent to draw a uniform conclusion to the suitability of the data running VAR models.

<u>Conclusion:</u>

Data modelling is an attempt to solve problems. However no model can correctly model a real life problem. Accurate models can sometimes suffer from lack of domain knowledge, out of sample events, as well as cognitive bias in the form of selection bias, confirmation bias etc.

***"A model is a tool to help us understand the complexities of the universe, and never a substitute for the universe itself."***

- ***Statistician Nate Silver***

Data models helps us in understanding the problem at hand. More importantly, understanding how it’s wrong, and what to do when it’s wrong and minimizing the cost to us when it’s wrong



Sources: 

https://core.ac.uk/download/pdf/4836804.pdf

https://peerj.com/preprints/3190/




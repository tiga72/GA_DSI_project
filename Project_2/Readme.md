### Project 2 - Ames Housing Data and Kaggle Challenge

#### About the dataset

Ames Housing Authority is a public housing agency that serves the city of Ames, Iowa, US. It helps to provide a decent and safe rental housing for eligible low-income families, the elderly, and persons with disabilities The housing authority has collected 79 assessment parameters which describes every aspect of residential homes in Ames. These variables focus on the quality and quantity of the physical attributes of a property. Most of the variables are exactly the type of information that a typical home buyer would want to know about a potential property.



#### Problem Statement

To create a regression model based on the Ames Housing Dataset. This model will predict the price of a house at sale



#### Preprocessing and Modeling:

<u>Looking into features datatypes</u>:

In the decision process to convert dataset's type, MSSubClass, YrSold and MoSold were strong cases 
where I had no doubt the data type had to be changed. The magnitude of their integer values is not correlated to the Saleprice and therefore they are changed from integer to string.

On the other hand, OverallQual and OverallCond are integers originally. Here, integer values are assigned according to ordinal categories (1:Very Poor; 10:Very Excellent etc) which seems appropriate but note that we should not have the assumption that they are equally spaced. Ideally, we should be using dummy variables to allow the data to make the inference The case with these 2 features to change the datatype was somehow strong bearing in mind that model simplicity could be just as important as a model with strong explanatory power (Converting OverallQual and OverallCond into strings results in more complexity as there are more columns after encoding)

With that, I ran the best fit model with and without changing the mentioned datatypes

**Datatype of OverallQual and OverallCond unchanged(integer):**
train_score: 0.9376205271035624
test_score: 0.9311298351869669

**Datatype of OverallQual and OverallCond changed(to string):**
train_score: 0.9405838986391625
test_score: 0.9364787441655245

The model runs better with my orginal recommendations and it got worst result when the datatype of OverallQual and OverallCond were left unchanged



<u>Impute values for LotFrontage with Regression</u> 

Background: 
'LotFrontage' has 330 missing values which is about 16% of 2051 rows
Lot Frontage is boundary between the house and the road

Aim: 
To obtain impute meaningful data for the mentioned missing values based on examination of the relationship between the 3 features

Approach:
Quick multiple linear regression using Gretl

Steps:

1. Feed a csv file to Gretl
2. run OLS model with: 
   Dependent variable: LotFrontage
   interdependent variables: LotArea and stFlrSF (Obtained using correlation method in pandas)
3. File -> 'View as equation..' # to get the MLR equation
   **Equation as from Gretl: ^LotFrontage = 32.1 + 0.00213*LotArea + 0.0143*stFlrSF**
R-squared            0.376486   (low but acceptable value)
Adjusted R-squared   0.375760

<u>Standard Scaler vs Robust Scaler</u>

With the presence of a relative huge and data with outliers, it prompt me further to explore other scaling methods. Robust Scaler scaler are based on percentiles and are therefore not influenced by a few number of very large marginal outliers. However, test runs with the 3 models(Ridge, Lasso and ElasticNet) on Robust Scaler yield a lower R-squared score. The scores convinced me to stick to Stand Scaler for this project

<u>Feature Engineering</u>

This dataset contains many features relates to area<br>
Total area could be represented by = GrLivArea + TotalBsmtSF<br>
It is worthwhile to to examine this in light of the the following:<br>

- Seeing how our model could relate to the new feature
- Using this new feature to further compute the psf(price per square foot)
- get interesting trends from psf

Adding new features could present another way to view and gain better insights into the data but I was mindful that in this case, adding a feature 'total area' (that consist of other area) could be in violation of the no multicollinearity rule. With that, I ran the models with and without the new feature and it turns out that, adding this new feature improves both R-squared score and Root mean squared error but I notice a small penalty was incurred onto Adjusted R-squared.  

Various implemented models and their performance on the validation holdout set are as follows:

| Lasso                   | With totalsf <br>ss scaler | Without totalsf   <br/>  ss scaler | With totalsf <br/>   robust scaler |
| ----------------------- | -------------------------- | ---------------------------------- | ---------------------------------- |
| Root mean squared error | 20,034.554143              | 20,039.677056                      | 25,331.661774                      |
| R^2                     | 0.936511                   | 0.936479                           | 0.898500                           |
| Adjusted R^2            | 0.837469                   | 0.838195                           | 0.740161                           |
| **ElasticNet**          |                            |                                    |                                    |
| Root mean squared error | 20,034.428038              | 20,039.583306                      | 25,331.661774                      |
| R^2                     | 0.936512                   | 0.936479                           | 0.898500                           |
| Adjusted R^2            | 0.837471                   | 0.838196                           | 0.740161                           |



#### Visualization and Insights

The original train data file was modified and ouput to another file name for ease of work when performing visualization. The following was performed:

1. Some features are abbreviated, changed the columns to be more descriptive so it is more intiutive when doing visualization. Features to changed: MSZoning, BldgType and Neighbourhood

2. Based on best fit regression model, we know which the the stronger predictors/features. I went ahead to drop all those columns that I do not need for visaulisation.

Since this part is visualization, I wanted to unclunter and just concentrate on visualization aspect, all python modifications thus was not included in this workbook. 

I utilized Tableau for visualization portion of the work and have included a tableau in the misc folder for reference

NB: All 3 notebooks and sections are hyperlinked for easy navigation

#### Findings & Recommendations

The elastic net regression model has the best performance in the prediction house sales price in Ames. The biggest takeaways from the model was that living area, condition, age, and the location of the houses are are very important factors for sale prices. 

The model could be limited in scope and practicality as it is based on the 4 year housing dataset for the city Ames in Iowa 

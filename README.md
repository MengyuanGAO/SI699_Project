# Make Better Pricing Decisions : Predicting Dish Prices on a Sharing Economy Site
2019 WINTER SI-699

Maintained by [Liangyi Murong](https://github.com/mrly16) and [Mengyuan Gao](https://github.com/MengyuanGAO)

Last Updated: April 25, 2019

## Project Overview
### Motivation
In a nut shell, this project tried to predict the dish prices for different kitchens on a sharing economy site and then provide some dish pricing recommendations to those kitchens to help them improve their business, because pricing is one of the most important decisions a marketer can make since it is the single greatest lever you have to improve profitability.

Specifically, we want to use machine learning techniques to predict dish prices using dish, kitchen and order information at a large scale, and then provide pricing recommendations to help the kitchens make better pricing strategies and help the business grow.

### Problem Definition
The main problem here is to help the kitchens to decide whether they priced their dishes reasonably and make better pricing decisions for their new dishes. So specifically, the prediction task for this project can be defined as, given a new dish for a specific kitchen, we want to predict its price based on other similar dishes and similar kitchens’ information.

To evaluate the performance of our prediction model, we decided to use RMSE(Root Mean Square Error) as our main metric, which can be interpreted as the standard deviation of the prediction errors, so that we can see how concentrated the data is around the line of best fit.

### Data Overview
The data we have access to for this project is mainly transaction data of Air Kitchen, a sharing economy site, including 2 million users, all transactions and profiles as well as 3 months behavioral log. Overall, we mainly used three datasets, i.e., the dish info dataset(380k records), the kitchen info dataset(165k records), and the order info dataset(1.05 million records).

### Pipeline
There are four main parts of our project, firstly we did some Exploratory Data Analysis for initial feature selection and correlation analysis. Then the main part, model setup and improvement, including model selection, parameter tuning, feature selection, and we also try to use collaborative filtering to get better performance, and also error analysis. Then we tried to integrate all of these and also did some complementary exploration to provide better pricing recommendations.

<a href="url"><img src="https://github.com/MengyuanGAO/SI699_Project/blob/master/images/pipeline.png" align="centre" height="250" width="600" ></a>

## Project Implementation
### Phase One: Exploratory Data Analysis
So the first phase of this project is conducting some Exploratory Data Analysis to fully understand the datasets, including analysing each features to pick those features that we thought might have an impact on our predictor and also looked into their correlations. 

<a href="url"><img src="https://github.com/MengyuanGAO/SI699_Project/blob/master/images/correlation.png" height="400" width="400" alt="Feature Correlations" ></a>

<a href="url"><img src="https://github.com/MengyuanGAO/SI699_Project/blob/master/images/Features.png" height="120" width="1000" alt="Feature Construction" ></a>

### Phase Two: Model Fitting
#### Model Setup and Improvement
The first part of model fitting is setup the model and we did a lot of explorations to improve it. Firstly, we tried different algorithms to see which has the best performance, such as Linear Regression, Ridge Regression, Lasso Regression, Polynomial Regression, Decision Tree, Random Forest, and it turns out Random Forest model works the best.

Then we applied different parameter tuning methods such as Grid Search and Randomized Search and got the parameters that gave the best performance. For feature selection, we tried we tried Grid Search Feature Importance, Chi-square, Recursive Feature Elimination(RFE) and Principal Component Analysis(PCA), and it turned out PCA works out quite well. Therefore, by doing this, we got the best model, best parameters and best features for our prediction. 



#### Dish Name Matching
The intuition behind this approach is that dishes with same names should have similar prices, because they are basically the same things. For instance, knowing the prices of all the other _buttermilk fried chicken_ would certainly help to determine a new dish named _buttermilk fried chicken_. So our initial attempt was to predict the test data using the mean price of different unique dish names.

However, the approach appeared to be naive, and it fails to function when the dish names have very minor difference. For example, it recognized _buttermilk fried chicken_ and _homestyle fried chicken_ as completely different dishes, though they might have very similar prices because they are similar to each others in nature. 

To deal with this problem, we assumed that the most determinative factor we can get from the dish name is the ingredient of the dish. We developed a pipeline to tokenize the dish names and filtered out the noun tokens. The mean price of these tokens can be considered as mean price of the ingredients.

<a href="url"><img src="https://github.com/MengyuanGAO/SI699_Project/blob/master/images/dish_name_matching.PNG" width="700" alt="Feature Correlations" ></a>

#### Kitchen Matching
Similarly, it is also helpful to know the prices of all the other dishes in the kitchen where our new dish is from. So the collaborative filtering with kitchen matching comes mainly from the mean price of the dishes from kitchens. Additionally, the city where the kitchen is in and the price of staple in the kitchen also help to determine how different the kitchen is comparing to the others. So we added two bias terms _city bias_ and _staple price bias_ to the prediction.

<a href="url"><img src="https://github.com/MengyuanGAO/SI699_Project/blob/master/images/bias_terms.PNG" width="400" alt="Feature Correlations" ></a>


<a href="url"><img src="https://github.com/MengyuanGAO/SI699_Project/blob/master/images/ktichen_formula.PNG" width="300" alt="Feature Correlations" ></a>

### Phase Three: Model Integration and Performance

Our hybrid system for price suggestion consists of two parts: the tuned Random Forest model in Phase Two serves as a base, and the results of collaborative filtering are added as features to contribute to the overall improvement of the model. 

<a href="url"><img src="https://github.com/MengyuanGAO/SI699_Project/blob/master/images/model_architecture.PNG" width="400" alt="Feature Correlations" ></a>

### Conclusions & Achievement

We created a system of suggesting the price of new dishes based on available dish, user and kitchen information. The project was a cold start problem without sufficient and accurate information, but we managed to break down the problem into solvable parts and improved the model performance step by step. In reality, pricing a new dish will also rely on the marketing strategies and restrictions of the kitchens, which cannot be measured. So, the recommendation our systems provides were the outcome of our best understanding of the available data, and will somehow differs from the truth at some circumstances. 

<br>

_We thank Professor Mei and his PhD student Teng Ye for the mentorship._

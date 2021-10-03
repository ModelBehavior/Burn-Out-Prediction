## Burn Out Prediction
### Data Description
Globally, World Mental Health Day is celebrated on October 10 each year. The objective of this day is to raise awareness about mental health issues around the world and mobilize efforts in support of mental health. According to an anonymous survey, about 450 million people live with mental disorders that can be one of the primary causes of poor health and disability worldwide. These days when the world is suffering from a pandemic situation, it becomes really hard to maintain mental fitness. The Variables are: 

+ **Employee ID**: The unique ID allocated for each employee (example: fffe390032003000)
+ **Date of Joining**: The date-time when the employee has joined the organization (example: 2008-12-30)
+ **Gender**: The gender of the employee (Male/Female)
+ **Company Type**: The type of company where the employee is working (Service/Product)
+ **WFH Setup Available**: Is the work from home facility available for the employee (Yes/No)
+ **Designation**: The designation of the employee of work in the organization. In the range of [0.0, 5.0] bigger is higher designation.
+ **Resource Allocation**: The amount of resource allocated to the employee to work, ie. number of working hours. In the range of [1.0, 10.0] (higher means more resource)
+ **Mental Fatigue Score**: The level of fatigue mentally the employee is facing. In the range of [0.0, 10.0] where 0.0 means no fatigue and 10.0 means completely fatigue.
+ **Burn Rate**: The value we need to predict for each employee telling the rate of Bur out while working. In the range of [0.0, 1.0] where the higher the value is more is the burn out.

### Inspiration
Try to build some really amazing predictions keeping in mind that happy and healthy employees are indisputably more productive at work, and in turn, help the business flourish profoundly.

### Split Data
Before modeling, the data was split into a training and testing set, and a seed was set for reproducibility. 80% of the data were randomly allocated to the training set. 10-fold cross-validation is used to split the data for tuning and evaluation of training models.

### Initail Data Analysis
Strong relationships can be seen in several of the plots. Resource allocation and designation seems like they can be encoded as a categorical variable. This is hard to see from the plots above, but individual plots of the variables show this. In the plot for mental fatigue score, we can see the fitted line goes below zero, which is problematic since the observed values of burn rate cannot be negative. This could be helped with a transfomation. Looking at the violin/boxplots, we can see that company type as pretty symmetrical distribution with respect to burn rate, as well as identical medians. Gender and wfh setup available appear to have skewed distribution with respect to burn rate and unequal medians. **NOTE: This is not a hypothesis test and could be different**.

![](https://github.com/ModelBehavior/Shawn_Portfolio/blob/main/images/burn_out_eda.png)

# Metrics and Controls
The metrics used to investigate the models are RMSE, R-squared, CCC, and MAE.

# Linear Models
OLS, ridge, LASSO, elastic net regression was used on the data. The tuning parameters for the penalty models was found using 10-fold cross-validation over a random grid of 20 values.

### PreProcess
The numeric predictors were transformed using Yeojohnson transformations, centered and scaled, dummy variables were made for the nominal predictors, near-zero
variance filter was done on the predictor's space, and a correlation filter was done on the predictors.

# Non-Linear Models

### KNN
KNN was tuned over the number of neighbors (k) in the range 1-10 inclusive.

### MARS
MARS model was tuned over the degree of polynomial 1 or 2 and the number of terms to keep in the model.

### NNET
The single-layer neural network model tuned over the number of hidden units [1,10], dropout, weight decay, and 500 training iterations.

### SVM
A radial basis support vector machine tuned over cost, sigma, and margin with 20 randomly spaced values.

### Preprocess 
The preprocessing of the data for the non-linear models included centering and scaling numeric predictors, making dummy variables for all nominal predictors, and checking for near-zero variance predictors.

# Tree Models

### Random Forest
A random forest model, with mtry = 5, number of trees = 1000, and 20 random values of min_n

### Cubist Model
A Cubist Model tuned over the number of committees, neighbors, and rules. 50 evenly spaced random values were chosen for the grid.

### Boosted Tree
A boosted tree model was tuned over min n, tree depth, learn rate, loss reduction, and sample size. A grid of 50 evenly spaced values was chosen for the grid.

### Preprocess
Little preprocessing was done for the tree models. Dummy variables were made for the nominal predictors with one-hot encoding, and a near-zero variance filter for the predictor space. 

# Training Model Results
We can see that neural network, cubist, and boosted tree models are all within standard errors of each other. We can do a repeated-measures ANOVA to check if there are any differences between the models or use a paired t-test.

![](https://github.com/ModelBehavior/Shawn_Portfolio/blob/main/images/burn_out_train_res.png)

# Results 
We get a test set RMSE of 0.0534458.

# Data-Society-Case-Study
Eells Case Study on Energy Consumption for Data Society's Final Interview 

## Description
The accompanying Python notebook forecasts the expected active (real) power in January 2019 to determine if utilization is lower year over year and month over month. After assuming logistic growth and holiday impacts, it searches a hyperparameter space of 54 options. Note: it takes approximately 7 minutes to evaluate a single model within the hyperparameter space.    
The notebook also includes exploratory data analysis of active real power by load type and numerous time variables (e.g. day of week, time of day, day of month, etc.).  

## Data
The analysis leverages the "Steel Industry Energy Consumption" data set from [Kaggle](https://www.kaggle.com/datasets/csafrit2/steel-industry-energy-consumption/data).  
The relevant fields are Usage_kWh & Date.  

## Model Specification
**Target Variable**: Log(Usage_kWh + 1)  
**Model**: [Prophet](https://facebook.github.io/prophet/)  
**Parameters**:   
{  
'growth': 'logistic',  
 'holidays': [South Korean Holidays](https://pypi.org/project/holidays/),  
 'seasonality_mode': 'multiplicative',  
 'changepoint_prior_scale': 0.001,  
 'seasonality_prior_scale': 0.01,  
 'holidays_prior_scale': 0.01,  
 'yearly_seasonality': False,  
 'weekly_seasonality': True,  
 'daily_seasonality': True,  
 'interval_width': 0.9  
 }


## Evaluation
**Metric**: Root Mean Squared Error (RMSE)  
**Horizon**: 31 Days  
**Period**: 16 Days  
**Window**: 10%  

## Usage
The hyperparameters explored and their evaluation metrics can be found in evals.    
The best model is stored as best_model_log.  
Predicted values and the 90% confidence intervals of the best model are stored in best_model_log_cv. Note: values are still in log form, so they must be converted back with e^(yhat) -1 to be interpretable  
All evaluation metrics of just the best model are stored in best_model_log_p.  
All actual and predicted values in log and base form are stored in best_model_df.  
Predictions can be made by extending the time window with [make_future_dataframe](https://rdrr.io/cran/prophet/man/make_future_dataframe.html) and predicting upon the extended timeframe.  
Evaluation of different time windows can be done by editing the [cross validation](https://facebook.github.io/prophet/docs/diagnostics.html) function. Note: Cuttoff will need to be used to start at a different timeframe.


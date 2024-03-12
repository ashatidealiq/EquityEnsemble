# Project: EquityEnsemble

This project uses a collection of deep learning models to assess the impact of different types of data on equity prices. 

The three types of data are:

1. News data
2. Technical data
3. Fundamental data

This README describes the scope of the project including a system design and methodology, final output, and validation and success metrics and methodology.

### System Overview

Given the distinct nature of each data type we propose building three separate data pipelines and deep learning models.

![Alternative Text](https://github.com/ashatidealiq/EquityEnsemble/blob/main/pipeline.jpg)

Splitting the pipelines up like this will improve the interpretability of our end predictions (making it easier to interpret our results vs a black box.) Each pipeline will measure the correlation of its corresponding data type with the underlying price action (based on historic data) as well as return error measures for the model predictions (MSE, RMSE etc..).  

The increased interpretability also helps us compare the impact of the different data types in a meaningful way. This could help us build a decision system that integrates outputs from the three models to determine their overall impact. This system could involve weighting outputs based on historical accuracy for example (TBC)

### Data Inputs

Each pipeline will source data from publicly available datasets (TBC) and transform the data for the requirements of the individual models (we call this transformation "feature engineering.") 

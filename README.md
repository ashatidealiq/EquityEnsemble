# Project: EquityEnsemble

This project uses a collection of deep learning models to assess the impact of different types of data on equity prices. 

The three types of data are:

1. News data
2. Technical data
3. Fundamental data

This README describes the scope of the project including a system design and methodology, final output, and validation and success metrics and methodology.

### System Overview

Given the distinct nature of each data type we propose building three separate data pipelines and deep learning models.

Splitting the pipelines up like this will improve the interpretability of our end predictions (making it easier to interpret our results vs a black box.) Each pipeline will measure the correlation of its corresponding data type with the underlying price action (based on historic data) as well as return error measures for the model predictions (MSE, RMSE etc..).  

The increased interpretability also helps us compare the impact of the different data types in a meaningful way. This could help us build a decision system that integrates outputs from the three models to determine their overall impact. This system could involve weighting outputs based on historical accuracy for example (TBC)

![Alternative Text](https://github.com/ashatidealiq/EquityEnsemble/blob/main/pipeline.jpg)

### Data Inputs

We propose building the model with freely available data from the following public APIs:

News Data: Marketaux, Yahoo Finance
Technical Data: Alpha Vantage, Quandl
Fundamentals: Finnhub

Note: As a proof of concept we have a preference for quantity over quality. Our aim is to build an end to end model that works and has a reasonable level of interpretability, in a relatively short time frame. These 5 data sources should cover these requirements.

### Feature Engineering

Each pipeline will source data (via API or scraping) and transform the data for the requirements of the individual models - this transformation "feature engineering." 


The following transformations are based on core assumptions about the system's functionality:

Calculate ratios for fundamentals (eg PE, P2B)
Event datetime to ensure relevance 
Calc RSI, SMA etc for technicals
Label sentiment for news data




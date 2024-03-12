# Project: EquityEnsemble

This project uses a collection of deep learning models to assess the impact of different types of data on equity prices. 

The three types of data are:

1. News data
2. Technical data
3. Fundamental data

This README describes the scope of the project including a system design and methodology, final output, and validation and success metrics and methodology.

### System Overview

Given the distinct nature of each data type we propose building three separate data pipelines each with their own separate deep learning models. 

Each pipeline will measure the correlation of its corresponding data type with the underlying price action (based on historic data) as well as return error measures for the model predictions (MSE, RMSE etc..).  

Splitting the pipelines up like this will improve the interpretability of our end predictions. (ie: it makes it easier to "interpret" the results of our models vs building a black box.) 

The increased interpretability also helps us compare the impact of the different data types in a meaningful way, and enables us to build a decision system that integrates outputs from the three models to determine their overall impact. This composite system could involve weighting the three separate outputs based on historical accuracy for example (TBC)

The overall system design is as follows.

![Alternative Text](https://github.com/ashatidealiq/EquityEnsemble/blob/main/pipeline.jpg)

### Data Inputs

We propose building the model with freely available data from the following public APIs:

News Data: Marketaux, Yahoo Finance
Technical Data: Alpha Vantage, Quandl
Fundamentals: Finnhub

Note: As this is a proof of concept we have a preference for quantity over quality. The aim here is to build an end-to-end model that works and has a reasonable level of interpretability, in a relatively short time frame. These 5 data sources should cover our requirements.

### Feature Engineering

Each pipeline will source data via API or scraping (python scripts) and transform the data for the requirements of the individual models - this transformation "feature engineering." 

The process is basically as follows: 

1. Retrieve the data via vendor API (in JSON format) and csv and store in cloud storage in a simple folder based file system.
2. Use hosted Jupyter Notebooks (with python) for initial preprocessing and feature engineering.
3. Upload transformations code to Hopsworks (Hopsworks is an open source platform for orchestrating our feature engineering and hosting our deep learning models)

The following transformations are worth noting as they are based on core assumptions about the system's functionality:

#### - Calculate Ratios for Fundamentals 

(eg PE, P2B)



#### - Calculating Event Datetime 

Calculating an appropriate event datetime on stock prices, fundamental indicators, news events and technical indicators is necessary to ensure we are comparing relevant events to price moves. For example if we get a negative news article about TSLA on March 12th 2024, we should look for the impact this has on the next available price datetime after that news article datetime - the March 12th close. 



#### - Calc RSI, SMA etc for technicals


#### Label sentiment for news data




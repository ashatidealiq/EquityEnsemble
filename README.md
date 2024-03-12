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

  #### News Data: 
  - Marketaux 
  - Yahoo Finance
  
  #### Technical Data: 
  - Alpha Vantage
  - Quandl
  
  #### Fundamentals: 
  - Finnhub

Note: As this is a proof of concept we have a preference for quantity over quality. The aim here is to build an end-to-end model that works and has a reasonable level of interpretability, in a relatively short time frame. These 5 data sources should cover our requirements.

### Test Data Selection

We suggest a small initial selection of 18 US tech stocks for our model (TBC) A smaller test set lets us manage larger historical time series while still using the free API accounts and is more manageable for developing the model. We chose a mix that covers a range of market cap, sub-sectors, and growth stages. This allows us to view how different factors might impact the individual names  companies.

- **Apple Inc. (AAPL):** A leader in consumer electronics and technology.
- **Microsoft Corp. (MSFT):** Dominant in software, cloud computing, and personal computing.
- **Amazon.com Inc. (AMZN):** Major player in e-commerce and cloud services.
- **Alphabet Inc. (GOOGL):** The parent company of Google, leading in search engines, digital advertising, and more.
- **Facebook parent Meta Platforms Inc. (META):** A leader in social media and digital communication.
- **Block Inc. (SQ):** Innovator in financial services and mobile payments.
- **Shopify Inc. (SHOP):** E-commerce platform that powers businesses of all sizes.
- **Zoom Video Communications, Inc. (ZM):** Became essential for video communication in the remote work era.
- **Palantir Technologies Inc. (PLTR):** Specializes in big data analytics.
- **Roku, Inc. (ROKU):** Streaming media hardware and platform services.
- **Lemonade, Inc. (LMND):** Technology-driven insurance company leveraging AI and big data.
- **Plug Power Inc. (PLUG):** Innovator in alternative energy technology focusing on hydrogen fuel cell systems.
- **NVIDIA Corporation (NVDA):** Leading chipmaker in GPUs for gaming and AI.
- **Advanced Micro Devices, Inc. (AMD):** Competes in CPUs and GPUs, crucial for computing and gaming.
- **Intel Corporation (INTC):** Major player in CPU manufacturing and technology innovation.
- **Tesla, Inc. (TSLA):** Electric vehicles and clean energy products.
- **Netflix, Inc. (NFLX):** Streaming television and films.
- **Sony Group Corporation (SONY):** Consumer and professional electronics, gaming, entertainment, and financial services.

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




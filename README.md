# Project: Deep Learning Ensemble For Equities Market Valuations 

This project uses a collection of deep learning models to assess the impact of different types of data on equity prices. 

The three types of data we are interested in are:

1. News data
2. Technical data
3. Fundamentals data

This README describes the scope of the project including a discussion of the system design and methodology, data requirements, final output, and validation and success metrics methodology.

### System Overview

Given the distinct nature of each data type, we propose building three separate data pipelines, each with their own separate deep learning models. 

Each model will return closing price predictions for the underlying instruments as well as error measures for its model predictions (MSE, RMSE etc..). After feature engineering (see *Feature Engineering* below) each pipeline will also show the correlation of its corresponding data with the underlying price action (based on historic data.) 

Here's a visual overview of the high level system design.

![Alternative Text](https://github.com/ashatidealiq/EquityEnsemble/blob/main/pipeline.jpg)

Splitting the pipelines into 3 separate flows like this will improve the interpretability of our end predictions - ie: it makes it easier to "interpret" the results of our models vs building a black box.

This increased interpretability helps us compare the results of the different pipelines in a meaningful way. It also enables us to build the decision system that integrates outputs from the three different models (See *Data Output* below). 

## Data Inputs

We propose building the model with freely available data from the following public APIs:

  - **News Data:** Marketaux, Yahoo Finance
  - **Technical Data:** Alpha Vantage, Quandl
  - **Fundamentals:** Finnhub

As this project is a proof of concept we have a preference for *quantity over quality*. One of our main objectives is to build an end-to-end model that works and has a reasonable level of interpretability, in a relatively short time frame. These 5 data sources should cover our requirements.

## Test Data Selection

We suggest a small initial selection of 18 US tech stocks for our model (TBC). 

This smaller test set lets us work with large historical time series while still using free API accounts, and is more manageable for developing the model. 

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

We chose a mix of instruments that covers a range of market cap (AAPL vs PLUG), sub-sectors (PLTR vs NFLX), and growth stages (MSFT vs LMND). This mix allows us to observe how different factors might impact individual names (news and earnings for instance) while observing technicals and fundamentals that might impact the whole sector (like a secular trend or short squeeze).

## Feature Engineering

Each pipeline will source data via API or scraping (python scripts) and transform the data for the requirements of the individual models - this data transformation is called "feature engineering." 

The process is basically as follows: 

1. Retrieve the data via vendor API (in JSON format) and CSV and store in cloud storage in a simple folder based file system (easiest to use mine but I will provide setup at the end of the project).
2. Use hosted Jupyter Notebooks (with python) for initial preprocessing and feature engineering.
3. Upload transformations code to Hopsworks (Hopsworks is an open source platform for hosting and orchestrating our feature stores, and evemtually hosting our deep learning models)

The following transformations are worth noting as they are based on core assumptions about the system's functionality:

#### - Calculate Ratios for Fundamentals 

We may need to engineer financial metrics such as P/E ratio, ROE etc from financial reports to ensure comparability across different companies. TBC based on data quality and comparability as these may be more consistent given the names we have chosen for our test data. 

#### - Calculating Event Datetime 

Calculating an appropriate event datetime on stock prices, fundamental indicators, news events and technical indicators is necessary to ensure we are comparing relevant events to price moves. For example if we get a negative news article about TSLA on March 12th 2024, we should look for the impact this has on the next available price datetime after that news article datetime - the March 12th close. 

#### - Calculate Technicals 

Keeping the number of technicals we engineer low helps keep the model simple and our feature engineering pipeline relatively fast. We chose the following indicators for model training (TBC) based on the simplicity of their calculation from historical datasets and their (possible) impact on price action.

- **MACD:** shows the relationship between two moving averages and can help identify momentum.
- **RSI:** measures the speed and change of price movements. This info could train the deep learning model to identify overbought and oversold conditions.
- **Bollinger Bands:** A quasi volatility indicator that consists of a middle SMA along with two standard deviation lines above and below it. These might help the model learn about price breakouts and outliers and how to identify them during conditions of varying volatility.

#### Label sentiment for news data

We will use natural language processing to label articles with a sentiment "buy", "sell" or "hold" (TBC) depending on the testing. Named entity recognition helps identify and label articles precisely targeted to specific companies. This level of sentiment analysis is TBC depending on performance and the quality of the data given that we will be working with a reduced set of companies for testing. 

## Data Output

We will train and validate each model on separate datasets (60/40). The validation process compares prediction values to actual values and returns error measures for these predictions (Mean Squared Error, Root Mean Squared Error etc..). 

We have a couple of options to derive the final 100% composite number. This composite number could involve weighting the three separate outputs based on historical accuracy and error measures (TBC) or we could train a fourth machine learning model to learn the best combination of inputs.





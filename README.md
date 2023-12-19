# Project Summary


This projects creates a robust forecast that allows for the creation of hundreds of time-series forecasts, with analysis of performance at different combinations of granularity, time-horizon, and algorithms. 

## Background
Say we are a book publishing corporation, and we have 434 books written by 110 authors, under 20 publishing compaines, across 7 cities. We want to generate forecasts that are flexible enough that so that we can analyse the forecast at **(1) each of these levels of the hierarchy (book > author > publisher > city)**, across **(2) 1, 3, 6, 9, and 12 months time horizons**, as well as **(3) test 14 different statistical algorithms** to find the best one. 

For example, we want to be able to create a pipeline that gives us the following insights:
- Book 1 has a forecast of X, with a Y% error rate for a 9 month horizon
- The best algorithm to use for author Z is algorithm 2, followed by algorithm 3, then algorithm 6.
- While we have an X% average forecast error for each book, we have a Y% average forecast error for the city.
- We have a Y% confidence in our forecast for a 6 month horizon, vs Z% confidence for a 12 month horizon.

As you can imagine, doing this with hundreds of individual books, with multiple algorithms, multiple time-horizons, all while generating cross-validated accuracy numbers for each case can be an extremely tedious process. But with some useful python packages and critical thinking on designing the pipeline, it can be done.

## Dataset
- A dummy book revenue dataset ('data.csv') containing 434 book ids, as well as their revenue (y) from September 2020 to September 2023 (ds). 
- A dummy book hierarchy dataset ('dimensions.csv') containing a hierarchical map for each book, it's author, publishing company, and city.


## The Approach:
We use Nixtla's StatsForecast package which allows us to do forecasting due to its speed and flexibility. We can quickly swap in out various classical statistical forecasting algorithms, as well as easily generating cross validation for each stage. 

In this projects, we choose 14 algorithms to experimentally test, including a few Naive baseline models for comparison:

1. AutoARIMA
2. AutoETS
3. AutoTheta
4. HoltWinters
5. SeasonalExponentialSmoothingOptimized
6. MSTL
7. DynamicOptimizedTheta
8. CrostonClassic
9. CrostonOptimized
10. RandomWalkWithDrift
11. HistoricAverage
12. WindowAverage
13. SeasonalNaive
14. Naive

We then do a series of data wrangling transformation to generate the insights that we are looking for. The key is that since, we can aggregate revenue up, (e.g from a book level to and author level), but not down, we need to forecast at the finest grain of data, that is, one time series per book, per algorithm, for 12 months out.

## Output
The result is a robust system that creates a few folders with summarized outputs of the pipeline:

 1. A forecast directory contains forecasts that combines forecasted and historical data into various dataframe formats useful for further manipulation or to upload to a cloud store.
 2. A crossvalidation_logs directory containing crossvalidation logs that are unsummarized for detailed crossvalidation results.
 3. A crossvalidation directory containing crossvalidation summaries by algorithm and by time horizon
 
This allows us to easily look up crossvalidation results.

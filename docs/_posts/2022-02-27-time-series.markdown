---
layout: post
title:  "Time Series Forecasting"
date:   2022-02-27 21:43:38 -0800
categories: topics
---
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 

## Applications
Stock price prediction, Words separation in speech, Autocorrelated series (ex: $$v(t) = 0.99 * v(t-1)$$) etc. The spikes in the image below are sometimes called as '*innovations*', which can't be predicted based on past values.
{:refdef: style="text-align: center;"}
![img0](https://www.dropbox.com/s/j78qlmkedjqxpwd/Screenshot%202022-02-27%20at%209.08.22%20PM.png?raw=1){: width="75%" }
{: refdef}

Unlike normal ML, in time series, more data may not always mean better model training. Example, if there was a big event that changed the course of a stock price forever, we would better be off training on samples *after* the event took place. The samples before the event will work against us as they had a different trend altogether.

{:refdef: style="text-align: center;"}
![img1](https://www.dropbox.com/s/yvgqmmcsfz8vk5l/Screenshot%202022-02-27%20at%209.05.49%20PM.png?raw=1){: width="75%" }
![img1](https://www.dropbox.com/s/1ww3jq9mjr78yfn/Screenshot%202022-02-27%20at%209.13.21%20PM.png?raw=1){: width="75%" }
{: refdef}

## Splitting Data for Training

To test the performance of a forecasting model: split the data as below, train on training period, choose hyperparameters using validation period.
{:refdef: style="text-align: center;"}
![img0](https://www.dropbox.com/s/coqmxvz4lbkd3z7/Screenshot%202022-02-27%20at%209.21.09%20PM.png?raw=1){: width="75%" }
{: refdef}

Once you have chosen the hyperparams, you can re-train on both the train and validation periods, and test on the test period to see if the model performed well enough.

In contrast to normal ML, after above step, we re-train again on the entire data *including the test period* finally. This is because the test data is the closest data we have to the current point in time, and as such is the strongest signal in determining future values. Sometimes the test period can be in the future as well.

Another partitioning method is the Roll-Forward partitioning as shown below. Here we start with a small train period, and gradually increase it, say 1 day at a time, at each iteration, we train the model on the current training period and forecast the following day in the validation period. It's like doing the fixed partitioning a number of times to fine tune.

{:refdef: style="text-align: center;"}
![img0](https://www.dropbox.com/s/qh676rrgm5f55xm/Screenshot%202022-02-27%20at%209.26.21%20PM.png?raw=1){: width="75%" }
{: refdef}

## Moving Average Forecasting

One simple algorithm of forecasting is taking the Moving average over an averaging window. It does remove lot of the noise and emulates the original series, but it does NOT anticipate trend and seasonality.

{:refdef: style="text-align: center;"}
![img0](https://www.dropbox.com/s/1td2tm7ob7oq58n/Screenshot%202022-02-27%20at%209.31.16%20PM.png?raw=1){: width="75%" }
{: refdef}

One way to avoid this, is to remove trend and seasonality from the time series using a method called Differencing, and then train using the Moving average method, and then add back the difference to the forecasted data.

But this way, the output still will be very noisy. The noise came back because we added back the 'difference' term. The past noise can also be removed by using a moving average filter on that. With this simple approach, we will observe that we aren't that far from the optimal forecasting methods. Remember this when we get to NN models for forecasting, that simple models can sometimes perform good too! 

Trailing window ($$t-30$$ to $$t-1$$) vs Centered window ($$t-5$$ to $$t+5$$), the centered window wil obviously be more accurate and smooth. But we cant use centered window on the present values since we dont know the future values. We can use centered windows to smooth the past values.

## Sequence Bias

Sequence bias is when the order of things can impact the selection of things. For example, if I were to ask you your favorite TV show, and listed "Game of Thrones", "Killing Eve", "Travellers" and "Doctor Who" in that order, you're probably more likely to select 'Game of Thrones' as you are familiar with it, and it's the first thing you see. Even if it is equal to the other TV shows. So, when training data in a dataset, we don't want the sequence to impact the training in a similar way, so it's good to shuffle them up.

## Machine Learning for Forecasting

First, as in any ML problem, we need to divide the data into features and labels. We define the features as a number of values in the series and the label as the just next value. We call the number of continuous values as the *window size*. Ex: if window size is 30 (days), we have 30 features, and the next value is the 1 label.

{:refdef: style="text-align: center;"}
![img0](https://www.dropbox.com/s/9lwa2y16nyplry2/Screenshot%202022-02-27%20at%209.50.47%20PM.png?raw=1){: width="75%" }
{: refdef}

Once we have this set of data, we can just feed this dataset to a Neural network (or any other model) and predict future data using the model.

[Source](https://www.coursera.org/learn/tensorflow-sequences-time-series-and-prediction)
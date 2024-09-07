# Forex-Analysis

## Introduction
While I have made many machine learning models (especially Computer Vision) in the past, I have never really tried using them to learn patterns in the financial market. Recently, I have been converting SGD to GBP which inspired me
to attempt to find patterns in the Forex market for the GBP (British Pounds Sterling) to SGD (Singapore Dollar) conversion rates.

In this repository, I will be using a [Long Short-Term Memory (LSTM) model](https://www.geeksforgeeks.org/deep-learning-introduction-to-long-short-term-memory/) to predict changes in the currency exchange rate between GBP and SGD. I will also explore the use of the [Relative Strength Index (RSI)](https://www.wallstreetmojo.com/relative-strength-index/)
and the [200-day Simple Moving Average (MA)](https://www.investopedia.com/terms/s/sma.asp).

## Training the LSTM model
I retrieved the exchange rate dataset from 2003 to 2024 from Yahoo Finance. 
I then cleaned the data and calculated the change in prices for the previous 10 days (until T-10, these are the X values for the LSTM model). 
I also calculated the actual 1-day actual change in price the next day (T+1, this is the Y value for the LSTM model). 
I also made sure to calculate the RSI and the MA which will be used towards the end.
After that, I performed the training loop by feeding the X values and Y values into the model and trained the model for some epochs.
The accuracies (whether the model correctly predicts if the price will increase or decrease) for the different sets of data are as shown:

Training set: 49.77%

Validation set: 49.66%

Test set: 53.64%

## Including the RSI and MA indicators
I conducted a few linear regressions below with the following accuracies, once again the 1-day actual change in price the next day is the Y value:
1. Difference to 200-day MA only: 48.66%
2. RSI only: 49.04%
3. Both indicators together: 48.66%
4. Both indicators and the output from the LSTM model: 49.04%

## Conclusion
Using a LSTM model yielded was able to predict the direction at which the price would change the next day at about a 50% accuracy. Performing a linear regression on the difference to the 200-day Simple Moving Average and the RSI indicators yielded subpar results of below 50% which were lower than using the LSTM model, which I am honestly not disappointed about, given how difficult it is to make sense of the abundance of "noise" and randomness of the market.

There are many improvements that could be made:
1. The hyperparameters of the LSTM (such as learning rate, number of epochs, batch size, number of stacked LSTMs etc.) could all be varied to find the optimal combination. I tried experimenting around with them for a while but I could have found better sets of hyperparameters.
2. Perhaps I should also have done a forward filling / fill based on surrounding mean for the non-trading days (such as the weekends) to get a continuous time series of 1 day separation.
3. Ideally, I can consider the high/low prices and perhaps assume that we buy at the mean of high and low, rather than buying at the adjusted closing price.
4. I can consider looking at more past days as well.

Future Work:
1. Rather than looking at RSI and MA indicators, perhaps I can try out other notable indicators such as [MACD](https://www.investopedia.com/terms/m/macd.asp) (Moving Average Covergence Divergence) or [Bollinger Band](https://www.investopedia.com/terms/b/bollingerbands.asp).
2. Try predicting further into the future and try to take the volume/low/high data into consideration as well. (especially if we want longer-term predictions)
3. I can also consider looking at pair trading, which could give better signals than the other indicators.
4. Lastly, I can also try a different model architecture like a transformer model or simply a binary classification if we only need to direction at which the price will move (up or down).

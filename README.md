# Volatility-Of-US-Bond-Yields
Modeling the volatility of US bond yields. 

In this project, I built a model to study the nature of volatility in the case of US government bond yields.

In the output table in the variable yc_all_tail , we see the yields for some maturities.

These data include the whole yield curve. The yield of a bond is the price of the money lent. The higher the yield, the more money you receive on your investment. The yield curve has many maturities; in this case, it ranges from 1 year to 30 years. Different maturities have different yields, but yields of neighboring maturities are relatively close to each other and also move together.

As we visualize the yields over time. We will see that the long yields (e.g. SVENY30) tend to be more stable in the long term, while the short yields (e.g. SVENY01) vary a lot. These movements are related to the monetary policy of the FED and economic cycles.
![image](https://user-images.githubusercontent.com/74027890/110206269-ab352a80-7e4a-11eb-90a2-746684813bb0.png)


From the plot of variable yc_all, we see the level of bond yields for some maturities, but to understand how volatility evolves we have to examine the changes in the time series. Currently, we have yield levels; we need to calculate the changes in the yield levels using differentiation to make the time series independent of time.

Now that we have a time series of the changes in US government yields let's examine it visually.
By taking a look at the time series from the previous plots, we see hints that the returns following each other have some unique properties:

  - The direction (positive or negative) of a return is mostly independent of the previous day's return. In other words, you don't know if the next day's return will be positive or negative just by looking at the time series.
  - The magnitude of the return is similar to the previous day's return. That means, if markets are calm today, we expect the same tomorrow. However, in a volatile market (crisis), you should expect a similarly turbulent tomorrow.
 ![image](https://user-images.githubusercontent.com/74027890/110206330-fcddb500-7e4a-11eb-8d26-da818501a0e1.png)

The statistical properties visualized can also be measured by analytical tools. The simplest method is to test for autocorrelation. Autocorrelation measures how a datapoint's past determines the future of a time series.

  - If the autocorrelation is close to 1, the next day's value will be very close to today's value.
  - If the autocorrelation is close to 0, the next day's value will be unaffected by today's value.
Because we are interested in the recent evolution of bond yields, we will filter the time series for data from 2000 onward.
![image](https://user-images.githubusercontent.com/74027890/110206387-4d551280-7e4b-11eb-80a8-58d58c5e2429.png)

A Generalized AutoRegressive Conditional Heteroskedasticity (GARCH) model is the most well known econometric tool to handle changing volatility in financial time series data. It assumes a hidden volatility variable that has a long-run average it tries to return to while the short-run behavior is affected by the past returns.

The most popular form of the GARCH model assumes that the volatility follows this process:

σ2t = ω + α ⋅ ε2t-1 + β ⋅ σ2t-1

where σ is the current volatility, σt-1 the last day's volatility and εt-1 is the last day's return. The estimated parameters are ω, α, and β.

Using the GARCH Model we get the following results for the 1 year and 20 year maturiities respectviely, calulating both volatility and the residuals of the fitted model.
![image](https://user-images.githubusercontent.com/74027890/110206467-d1a79580-7e4b-11eb-9a65-ff322dc85b08.png)
![image](https://user-images.githubusercontent.com/74027890/110206486-eb48dd00-7e4b-11eb-8aee-113f17ee09eb.png)

As we can see in the plot from the 1 year maturity, the bond yields of various maturities show similar but slightly different characteristics. These different characteristics can be the result of multiple factors such as the monetary policy of the FED or the fact that the investors might be different.

From the plots of the 1 year and 20 year, we can see that the 1-year GARCH model shows a similar but more erratic behavior compared to the 20-year GARCH model. Not only does the 1-year model have greater volatility, but the volatility of its volatility is larger than the 20-year model. That brings us to two statistical facts of financial markets not mentioned yet.

    - The unconditional (before GARCH) distribution of the yield differences has heavier tails than the normal distribution.
    - The distribution of the yield differences adjusted by the GARCH model has lighter tails than the unconditional distribution, but they are still heavier than the normal distribution.
Let's find out what the fitted GARCH model did with the distribution by plotting the density before the GARCH was applied, after the GARCH was applied and a normal distribution. 
![image](https://user-images.githubusercontent.com/74027890/110206571-8a6dd480-7e4c-11eb-9b04-f4c699a40f0f.png)

In the previous plot, we see that the two distributions from the GARCH models are different from the normal distribution of the data, but the tails, where the differences are the most profound, are hard to see. Using a Q-Q plot will help us focus in on the tails.
![image](https://user-images.githubusercontent.com/74027890/110206595-aa04fd00-7e4c-11eb-8215-22524447989a.png)

By using a GARCH model to better understand how bond volatility evolves and the affect on probability distribution we were able to apply the fitted model to analyze how volatility changed over time and bring the residuals closer to normal distribution. Also by comparing the 1 year vs the 20 year we were able to determine that the 1 year yield deviates more from a normally distributed white noise process.


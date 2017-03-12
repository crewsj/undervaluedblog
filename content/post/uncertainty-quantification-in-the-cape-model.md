+++
draft = false
date = "2017-03-04T12:34:30-05:00"
title = "Uncertainty Quantification in the CAPE Model"
tags =["finance", "uncertainty quantification"]
+++

I have been looking for a reason to play around with [emcee](http://dan.iel.fm/emcee/current/) and decided to use it to model the uncertainty in one of [John Hussman's](https://www.hussmanfunds.com/weeklyMarketComment.html) models. Emcee is one of two main Markov Chain Monte Carlo (MCMC) python libraries[^pymc]. I have no intention of giving a detailed description on Bayesian inference, but MCMC is one popular Bayesian method to quantify model uncertainty[^mcmcbooks].

John Hussman is the type of mutual fund manager that I would want to be if I were a mutual fund manager. His [weekly commentaries](https://www.hussmanfunds.com/weeklyMarketComment.html) are worth reading regulary in my opinion. Full disclaimer: I do not have money in any of Hussman's funds but that is a post for another day. Hussman has presented numerous models[^hussmanmodels] that he uses to predict forward 10-year annualized returns. Here I will use the [Shorthand Shiller Model](https://www.hussmanfunds.com/wmc/wmc130218.htm). The Shorthand Shiller Model predicts the forward 10-year return $p$ of the S&P500 using

$$p=(1+g)\left(\frac{\overline{C}}{C}\right)^{\frac{1}{10}} - 1 + d$$

where $g$ is the growth in operating profits, $\overline{C}$ is the historical average for the [Cyclically Adjusted Price/Earnings](http://www.econ.yale.edu/~shiller/data.htm) ratio, $C$ is the current CAPE ratio, and $d$ is the current dividend rate. Hussman uses $g=6.3%$, the historical growth rate in S&P500 operating earnings, and $\overline{C} = 15$, the historical average for the CAPE. Below we can see actual realized 10-year returns versus the model predicted returns[^modelcomp].

![annualized return return](/images/hussman/shiller_cape_vs_actual.png "10-year annualized return vs predicted")

The model generally follows the same trend and intuitively makes sense: long-term returns will be governed by growth in profits and valuation mean reversion. Clearly stock market returns are driven by more than that (especially in the short-term) but from a conceptual standpoint, I do not have any issues with the model. I mainly want to estimate the current predicted return using emcee. The model predicts a 10-year annualized return of 1.93% using a 2016 year-end CAPE of 27.93 and dividend rate of 2.03%. This is an absolutely abysmal annualized return for stocks. However, I have two issues with making investment decisions based on this single prediction[^pred]:

1. We have no idea if the credible interval for this predicted return is large or small. It would be one thing if 99% of the time the return was between 0-2%. It is quite another if the range of expected returns is 0-20%.
2. Clearly the model has under-predicted returns for nearly 30 years.

MCMC helps us address the first issue by quantifying the uncertainty in the model. For the second point, I imagine the model has under-predicted returns due to the 30 year bull-market in bonds and drop in interest rates. Below I plot the model error with the yield on the 10 year U.S. bond.

![model error and yield rates](/images/hussman/model_error_w_interest_rate.png "Model error and interest rates")

Maybe in another post I will include interest rates to improve the predictive power of the model, but for now, I just want to use emcee to quantify the model uncertainty. All code is available [here](https://github.com/crewsj/data_analysis/blob/master/examples/stock_returns/future_returns.ipynb). 

Instead of using fixed estimates for $g$ and $\overline{C}$, I estimate their marginal probability density functions (PDFs) using emcee[^var], shown here using histograms of the emcee output:

![growth rate PDF](/images/hussman/g_pdf.png "PDF for the growth rate")
![mean CAPE PDF](/images/hussman/cape_pdf.png "PDF for the CAPE")

The histograms show just how much uncertainty is in the model parameters. The corner plot also shows the relationship between the two parameters in the joint probability density function. 

![corner plot](/images/hussman/corner.png "corner plot")

It doesn't require genius or MCMC to notice the dependency in a model that is proportonal to $g \times \overline{C}^{\frac{1}{10}}$.

Using the estimates for the PDFs we can visualize the model uncertainty with the 50% and 95% credible intervals:

![model output](/images/hussman/CI.png "model output and credible intervals")

Finally, we can visualize the histogram for current predicted 10-year annualized returns.

![histogram of current model predictions](/images/hussman/hist_of_current_pred.png "histogram of current model predictions")

This gives a better idea of expected 10-year annualized returns given the current (elevated) CAPE. The 50% credible interval is (-1.98%, 7.04%) -- somewhere between abysmal and ok. The 95% credible interval is (-10.66%, 17.02%). This means that you have a 2.5% chance of losing more than 67% of your investment over the next 10 years (the total return for a -10.66% ten-year annualized return). The median return is 2.42%, slightly higher than the estimation using Hussman's parameters.

So what does this show besides the fact that [pandas](http://pandas.pydata.org/) and emcee are awesome? Not much really. As an investor... well I don't care much for index funds anyway so I don't get too hung up on overall market predictions. The only thing this shows is how uncertain markets are and the dangers of investing at elevated valuations -- neither insights require Bayesian inference. Investing is not about a single option though; investing is about evaluating opportunities and tradeoffs in terms of risk and return. Other liquid options (namely cash and bonds) currently offer low returns as well. The one key takeway from Hussman's model and its current prediction for poor 10-year returns is to save more. If you are saving for a young child's college, plan for 2% returns for 10 years and then 6-7% after that. I wouldn't hinge my saving plans on earning 6-7% equity returns for 20 years.

[^pymc]: The other is [pymc](http://pymc-devs.github.io/pymc/). 

[^mcmcbooks]: For a detailed description, simply bing it. I have also started reading [Bayesian Methods for Hackers](http://amzn.to/2lqsQgx) but I haven't read enough to recommend it yet. I can recommend my former post-doc advisor's book on [uncertainty quantification](http://amzn.to/2lITk8m).

[^hussmanmodels]: See [here](https://www.hussmanfunds.com/wmc/wmc130218.htm) and [here](https://www.hussmanfunds.com/wmc/wmc140414.htm).

[^modelcomp]: If you compare closely my output to [Hussman's](https://www.hussmanfunds.com/wmc/wmc130318a.gif) you will notice the actual returns that I have plotted seem to be off by about 1% on the high end and the low end. I use [Shiller's original data](http://www.econ.yale.edu/~shiller/data.htm) but do not include the reinvestment of dividends (which I should have) since the goal here was mainly to play around with emcee.

[^pred]: Besides the fact that I have issues with making investment decisions based on any *single* prediction.

[^var]: Note that I did not sample the variance, which I should have.


<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
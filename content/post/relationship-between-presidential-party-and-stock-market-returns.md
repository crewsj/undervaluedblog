+++
draft = false
date = "2017-02-19T13:52:56-05:00"
title = "Stock Market Returns Based on Presidential Party"
tags =["finance"]
+++

Since the election, a few people have made comments to me implying *Trump will be good for stocks*[^trump]. The implication is that Republican presidents are better for the stock market since they are generally viewed as being pro-business. It amazes me that this assumption is pervasive given stock market performance for the last three presidents (Clinton, Bush, Obama). However, since I recently read [The Undoing Project](/2017/02/the-undoing-project), I was cognizant of any biases I may have and decided to look at the available data.

Before presenting the data, I should confess that I view this analysis as futile. I do not believe that one should make investing decisions solely based on the president (and [others](https://www.nytimes.com/2017/02/13/upshot/its-probably-a-bad-idea-to-sell-stocks-because-you-fear-trump.html) agree). Nor do I believe that the president (or Congress) has much influence over the stock market[^fed]. Many factors are out of the president's control. For example, President George W. Bush inherited a stock market bubble and left office in a deep bear market and recession[^bush].

For stock market returns, I used the adjusted close of the S&P500. [Yahoo](http://finance.yahoo.com/quote/%5EGSPC?p=^GSPC) provides data through the Truman administration. The list of presidents and corresponding inauguration dates is available on [Wikipedia](https://en.wikipedia.org/wiki/List_of_Presidents_of_the_United_States). The data and analysis are available as a Jupyter notebook[^jupyter] on [github](https://github.com/crewsj/data_analysis/blob/master/examples/stock_returns/presidential_returns.ipynb)[^mistakes].

Without further ado, the total and annualized return when each party controlled the presidency:

| Presidential Party | Total Return (%) | Annual Return (%)|
|--------------------|:----------------:|:----------------:|
| Republican         | 354.79           | 4.30             |
| Democrat           | 2896.92          | 10.01            |

If you invested only when the Democrats controlled the executive branch, you would have a nearly 30x return on your money. If you invested only when the Republicans controlled the executive branch, you would have approximately a 4.5x return on your money. Democrats turned $1 into $30; Republicans turned $1 into $4.5.

The difference shocked me but if we look at the returns for each president we can see why. The results per president in chronological order are below.

![total return](/images/total_return_chrono.png "total return")

![annual return](/images/annual_return_chrono.png "annual return")

Ranking the results show just how much Clinton and Obama dominated[^dominated]. All the other Democratic presidents would have given you approximately at 3x return on your money, while Obama and Clinton added nearly another 10x on top of that.

![annual return ranked](/images/annual_return_ranked.png "annual return ranked")

Losing 40% during President George W. Bush's adminstration compared to 182% and 209% returns for Obama and Clinton, respectively, really hurt the Republicans[^rule]. If we take out the last three presidents[^stupid], the results are more even with a 6.48% annualized return for the Democrats and a 7.51% annualized return for the Republicans.

The other interesting thing that stood out to me was George H. W. Bush's[^hwbush] position in the rankings. He is the second-best Republican (annualized return) and hardly deserves being remembered for losing to ["It's the economy, stupid"](https://en.wikipedia.org/wiki/It%27s_the_economy,_stupid). 

Perhaps I will look at other statistics such as unemployment rate, manufacturing indices, etc. I would also like to include the party controlling Congress in the results. None of these results should be used to make investment decisions. More to come...


[^trump]: The [most relevant data point](http://www.marketwatch.com/story/donald-trump-was-a-stock-market-disaster-2015-07-22) always seems to be dismissed during these conversations. Fake news I guess.

[^fed]: The Federal Reserve is a different matter...

[^bush]: I do not blame President Bush (or Republicans) for the Great Recession, financial crisis, whatever you want to call it. Many people would disagree with me, but Glass-Steagall was [repealed](https://en.wikipedia.org/wiki/Gramm%E2%80%93Leach%E2%80%93Bliley_Act) on Clinton's watch. I don't blame President Clinton either.

[^jupyter]: Not sure how I feel about [Jupyter](http://jupyter.org/). Perhaps I'll have more on that later. For simple analyses that you want to share, it works pretty well though.

[^mistakes]: Email mistakes to undervaluedblog [at] gmail.com

[^dominated]: Not sure if "dominated" is the right word.

[^stupid]: And without Lebron the Cavs wouldn't have won. I realize this makes a futile analysis even dumber.

[^rule]: Buffett's rule number 1: "Never lose money." Rule number 2: "See rule number 1."

[^hwbush]: Big fan.


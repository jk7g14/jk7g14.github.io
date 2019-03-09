---
layout: post
title:  "Deep Trading Agent, Bitcoin Futures Trading using Reinforcement Learning"
subtitle: "sdfsdf"
comments: true
---

Reinforcement Learning could be used in game areas, control system management, and many other areas. Trading assets can be considered as a game that Reninforcement Learning can be applied.

## Problem Statement
Very few traders are coming into stock markets and earning profits. I am not talking about investors in group companies. We are talking about just ordinary traders. Sometimes, we call them an Ant. Many people said that to be a successful trader, you need to learn some strategies or patterns including the control of greed. To get rid of greed is the most difficult part here for humans. So, there are many algorithmic trading bots on the internet, but even when you choose trading bots, you need to know which strategies or patterns you are going to use. The reasons that I chose bitcoin trading are that cryptocurrency trading is 24-hours-operation, APIs are easy to use and it is acceptable for futures trading with small amount of money.

## A brief explanation of Deep Trading Agent
In this project, I am building and testing a bitcoin futures trading using [Deepmind's DQN(Deep-Q-Learning)](https://github.com/jk7g14/ethermembership). There will be two networks at the same time during trading because I want our agents to be experts on both short and long positions. Furthemore, in terms of trading rules and environment, we are going to use XBTUSD in [BitMex](https://www.bitmex.com). For the dataset, one of the crtyptocurreny exchanges called [Bitstamp](https://www.bitstamp.net) historical price from 2014 to 2018 is used. It contains open, low, high, close prices and volumes.

## Deep Q Network
For the network achitecture, I used [samre12/deep-trading-agent](https://github.com/samre12/deep-trading-agent)'s architecture named Deep Sense which consists of CNN, RNN(with GRU)and Q Function Estimator with fully connected layers.

There are three actions in this network including Up, Down, Neutral which are (buy/long), (sell/short), (hold/do nothing) respectively.

![Sample Images](/img/deep-trading-agent/arch.png){:width="600"}

# Preprocessing

Bitcoin prices have to be normalized because it has no limit and no range of it. The number of features of each time step, there were only 5 features including open, high, low, close prices and volumes, but we added some basic indicators like all the traders do. As a result, we got 12 features in each frame.

* Candle (Open, High, Low, Close)
* Volume
* RSI
* Bollinger Bands(upper, middle, lower)
* Simple Moving Average(SMA5, SMA20, SMA60)

In this project, I used 1 hour candle because this was the minimum candle which my agent could get profit. Every 1 hour, past 180 hours of data was passed to the network to get the action.

## Reward Function
```
Unrealized PnL = ($1/Average_Entry_Price - $1/Current Price) * Contract Quantity
```

For the reward, unrealized PnL(Profit & Loss) is used in both short and long positions. For example, if we are in short position, Contract Quantity is minus, so this equation works in both positions. Furthemore, each price has to divide $1 because basic currency in BitMex is BTC, the amount of bitcoin (we call it [Satoshi](https://en.bitcoin.it/wiki/Satoshi_(unit)))
## Agent

Final form of our agent is like this. Because market may go up trend or down trand, if we are in one position, we cannot take a profit all the time. 

![Sample Images](/img/deep-trading-agent/diagram.png){:width="600"}

## Tricks

- **Storage**

Even if agent takes a lot of profits, the limitation of bet size is required to protect the money. For example, you earned 3.0 btc from 1.25 btc and then total btc is 4.25, but you are not going to bet 4.25 btc next time. You are going to bet 1.25 btc again and keep the 3.0 btc. I found this really profitable since we are in both short and long positions.

- **Stop Loss**

Because this is Futures Exchange, there is liquidation price which makes you bankrupt. Therefore, stop loss is required. In this project, stop loss is operated when profit is -3% when the laverage are x5(short) and x4(long).


## Results

In training set, I used all the data I have and from 2014 to 2018, long position agent increased the amount of bitcoin from 2.5 to 5.01 and shor position agent increased the amount of bitcoin from 2.5 to 7.79. To sum up, the amount of bitcoin was increased from 5 to 12.8. This seems just very small profit, but when it comes to dollars, it started from $4000 and ended up to about $81920 dollars.

(Green is buy/long and Red is sell/short)

* **Long Position**

![Sample Images](/img/deep-trading-agent/bitmex_long.png){:width="600"}

* **Short Position**

![Sample Images](/img/deep-trading-agent/bitmex_short.png){:width="600"}

* **Long Position from 2014-2018**

![Sample Images](/img/deep-trading-agent/bitmex_long-2014-01-01-2014-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_long-2015-01-01-2015-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_long-2016-01-01-2016-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_long-2017-01-01-2017-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_long-2018-01-01-2018-12-31.png){:width="600"}

* **Short Position from 2014-2018**

![Sample Images](/img/deep-trading-agent/bitmex_short-2014-01-01-2014-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_short-2015-01-01-2015-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_short-2016-01-01-2016-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_short-2017-01-01-2017-12-31.png){:width="600"}
![Sample Images](/img/deep-trading-agent/bitmex_short-2018-01-01-2018-12-31.png){:width="600"}

## Code

This is a prototype of the project([deep-trading-bot](https://github.com/jk7g14/ethermembership)).

This does not support BitMex environment, but supports normal crpyto trading exchanges.
<br />

---

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
  /*
  var disqus_config = function () {
  this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
  this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
  };
  */
  (function() { // DON'T EDIT BELOW THIS LINE
  var d = document, s = d.createElement('script');
  s.src = 'https://jk7g14-github-io.disqus.com/embed.js';
  s.setAttribute('data-timestamp', +new Date());
  (d.head || d.body).appendChild(s);
  })();
  </script>
  <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  {% endif %}

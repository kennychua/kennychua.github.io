---
layout: post
title: "Experiments : Creating a Crypto Twitter Bot to Announce Newly Listed CryptoCoins"
date: 2018-11-17 22:22:57
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
share: linkedin twitter
---

*Welcome to a series of posts where I document my thoughts and experiments as an avid tinkerer, learner, and explorer in Technology*	

# Experiments : Creating a Crypto Twitter Bot to Announce Newly Listed CryptoCoins

In crypto trading, there are big movements in a cryptocurrency's price in a short period after it is added to an exchange.

The positive effect of being listed on a popular exchange has been quite substantial for altcoins and newly-issued ICO tokens as it not only provides the digital asset with a certain level of industry approval but it also allows a much larger investor base to invest in it. 

Aside from being listed on international cryptocurrency exchanges, a listing on a popular regional exchange can also have a profound impact as it allows local investors to invest and withdraw profits using their own local fiat currency.


![LINK bithumb price jump](https://raw.githubusercontent.com/kennychua/kennychua.github.io/master/assets/img/exchange-listing-price-movement.jpg)

The screenshot above illustrates the price jump in the LISK coin when it was announced that it had been listed on the South Korean Bithumb Exchange.

## Experiment time!
***What if... *** I made a bot that polls the top cryptocurrency exchanges for new coins and posts on Twitter whenever it detects a new cryptocurrency being listed on an exchange?

As it turns out, the idea was very achievable! The bot has found and posted about a **whopping 1000+ new coins**, and has an audience of **800+ followers** so far!

**In fact, I noticed that most exchanges add new cryptocurrencies to their APIs ahead of any official announcement, so this is a huge advantage!**

![](https://raw.githubusercontent.com/kennychua/kennychua.github.io/master/assets/img/NewlyListedCoinBot-twitter-profile.jpg)

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Found newly added coin USDC on binance</p>&mdash; NewlyListedCoinBot (@NewlyListedCoin) <a href="https://twitter.com/NewlyListedCoin/status/1063627789063151616?ref_src=twsrc%5Etfw">November 17, 2018</a></blockquote>

See more tweets and follow at [this link!](https://twitter.com/NewlyListedCoin?lang=en)


***Otherwise, read on to find out more about how I made the bot!*** 

----

## Challenges :

### Challenge #1 : Getting a consistent API to as many exchanges as possible
Though most exchanges offer a public API that can return information on what coins are supported on their exchange, there is no standard form so it would have been too painstakingly slow to write custom logic for each exchange to parse the each exchange's response individually.

Luckily, I came across the excellent [ccxt library](https://github.com/ccxt/ccxt) in previous experiments that offers a simple consistent API for the top-100 exchanges in nodeJs, Python and PHP. *Perfect!*

### Challenge #2 : Detecting when new cryptocurrencies are *added* to an exchange
Using the ccxt library described above, it returns all of an exchange's supported coins, but it doesn't tell us **when** a new cryptocurrency is ***added***.

So, the strategy here is to repeatedly query an exchange every few minutes for supported cryptocurrencies. Then, we compare the list of currently supported cryptocurrencies with a list we stored from the previous execution.

When we find a difference between the old list and the current list, the newly added items in the list are the newly supported cryptocurrencies!

### Challenge #3 : Running a node process on a schedule, forever
Thanks to `npm`, there are easy solutions for these that do exactly what it says on the box!

To run a task on a schedule, [node-schedule](https://github.com/node-schedule/node-schedule) comes to the rescue and provides a cron-like syntax to run tasks on a schedule

As for making sure a script keeps running, there is [foreverjs](https://github.com/foreverjs/forever) to make sure a script keeps running and gets revived if it crashes or gets killed

----

## Solution goals

The goal here was to keep things as simple as possible.
### Minimise callback hell (aka callback pyramids) and promise chaining
In order to keep this as simple as possible, I was keen to leverage the ES2017 async/await pattern to replace the ugly callback pyramids and promise chaining when dealing with lots of asynchronous programming.

![The dreaded callback pyramid](https://raw.githubusercontent.com/kennychua/kennychua.github.io/master/assets/img/callback-hell.jpg "Gross!")

### Try a Virtual Private Server
I've tried cloud services from Google and AWS, but haven't actually tried a VPS (Virtual Private Server) from one of the smaller providers. This was an opportunity to try a little server for $5 a month from [Vultr](https://www.vultr.com/)

---

## Source code
```
const schedule = require("node-schedule");
const ccxt = require("ccxt");
const jsonfile = require("jsonfile");
const _ = require("underscore");
const Twit = require("twit");

const file = "./previouslyFoundCoins.json";

const T = new Twit({
  consumer_key: "XXX",
  consumer_secret: "XXX",
  access_token: "XXX",
  access_token_secret: "XXX",
  timeout_ms: 60 * 1000 // optional HTTP request timeout to apply to all requests.
});

let exchanges = [];

async function main() {
  // List the exchanges that we want to scan for new coins
  exchanges = [
    new ccxt.binance(),
    new ccxt.btcmarkets(),
    new ccxt.kucoin(),
    new ccxt.bittrex(),
    new ccxt.bithumb(),
    new ccxt.bitfinex2(),
    new ccxt.okex(),
    new ccxt.gateio(),
    new ccxt.huobipro(),
    new ccxt.hitbtc2(),
    new ccxt.gdax(),
    new ccxt.poloniex(),
    new ccxt.bitflyer(),
    new ccxt.bitmex(),
    new ccxt.bitstamp(),
    new ccxt.gemini()
  ];

  // Read our file containing previously found coins
  jsonfile.readFile(file, async function(err, previouslySavedData) {
    // Query an exchange for supported coins
    for (let exchange of exchanges) {
      await exchange.loadMarkets();
      var currencies = await exchange.currencies;

      // Compare with previous list of supported coins
      var differences = _.difference(
        Object.keys(currencies),
        previouslySavedData[exchange.id]
      );

      // Notify if we found a difference between current list and previous list
      // A newly added coin was found
      if (differences.length > 0) {
        var newPairMessage = `Found newly added coin ${differences.join(
          ", "
        )} on ${exchange.id}`;
        console.log(new Date().toString() + " " + newPairMessage);

        T.post("statuses/update", { status: newPairMessage }, function(
          err,
          data,
          response
        ) {
          console.log(data);
        });

        // Save all currencies including newly found ones to memory
        previouslySavedData[exchange.id] = Object.keys(currencies);
      } else {
        console.log(
          new Date().toString() + "No new coins found on " + exchange.id
        );
      }
    }

    // Save new list of supported coins back to file
    jsonfile.writeFile(file, previouslySavedData, function(err) {
      console.error(err);
    });
  });
}

// Run this loop every 3 minutes
schedule.scheduleJob(`*/3 * * * *`, function() {
  main();
});

```

----


## Next explorations/Things to do differently next time

- Vultr was nice as a VPS, but I would move this job to an AWS Lambda function using the [Serverless Framework](https://serverless.com/) just so all my side projects are located in the same place. 
	- Given the asynchronous of this project, the cold boot latency of a AWS Lambda function is acceptable.
	- Since this solution doesn't require high-speed high-frequency executions, the smallest possible scheduled interval of 1-minute in AWS Lambda is also acceptable.

- Now that I am aware the moment a new coin is added to an exchange, the next step is to hook this up to my automated cryptocurrency trader to realise some from this early knowledge! More on this next time...!






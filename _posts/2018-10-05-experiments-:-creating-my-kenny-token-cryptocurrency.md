---
layout: post
title: "Experiments: Creating my Kenny Token Cryptocurrency"
date: 2018-10-05 00:15:09
image: '/assets/img/'
description:
tags:
categories:
twitter_text:
share: linkedin twitter
---

*Welcome to a series of posts where I document my thoughts and experiments as an avid tinkerer, learner, and explorer in Technology*

## Experiments : Creating my own Kenny Token Cryptocurrency

In the flurry of ICOs and the hype of cryprocurrencies, I set out to find out how simple it would be to create my own cryptocurrency.

As it turns out, it's not difficult at all to create an ERC20 based token on the Ethereum network!

### TL;DR - I managed to create KennyToken in under an hour!
![kennytoken screenshot](https://raw.githubusercontent.com/kennychua/kennychua.github.io/master/assets/img/kennytoken.jpg)

Out of thin air, I created 100,000,000 KENNY tokens! You can find the creation of this on the Ethereum Ropsten Testnet Blockchain at https://ropsten.etherscan.io/token/0x9a170ac4b1458fe121498bf10693193b89d11d57

The source for Kenny Token is also available at 
<script src="https://gist.github.com/kennychua/7dec76ba70aba3dad38c1e8925dfb1b2.js"></script>


### Prior knowledge before exploration

#### Solidity programming language
Much of the heavy lifting in creating an ERC20 cryptocurrency has been done and is available as public references, eg
- https://solidity.readthedocs.io/en/v0.4.24/solidity-by-example.html
- https://github.com/bkrem/awesome-solidity
- https://www.ethereum.org/token

Learning Solidity was made easy and fun with CryptoZombies - a game where you progress thru the levels by learning and completing Solidity code
- https://cryptozombies.io/

### Next explorations
Having explored a bit of Solidity as a programming language, the next steps are to explore creating smart contracts with a framework. 

Frameworks that ease development by including built-in support for automated testing for smart contracts, support for deployment management to testnet and mainnet, and frameworks that cover both the backend smart contract as well as the frontend via `Web3.js`

A shortlist includes
- [Truffle Framework](https://truffleframework.com/docs/truffle/overview)
- [Embark](https://github.com/embark-framework/embark)

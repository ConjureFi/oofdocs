---
description: Scry Open Oracle Framework
---

# Introduction

Scry is an open source, permissionless, flexible and robust framework for Oracle deployment with multifeed support, subscriptions and instant deployment. We have developed the Open Oracle Framework for high scale and flexible oracles.

**The Uniswap of Data**

In the world of blockchain and decentralized systems, data is a critical component that is needed to execute smart contracts, enable dApps, and facilitate transactions. However, the problem with the current approach to data is that it is centralized even while most pretend to be decentralized, which goes against the very ethos of decentralization. This is where Data LPs and free market approaches come in.

One of the biggest issues with data and oracle infrastructure in crypto is that it is monopolized by players like Chainlink, and all alternatives suffer from the same closed systems for data through set signers or a single token as the core security. This results in a centralized approach to data, which goes against the very ethos of decentralization.

Data LPs (Data Liquidity Providers) are essentially decentralized marketplaces for data. They provide a way for developers to source data from a variety of different sources, while also providing incentives for individuals and entities to contribute their data to the marketplace. In a Data LP, anyone can contribute their data and earn rewards for doing so. Developers can then purchase the data they need from the signers they choose. Scry is a pioneering a decentralized and open market data approach with Morpheus that allows anyone to easily set up and operate their own oracle and node with no technical expertise.

By enabling anyone to fill requests for data, Scry allows developers and projects to choose where they source their data based on reputation for who they feel will be honest, as well as even source the data from their own community.

Oracles are able to be self-deployed by developers with their own signers securing the feeds with own nodes, allowing fully decentralized deployment and data, while allowing projects to deploy in realtime as they need feeds by simply updating their source sheet.

**Flexibilty**\
By allowing people to easily bootstrap feeds using multiple members, users can create decentralized oracle feeds for whatever they want, this is especially important for L2s where gas fees are low and so large signer sets and feeds can be created for realtime price sources. A simple node that queries many apis, submits them and then returns the med value on request allows for rapid and scalable deployment for Synthetic Assets, as anyone can just set up a node or set the feed to be sourced from a gnosis, letting the logic be based on an api or general agreement.

**1 Contract Many Feeds**\
Oracles can have a single feed or have 1 contract and submit many feeds/prices available, allowing submitFeed(uint\[] feedIDs, uint\[] values) to save gas for large feed sets. This allows large players to easily submit multiple feeds with 1 call and not need to manage multiple transactions/calls to multiple contracts.

**Monetization and Incentives**\
Because oracles have upkeep costs, the oracles can charge a small fee in ETH. This way they can deploy many feeds as long as they are worth the upkeep, and because we compact many feed updates into single calls, efficiency couldn’t be better. Users can then shop around for the best feed or can choose based on price, depending on risk tolerance. This creates an open oracle marketplace and revenue model and allows projects to quickly bootstrap feeds they need from providers. Oracles can then price their feeds based on upkeep and cost as well as deprecate unused feeds.

**Containerization**\
Oracles are created as single contracts which are owned by the oracle parties that submit feeds, these oracles then create an id for every feed they want to create, which allows for users to explore feeds offered by an oracle and select based on id, while having each oracle be completely contained, such that bad oracles do not affect good oracles and users can choose providers that are good for them.

**Scalability**

OOF was designed for projects, users and providers to easily create new feeds and oracles for Ethereum and other networks to have access to offchain data in a decentralized and open market model. By allowing for anyone to create an oracle with minimal knowledge and time, and setting up decentralized providers that are robust, flexible and secure (with enough signers), feeds can rapidly scale and with them, the services that these networks provide can scale rapidly aswell.

---
description: Scry Open Oracle Framework
---

# Introduction

Scry is an open source, permissionless, flexible and robust framework for independant Oracle deployment with high scale batching, revenue earning, crosschain data and instant deployment. We have developed the Open Oracle Framework and Morpheus for high scale and flexible oracles deployable on any EVM. We are the only protocol that allows anyone to run their own fully independant, autonomous oracle with 0 code needed and deployable in <5m.\
\
**TLDR**

* Crosschain lookup for any EVM/contract on demand
* &#x20;Only protocol that allows anyone to run theirown fully independant, autonomous oracle with 0 code needed&#x20;
* Ability to use any API with a robust parse engine.&#x20;
* &#x20;Ability to run a data provider and earn fees from requests.&#x20;
* Real-time access to any API endpoint for any data.&#x20;
* VRFs and verifier tools based on new mechanisms.&#x20;
* &#x20;AI agent support for autonomous data determination and interaction.

**The Uniswap of Data**

In the world of blockchain and decentralized systems, data is a critical component that is needed to execute smart contracts, enable dApps, and facilitate transactions. However, the problem with the current approach to data is that it is centralized even while most pretend to be decentralized, which goes against the very ethos of decentralization. This is where Data LPs and free market approaches come in.

One of the biggest issues with data and oracle infrastructure in crypto is that it is monopolized by players like Chainlink, and all alternatives suffer from the same closed systems for data through set signers or a single token as the core security. This results in a centralized approach to data, which goes against the very ethos of decentralization.

### Data LP

Data LPs (Data Liquidity Providers) are essentially decentralized marketplaces for data. They provide a way for developers to source data from a variety of different sources, while also providing incentives for individuals and entities to contribute their data to the marketplace. In a Data LP, anyone can contribute their data and earn rewards for doing so. Developers can then purchase the data they need from the signers they choose. Scry is a pioneering a decentralized and open market data approach with Morpheus that allows anyone to easily set up and operate their own oracle and node with no technical expertise.

By enabling anyone to fill requests for data, Scry allows developers and projects to choose where they source their data based on reputation for who they feel will be honest, as well as even source the data from their own community.

Oracles are able to be self-deployed by developers with their own signers securing the feeds with own nodes, allowing fully decentralized deployment and data, while allowing projects to deploy in realtime as they need feeds by simply updating their source sheet.

### TLDR Features

1. Autonomous oracle system where devs can self deploy own oracles and use custom signers for permissionless, decentralized and secure deployment with self-controllable feed creation using custom APIs for rapid development.&#x20;
2. The nodes are able to be deployed in <15m on any EVM by devs with 0 code needed and update feeds supported in realtime as needed.&#x20;
3. &#x20;Use any API with a highly robust parse engine, just put the URL, the parse for the json to be expected and basic info and the feed will be ready to use and submitted on chain by all signers.&#x20;
4. Data lookup for historical data for all feeds natively, allows for both immutable data access, TWAP construction and onchain analytics when using pull based oracle.&#x20;
5. Allows for various monetization structures at a feed level, oracle providers can charge for certain feeds to earn revenue as well as provide others for public use, subscription models for data requests at the feed level and oracle level.&#x20;
6. &#x20;Largest data provider for Goerli, Sepolia, Optimism, Canto, Polygon and providing 3x the data Link are able to, with the ability to scale to 100x by simply updating the APIs pulled from. Stress tested with up to 500 feeds with just 1 tx.&#x20;
7. Fully decentralized and open source, the Front End, Node and Contracts are all open source and can be self hosted by teams and networks&#x20;
8. Anyone can run a data provider and earn fees from requests&#x20;
9. &#x20;Creates a data market and lets devs choose their oracle sources at the signer level and batch many for redundancy&#x20;
10. Any API endpoint in realtime for any data&#x20;
11. Oracles now support string data so can be used for hex data as needed as well as so can do non int based data for social/games/whatever
12. VRFs and verifier tools based on new mechanism
13. A neat support for chatGPT with the idea being so that can use these models to autonomously determine data like if a protocol was hacked, current weather in x for insurance, if some condition based on x was met, as well as DAOs/whoever can use the AI to interact with offchain shit, ie, dao sends a tx to the oracle for chatgpt to create a contract on it's behalf dao can then sign it and have the AI agent through a plugin like send an email/do whatever

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

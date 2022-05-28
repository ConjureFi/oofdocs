# Introduction

Open Oracle Framework is an open source, permissionless, flexible and robust framework for Oracle deployment with multifeed support, subscriptions and instant deployment.

Open Oracle Framework’s a framework for permissionless oracle deployment and robust and flexible on-chain oracle feeds. Oracles are simply multi-party median value feeds, but you can even implement your own price logic through a dedicated proxy owner, which itself has it’s own price logic. The median value model takes the value for n signers, uses the median value submitted, makes sure that m signers for the m/n threshold has been met and returns the value. The oracle can require a small fee for subscriptions to feeds to cover gas fees, and are able to provide many feeds from 1 contract, allowing for highly efficient, scalable feed submission and retrieval.

**Flexibilty**\
By allowing people to easily bootstrap feeds using multiple members, users can create decentralized oracle feeds for whatever they want, this is especially important for L2s where gas fees are low and so large signer sets and feeds can be created for realtime price sources. A simple node that queries many apis, submits them and then returns the med value on request allows for rapid and scalable deployment for Synthetic Assets, as anyone can just set up a node or set the feed to be sourced from a gnosis, letting the logic be based on an api or general agreement.

**1 Contract Many Feeds**\
Oracles can have a single feed or have 1 contract and submit many feeds/prices available, allowing submitFeed(uint\[] feedIDs, uint\[] values) to save gas for large feed sets. This allows large players to easily submit multiple feeds with 1 call and not need to manage multiple transactions/calls to multiple contracts.

**Monetization and Incentives**\
Because oracles have upkeep costs, the oracles can charge a small fee in ETH for subscriptions. This way they can deploy many feeds as long as they are worth the upkeep, and because we compact many feed updates into single calls, efficiency couldn’t be better. Users can then shop around for the best feed or can choose based on price, depending on risk tolerance. This creates an open oracle marketplace and revenue model and allows projects to quickly bootstrap feeds they need from providers. Oracles can then price their feeds based on upkeep and cost as well as deprecate unused feeds.

**Containerization**\
Oracles are created as single contracts which are owned by the oracle parties that submit feeds, these oracles then create an id for every feed they want to create, which allows for users to explore feeds offered by an oracle and select based on id, while having each oracle be completely contained, such that bad oracles do not affect good oracles and users can choose providers that are good for them.

**Scalability**

OOF was designed for projects, users and providers to easily create new feeds and oracles for Ethereum and other networks to have access to offchain data in a decentralized and open market model. By allowing for anyone to create an oracle with minimal knowledge and time, and setting up decentralized providers that are robust, flexible and secure (with enough signers), feeds can rapidly scale and with them, the services that these networks provide can scale rapidly aswell.

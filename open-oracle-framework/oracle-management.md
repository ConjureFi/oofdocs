---
description: Oracle Management
---

# Oracle Management

All oracles are able to be managed by their signers, we have built in a lightweight democratic vote system, whereby a signer can propose a change to the contract, and with the threshold being met, can execute these changes. This allows for oracles to change signers/update feeds/update prices/withdraw funds and do other management

Funds that have been donated/paid to the oracle can be withdraw to either a designated address for custom withdraw features to be handled by another contract, or can be split between all signers. This allows for cold wallets to be feed providers, using a contract to send the payment to the cold wallets instead on receive/fallback.

**Proposal Types**

* 0 updatePricePass(uintValue)
* 1 updateThreshold(uintValue)
* 2 addSigners(addressValue)
* 3 removeSigner(addressValue)
* 4 updatePayoutAddress(addressValue)
* 5 updateRevenueMode(feedId, uintValue)
* 6 updateFeedCost(feedId, uintValue)


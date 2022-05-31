---
description: How to be a mercenary data provider 101
---

# Permissionless Data Requests and Data Providers

By having a support field for payments to support feeds, a helper contract can be used to create permissionless data requests to providers. This can be done by having a contract that maps to an oracle and takes an ETH payment as well as metadata for a API endpoint as well as the path as a new fn `requestFeed(endpoint, path, feedid)`The data provider then listens for deposits to the helper and then simply updates their Spreadsheet and restarts the node or updates the feeds. This allows for an unknown party to pay a provider a set ETH amount for a feed to be created and data submitted to the set feed ID, with the helper contrcact forwarding the ETH on the feeds creation. This process can be fully automated and by simply having providers specify their rates per feed per submit as well as a min if needed, an on demand data service can be established for all contracts, with a free market for the best and most reputable oracle providing the data. A Data Bizaar.

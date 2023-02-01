---
description: How to hook up Scry Oracles to your contracts
---

# Solidity Contracts and Interface

## Use Scry Feeds in Your Contracts

#### SOLIDITY

Take feedID in the fn params or hardcode with the interface to get the feed value. Set the oracleAddress to the address for the oracle with the feed you're using.

```
uint256 feedID = x;
uint256 feedValueRAW, uint256 feedLastTimeStamp, uint256 feedDecimals = IOpenOracleFramework(oracleAddress).getFeed(feedID);
uint256 feedValue = feedValueRAW / (10**18 * feedDecimals);
```

You can now use the feed value as needed.&#x20;



Note that `feedValueRAW / feedDecimals`

Should be used for the real value represented by the front end.

## **Scry Feed Lookup**

[https://github.com/ScryProtocol/feedLookup](https://github.com/ScryProtocol/feedLookup)

This repo allows you to use a simple script to check oracle data, specifically for the reference oracles but for any oracle address using the RPC set for the network.

### Use

Simply

`git clone https://github.com/ScryProtocol/feedLookup`

into the repo,

`npm install`

and then run `node feeds.js`

All feeds and associated IDs and info will show. To use the feeds in your contracts simply use the interface in your contract or in a handler contract

### Interface

```
 * @dev Interface of the OpenOracleFramework contract
 */
interface IOpenOracleFramework {
    /**
    * @dev getHistoricalFeeds function lets the caller receive historical values for a given timestamp
    *
    * @param feedIDs the array of feedIds
    * @param timestamps the array of timestamps
    */
    function getHistoricalFeeds(uint256[] memory feedIDs, uint256[] memory timestamps) external view returns (uint256[] memory);

    /**
    * @dev getFeeds function lets anyone call the oracle to receive data (maybe pay an optional fee)
    *
    * @param feedIDs the array of feedIds
    */
    function getFeeds(uint256[] memory feedIDs) external view returns (uint256[] memory, uint256[] memory, uint256[] memory);

    /**
    * @dev getFeed function lets anyone call the oracle to receive data (maybe pay an optional fee)
    *
    * @param feedID the array of feedId
    */
    function getFeed(uint256 feedID) external view returns (uint256, uint256, uint256);

    /**
    * @dev getFeedList function returns the metadata of a feed
    *
    * @param feedIDs the array of feedId
    */
    function getFeedList(uint256[] memory feedIDs) external view returns(string[] memory, uint256[] memory, uint256[] memory, uint256[] memory, uint256[] memory);
}
```

###

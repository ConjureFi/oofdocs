---
description: How to hook up Scry Oracles to your contracts
---

# Solidity Contracts and Interface

## Scry Feed Lookup

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

```solidity
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

### Use

#### SOLIDITY

Take feedID in the fn params or hardcode with the interface to get the feed value.

<pre class="language-solidity"><code class="lang-solidity">uint256 feedID = x;
<strong>uint256 feedValue, uint256 feedLastTimeStamp, uint256 feedDecimals = IOpenOracleFramework(0x00f0feed50dcdf57b4f1b532e8f5e7f291e0c84b).getFeed(feedID);
</strong>uint256 feedValue =feedValue/10**feedDecimals;

;

</code></pre>

You can now use the feed value as needed.

Note that `feedValue / 10**feedDecimals`

Should be used for the real value represented by the front end.

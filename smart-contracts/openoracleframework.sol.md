# OpenOracleFramework.sol

### OOF Init

### `initialize`

```
    function initialize(
        address[] memory signers_,
        uint256 signerThreshold_,
        address payable payoutAddress_,
        uint256 subscriptionPassPrice_,
        address factoryContract_
    ) external
```

Initializes the OOF contract. This function takes the signer addresses, the signer threshold as well as the address used for payouts and the optional price of a subscription pass (0 for disabled).

After this function is executed, the Open Oracle Framework contract is ready to be used.

### Helper Functions

### `sort`

```
function sort(uint[] memory data) private pure returns (uint[] memory) 
```

Sorts incoming data, used for calculating median prices.

### View Functions

### `getHistoricalFeed`

```
function getHistoricalFeeds(uint256[] memory feedIDs, uint256[] memory timestamps) external view returns (uint256[] memory) {
```

Function to look up historical prices by accepting an array of feedIDs and an array of timestamps.

If a feed is restricted, there will be a check if the address calling this function has the rights to see historic prices for that feed.

Returns the historic price for each feed requested during the timeslot for the time stamp from the storage of the contract.

### `getFeeds`

```
function getFeeds(uint256[] memory feedIDs) external view returns (uint256[] memory, uint256[] memory, uint256[] memory)
```

�Function which takes an array of feed IDs as an input. Returns the price, timestamp of the last price update, and the feed decimals as a return value.

Calls getFeed internally.

### `getFeed`

```
function getFeed(uint256 feedID) public view returns (uint256, uint256, uint256)
```

�This function returns the last price, timestamp of last update and feed decimals as the return value.

Also if the feed is a paid one, there will be a check if the address calling this function has the rights by having paid for the feed subscription or has an oracle feed pass.

### `getFeedList`

```
function getFeedList(uint256[] memory feedIDs) external view returns(string[] memory, uint256[] memory, uint256[] memory, uint256[] memory, uint256[] memory)
```

�Accepts an array of feedIDs as an entry and returns metadata of all the feeds which have been requested.

Will return

* Feed Name
* Feed Decimals
* Feed Timeslots
* Feed Revenue Mode (free or with subscription)
* Feed Cost (if subscription)

### Basic Functions

### `withdrawFunds`

```
function withdrawFunds() external
```

�Withdraws funds to the payout address if funds have been accumulated by providing oracles services.

If the zero address is the payout address, this function will split the funds evenly accross all signers.

### `createNewFeeds`

```
function createNewFeeds(string[] memory names, string[] memory descriptions, uint256[] memory decimals, uint256[] memory timeslots, uint256[] memory feedCosts, uint256[] memory revenueModes) onlySigner external {
```

�This function creates 1..n new feeds according to the input values. The feeds are defined by

* Name
* Description
* Decimals
* Timeslots
* Costs
* Revenue Mode

### `submitFeed`

```
function submitFeed(uint256[] memory feedIDs, uint256[] memory values) onlySigner external
```

�This function is called by each singer. The signer can submit a set of feeds containing feedIDs and values.

The contract will check at which timeslot round of the corresponding feed the value is submitted and will place the value in the feed mapping of the feed.

If the signer threshold is met for a feed at during the timeslot the last price value and the last time updated value of the feed will be updated. The last price is the median price of all submitted prices of all signers in the timeslot.

Also the previous last price will be written into the historical values mapping for integrations.

### `signProposal`

```
function signProposal(uint256 proposalId) onlySigner external
```

�Signs a specific proposal. If the threshold is met the proposal will be executed.

### `createProposal`

```
function createProposal(uint256 uintValue, address addressValue, uint256 proposalType, uint256 feedId) onlySigner external {
```

�This function creates a new proposal. Proposals can be used to update the core contract values such as the signers, signer threshold or feed cost values.

Can only be executed and signed by a signer.

0 values can be used for types where the info is not needed for the proposal types, you can supply 0 uintValue if the proposals for a new signer address / you can supply the 0 address for updateFeedCost.

**Proposal Types**

* 0 updatePricePass(uintValue)
* 1 updateThreshold(uintValue)
* 2 addSigners(addressValue)
* 3 removeSigner(addressValue)
* 4 updatePayoutAddress(addressValue)
* 5 updateRevenueMode(feedId, uintValue)
* 6 updateFeedCost(feedId, uintValue)

### `updateFunctions`

```
function updatePricePass(uint256 newPricePass) private
```

```
function updateThreshold(uint256 newThresholdValue) private
```

```
function addSigners(address newSignerValue) private
```

```
function removeSigner(address toRemove) internal 
```

```
function updatePayoutAddress(address newPayoutAddressValue) private
```

```
function updateRevenueMode(uint256 newRevenueModeValue, uint256 feedId) private
```

```
function updateFeedCost(uint256 feedCost, uint256 feedId) private
```

Functions used to update contract values if a proposal is successful and executed.

### `subscribeToFeed`

```
function subscribeToFeed(uint256[] memory feedIDs, uint256[] memory durations, address buyer) payable external
```

Payable function which subscribes an address to the feeds provided in the arguments. Takes time in secs, as well as allows for aaddress gifting, useful for smart contracts having permission without needing to pay themselves.

A 2% fee is being sent to the Scry Router Contract and distributed to SCRY Founder NFT holders.

### `buyPass`

```
function buyPass(address buyer, uint256 duration) payable external
```

�This function allows an address to buy a pass for full access. With the pass all feeds can be accessed without having to buy subscriptions for each individual feed.

A 1% fee is being sent to the Scry Router Contract and distributed to SCRY Founder NFT holders.

### `supportFeeds`

```
function supportFeeds(uint256[] memory feedIds, uint256[] memory values) payable external
```

�This function can be used to support Feeds on a voluntary basis.

A 2% fee is being sent to the Scry Router Contract and distributed to SCRY Founder NFT holders.

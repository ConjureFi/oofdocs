# Use

### Interface

```solidity
interface Morpheus {
    function requestFeeds(
        string[] calldata APIendpoint,
        string[] calldata APIendpointPath,
        uint256[] calldata decimals,
        uint256[] calldata bounties
    ) external payable returns (uint256[] memory feeds);
     function supportFeeds(
        uint256[] calldata feedIds,
        uint256[] calldata values
    ) external payable;
}
```

## Requesting API through Morpheus

To request an API through Morpheus, you need to call the function `requestFeeds()` with the following parameters:

<pre class="language-solidity"><code class="lang-solidity">function requestFeeds(
        string[] memory APIendpoint,
        string[] memory APIendpointPath,
<strong>        uint256[] memory decimals,
</strong>        uint256[] memory bounties
    ) external payable returns (uint256[] memory feeds)
</code></pre>

* `APIendpoint`: This is the API endpoint that you want to call. You need to pass it as an array of strings.
* `APIendpointPath`: This is the path to the data that you want to extract from the API response. You need to pass it as an array of strings, separated by commas. For example, if you want to extract the "last" value from the response, you need to pass `['last']`.
* `decimals`: This is the number of decimals that you want to use for the API response. You need to pass it as an array of uint256 values.
* `bounties`: This is the bounty amount that you want to pay for the API request. You need to pass it as an array of uint256 values.

To use it with the sample input you provided, you would call the function like this:

```solidity
Morpheus morpheus = Morpheus(morpheusAddress);
string[] memory apiEndpoint = ['https://api.exchangerate.host/latest'];
string[] memory apiEndpointPath = ['rates,JPY'];
uint256[] memory decimals = [2];
uint256[] memory bounties = [20000000000000000];
uint256[] memory feeds = morpheus.requestFeeds{value: 20000000000000000}(apiEndpoint, apiEndpointPath, decimals, bounties);
```

In the code above, `morpheusAddress` is the address of the Morpheus contract. The `requestFeeds` function is called with the provided input, and 0.02 ETH is sent with the transaction. The return value is an array of feed IDs.

Note that you need to send enough ETH for the request for gas. If there's not enough bounty to make the transaction net 0 cost for the node, it won't be picked up.

### Updating a feed

To update a feed that you already requested, you can call the function `supportFeeds()` with the following parameters:

```solidity
function supportFeeds(uint256[] memory feedIds, 
        uint256[] memory values)
        external
        payable
```

* `feedIds`: This is the feed ID that you want to update. You need to pass it as an array of uint256 values.
* `values`: This is the new bounty amount that you want to pay for the updated feed. You need to pass it as an array of uint256 values.

Note that you need to check that the timestamp is newer than the last timestamp in your contract to make sure the update has been sent and the datas the new update.

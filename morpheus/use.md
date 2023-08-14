# Use / Request Feeds

Developers can implement the Morpheus interface in their smart contracts to enable fetching, retrieving and supporting feed data in a decentralized manner. The interface allows developers to specify the API endpoint, API endpoint path, decimals, and bounties when requesting new feed data, and to support feed data by specifying the feed ID and value.

### Interface

```solidity
interface Morpheus {
   
    function getFeed(uint256 feedID)
        external
        view
        returns (
            uint256 value,
            uint256 decimals,
            uint256 timestamp,
            string valStr
        );

    function getFeeds(uint256[] memory feedIDs)
        external
        view
        returns (
            uint256[] memory value,
            uint256[] memory decimals,
            uint256[] memory timestamp,
            string[] memory APIendpoint,
            string[] memory APIpath,
            string[] memory valStr,
        );

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

The Morpheus interface is a smart contract interface that defines the functions required for fetching, retrieving and supporting feed data in a decentralized manner.

#### Functions

### **`getFeed(uint256 feedID)`**

This function retrieves the value, decimals, and timestamp of a single feed.

**Parameters**

* `feedID` (uint256): The unique identifier of the feed request.

**Returns**

* `value` (uint256): The value of the feed.
* `decimals` (uint256): The number of decimal places for the feed value.
* `timestamp` (uint256): The timestamp of the feed value.
* valStr(string): The string value of the feed if used.

### **`requestFeeds(string[] calldata APIendpoint, string[] calldata APIendpointPath, uint256[] calldata decimals, uint256[] calldata bounties)`**

This function is used to request new feed data by specifying the API endpoint, API endpoint path, decimals and a bounty.

**Parameters**

* `APIendpoint` (string\[]): An array of API endpoints for the feeds.
* `APIpath` (string\[]): An array of API paths for the feeds.
* `decimals` (uint256\[]): An array of the number of decimal places for the feed values.
* `bounties` (uint256\[]): An array of bounties offered for providing the feed values in wei.

**Returns**

* `feeds` (uint256\[]): An array of unique identifiers assigned to the requested feeds.

### **`supportFeeds(uint256[] calldata feedIds, uint256[] calldata values)`**

This function is used to support feed data by specifying the feed ID and value.

**Parameters**

* `feedIds` (uint256\[]): An array of unique identifiers of the feeds to support.
* `values` (uint256\[]): An array of values to support for the corresponding feed IDs.

**Returns**

This function does not return any values.

### **`getFeeds(uint256[] memory feedIDs)`**

Description: This function is used to get the latest values of multiple feeds from the Morpheus oracle. It takes an array of feed IDs as input and returns an array of corresponding values, decimals, timestamps, API endpoints, and API paths.

Parameters:

* feedIDs (uint256\[] memory): An array of feed IDs to retrieve values for.

Returns:

* value (uint256\[] memory): An array of the latest values of the feeds corresponding to the provided feed IDs.
* decimals (uint256\[] memory): An array of the number of decimal places for the values of the feeds corresponding to the provided feed IDs.
* timestamp (uint256\[] memory): An array of the Unix timestamps (in seconds) of the latest updates for the feeds corresponding to the provided feed IDs.
* valueStr(string\[] memory): An array of the latest string values of the feeds corresponding to the provided feed IDs.
* APIendpoint (string\[] memory): An array of the API endpoints used to retrieve the values of the feeds corresponding to the provided feed IDs.
* APIpath (string\[] memory): An array of the API paths used to retrieve the values of the feeds corresponding to the provided feed IDs.

## Requesting API through Morpheus

To request an API through Morpheus, you need to call the function `requestFeeds()` with the following parameters:

<pre class="language-solidity"><code class="lang-solidity">function requestFeeds(
        string[] memory APIendpoint,
        string[] memory APIendpointPath,
<strong>        uint256[] memory decimals,
</strong>        uint256[] memory bounties,
    ) external payable returns (uint256[] memory feeds)
</code></pre>

* `APIendpoint`: This is the API endpoint that you want to call. You need to pass it as an array of strings.
* `APIendpointPath`: This is the path to the data that you want to extract from the API response. You need to pass it as an array of strings, separated by commas. For example, if you want to extract the "last" value from the response, you need to pass `['last']`.
* `decimals`: This is the number of decimals that you want to use for the API response. You need to pass it as an array of uint256 values.
* `bounties`: This is the bounty amount that you want to pay for the API request. You need to pass it as an array of uint256 values.

To request it with the sample input, you would call the function like this:

```solidity
Morpheus morpheus = Morpheus(morpheusAddress);

string[] memory apiEndpoint = new string[](1);
apiEndpoint[0] = "https://api.exchangerate.host/latest";

string[] memory apiEndpointPath = new string[](1);
apiEndpointPath[0] = "rates,JPY";

uint256[] memory decimals = new uint256[](1);
decimals[0] = 2;

uint256[] memory bounties = new uint256[](1);
bounties[0] = 20000000000000000;

uint256[] memory feeds = morpheus.requestFeeds{value: 20000000000000000}(apiEndpoint, apiEndpointPath, decimals, bounties);

```

Take feedID in the fn params or hardcode with the interface to get the feed value.

<pre class="language-solidity"><code class="lang-solidity"><strong>uint256[] memory feeds = x;
</strong><strong>uint256[] memory feedValue, uint256[] memory feedTimeStamps, uint256[] memory feedDecimals,,,string[] memory valStr = morpheus.getFeeds(feeds);
</strong><strong>for(uint n;n&#x3C;feeds.length;n++){
</strong><strong>feedValue[n] = feedValues[n]/10**feedDecimals[n];
</strong>}

</code></pre>

You can now use the feed value as needed.

Note that `feedValue / 10**feedDecimals`

Should be used for the real value represented by the front end. In the code above, `morpheusAddress` is the address of the Morpheus contract. The `requestFeeds` function is called with the provided input, and 0.02 ETH is sent with the transaction. The return value is an array of feed IDs.

Note that you need to send enough ETH for the request for gas. If there's not enough bounty to make the transaction net 0 cost for the node, it won't be picked up.

## VRF

You can `requestFeeds` with VRF by putting 'vrf' as the enpoint the salt you want to use as the path. The val will be a 256b uint.

```solidity
Morpheus morpheus = Morpheus(morpheusAddress);

string[] memory apiEndpoint = new string[](1);
apiEndpoint[0] = "vrf";

string[] memory apiEndpointPath = new string[](1);
apiEndpointPath[0] = "yoursalthere";

uint256[] memory decimals = new uint256[](1);
decimals[0] = 0;

uint256[] memory bounties = new uint256[](1);
bounties[0] = 10000000000000000;

uint256[] memory feeds = morpheus.requestFeeds{value: 10000000000000000}(apiEndpoint, apiEndpointPath, decimals, bounties);

```

0 ID with VRF&#x20;

<pre class="language-solidity"><code class="lang-solidity"> uint256[] memory times = new uint256[](1);
        uint256[] memory feedValue = new uint256[](1);
feedValue, times,,,, = morpheus.getFeeds(feeds);
//get a val from the VRF between 1-100
<strong>//feedValue[0] = feedValues[0]%100+1;
</strong>

//feedIds[0] after request filled
//9879752290979272170120564511694725973244445080113496454172745156547599793834
</code></pre>

0 ID with VRF 9879752290979272170120564511694725973244445080113496454172745156547599793834

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

## Jury - Human Defined Questions

You can use **Jury** as the API endpoint and the question in the path. This will then resolve by the oracle node when they question is answered. Be specific about what you want to know.

## Crosschain Data Lookup

The Crosschain Data Lookup `XCHAIN`feature offers two main functions, `XDATA` and `XBALANCE`, designed to allow for data extraction across various blockchains. This ability to perform crosschain lookups opens up a multitude of possibilities for complex applications.

### Parameters for XDATA/XBALANCE

* `RPC` - The RPC URL of the destination EVM compatible blockchain.
* `ADDRS` - The contract address to interact with.
* `DATA` - The encoded function call data. Must be ABI encoded, including fn signature. Make sure to check [https://docs.soliditylang.org/en/develop/abi-spec.html](https://docs.soliditylang.org/en/develop/abi-spec.html)

**Helper Script**

```javascript
const functionSig = 'balanceOf(address)';
const fnHash = ethers.utils.id(functionSig);  // keccak256 hash of function signature
const functionSelector = fnHash.slice(0, 10);  // first four bytes of hash         
const types = ['address'];
const values = ['0x9d31e30003f253563ff108bc60b16fdf2c93abb5'];
const encodedParams = ethers.utils.defaultAbiCoder.encode(types, values);
// Concatenate function selector and parameters
const encodedData = functionSelector + encodedParams.slice(2);  // remove '0x' from params

//0x70a082310000000000000000000000009d31e30003f253563ff108bc60b16fdf2c93abb5
```

* `FLAG` - A flag for XDATA for contract presets to make it easier to do common lookups like ERC20 balances using an address in the data rather than the full calldata. 0 for custom with any calldata.

**Constructed as**\
apiEndpoint = 'XCHAIN'\
This flags the oracle that you are making a XDATA or XBALANCE request for another chain.\
\
apiEndpointPath = "XDATA?RPC=" + EVMNETWORKRPCHERE + "\&ADDRS=" + TARGETCONTRACTADDRSHERE + "\&DATA=" + CALLDATAHERE + "\&FLAG=0";

### XDATA Function

The `XDATA`function performs a crosschain lookup for any read-only contract function.

**Usage**

```solidity
Morpheus morpheus = Morpheus(morpheusAddress);

string[] memory apiEndpoint = new string[](1);
apiEndpoint[0] = "XCHAIN";

string[] memory apiEndpointPath = new string[](1);
apiEndpointPath[0] = "XDATA?RPC=" + EVMNETWORKRPCHERE + "&ADDRS=" + TARGETCONTRACTADDRSHERE + "&DATA=" + CALLDATAHERE + "&FLAG=0";

uint256[] memory decimals = new uint256[](1);
decimals[0] = 0;

uint256[] memory bounties = new uint256[](1);
bounties[0] = BOUNTYVALUEHERE;

uint256[] memory feeds = morpheus.requestFeeds(apiEndpoint, apiEndpointPath, decimals, bounties);
```

In this usage example, `FLAG` is set to 0 which signifies a call to a function other than 'balanceOf'.

### XBALANCE Function

The `XBALANCE` function performs a crosschain lookup of an account's balance in the native token.

**Usage**

```solidity
Morpheus morpheus = Morpheus(morpheusAddress);

string[] memory apiEndpoint = new string[](1);
apiEndpoint[0] = "XCHAIN";

string[] memory apiEndpointPath = new string[](1);
apiEndpointPath[0] = "XBALANCE?RPC=" + EVMNETWORKRPCHERE + "&ADDRS=" + ACCOUNTADDRESSHERE;

uint256[] memory decimals = new uint256[](1);
decimals[0] = 0;

uint256[] memory bounties = new uint256[](1);
bounties[0] = BOUNTYVALUEHERE;

uint256[] memory feeds = morpheus.requestFeeds(apiEndpoint, apiEndpointPath, decimals, bounties);
```

These examples demonstrate how developers can leverage the Morpheus oracle contract to request crosschain data or balance. The execution time may vary based on the response from the destination chain and the gas price of the network where Morpheus is deployed.

### Crosschain Data Lookup - `getFeeds` Function

The `getFeeds` function retrieves the latest data from multiple feeds on the Morpheus oracle.

```solidity
function getFeeds(uint256[] memory feedIDs) public view returns (
    uint256[] memory value,
    uint256[] memory decimals,
    uint256[] memory timestamp,
    string[] memory valueStr,
    string[] memory APIendpoint,
    string[] memory APIpath
);
```

#### Parameters:

* `feedIDs (uint256[] memory)`: An array of feed IDs to retrieve values for.

### Returns

* `value (uint256[] memory)`: An array of the latest values of the feeds corresponding to the provided feed IDs.
* `timestamp (uint256[] memory)`: An array of Unix timestamps (in seconds) of the latest updates for the feeds corresponding to the provided feed IDs.
* `valueStr (string[] memory)`: An array of the latest string values of the feeds corresponding to the provided feed IDs. This is where the raw hexadecimal ABI-encoded data from an `XDATA` request is returned if the `FLAG` was set to `0`. This raw data can be decoded using Solidity's ABI decoding functions. [This example](https://solidity-by-example.org/abi-decode/) provides a helpful guide on how to decode the ABI in Solidity. On the other hand, for `XBALANCE` requests or ERC20 balance requests with `FLAG` set to `1`, the balance is returned as a decimal string in the `value` array.

## Sample NFT Chat with AI response contract

```
    function mint(address to, string calldata _tag) public payable virtual {
        require(msg.value >= price+10000000000000000);
        _mint(to, 1);
        tag[totalSupply()] = _tag;
        artist[totalSupply()] = msg.sender;
        time[totalSupply()] = block.timestamp;
        //create AI reply request
        string[] memory apiEndpoint = new string[](1);
        string[] memory apiEndpointPath = new string[](1);
        uint256[] memory decimals = new uint256[](1);
        uint256[] memory bounties = new uint256[](1);
//ai request
        apiEndpoint[0] = "GPT";
//prompt
        apiEndpointPath[0] = _tag;
        decimals[0] = 0;
        bounties[0] = 10000000000000000;
        
        uint256[] memory feeds = oracle.requestFeeds{value: 10000000000000000}(
            apiEndpoint,
            apiEndpointPath,
            decimals,
            bounties
        );
//store reply feed ID
        AI[totalSupply()] = feeds[0];
    }

    //_
    function getTag(
        uint256 id
    ) public view returns (string memory, address, uint256, string memory) {
//pull reply
         uint256[] memory feeds = new uint256[](1);
        feeds[0] = AI[id];
        string[] memory feedVal = new string[](1);

        (, , , , , feedVal) = oracle.getFeeds(feeds);
        return (tag[id], artist[id], time[id], feedVal[0]);
    }
```

---
description: Overview
---

# Contract Use and Request Feeds

Developers can implement the Morpheus interface in their smart contracts to enable fetching, retrieving and supporting feed data in a decentralized manner. The interface allows developers to specify the API endpoint, API endpoint path, decimals, and bounties when requesting new feed data, and to support feed data by specifying the feed ID and value.

## Interface

```solidity
interface Morpheus {
   
    function getFeed(uint256 feedID)
        external
        view
        returns (
            uint256 value,
            uint256 decimals,
            uint256 timestamp,
            string memory valStr
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
            string[] memory valStr
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

## Available Oracles and Addresses

### [https://morpheus.scry.finance/](https://morpheus.scry.finance/)

## **Jump to**

### - [**Requesting arbitrary APIs** ](./#requesting-api-through-morpheus)**-** [**VRFs**](vrf.md) **-** [**Crosschain Data Lookup** ](crosschain-data.md)**-** [**Human Based Requests**](jury-human-defined-questions.md)

### Functions

### **`getFeed(uint256 feedID)`**

This function retrieves the value, decimals, and timestamp of a single feed.

**Parameters**

* `feedID` (uint256): The unique identifier of the feed request.

**Returns**

* `value` (uint256): The value of the feed.
* `decimals` (uint256): The number of decimal places for the feed value.
* `timestamp` (uint256): The timestamp of the feed value.
* valStr(string): The string value of the feed if used.

**If the datas a string then value will be 88888888 to flag that its a string. If negative valueStr will be set to 'negative' to flag it and value as uint.**

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

If the datas a string then value will be 88888888 to flag that its a string. If negative valueStr will be set to 'negative' to flag it and value as uint.

## Bounties and Fees

The `requestFeeds`function is used to put a bounty for the oracle to fill a request and will only do so if profitable in gas used to fill the request. If there's not enough bounty to make the transaction net 0 cost for the node, it won't be picked up.&#x20;

**Rec Fee**

**L2** - 0.0001 ETH / feed request

To ping a feed that you already requested with extra bounty if not enough, you can call the function `supportFeeds()` with the following parameters:

```solidity
function supportFeeds(uint256[] memory feedIds, 
        uint256[] memory values)
        external
        payable
```

* `feedIds`: This is the feed ID that you want to ping. You need to pass it as an array of uint256 values.
* `values`: This is the new bounty amount that you want to pay ontop current for the feed. You need to pass it as an array of uint256 values.

Note - some oracles have 0 bounty fee as they are free public goods with gas costs compensated by grants.\
These are found on [https://morpheus.scry.finance/](https://morpheus.scry.finance/)

## Updating a feed

To update a feed that you already requested with a fresh value, you can call the function `supportFeeds()` with the following parameters:

```solidity
function supportFeeds(uint256[] memory feedIds, 
        uint256[] memory values)
        external
        payable
```

* `feedIds`: This is the feed ID that you want to update. You need to pass it as an array of uint256 values.
* `values`: This is the new bounty amount that you want to pay for the updated feed. You need to pass it as an array of uint256 values.

Note that you need to check that the timestamp is newer than the last timestamp in your contract to make sure the update has been sent and the datas the new update. VRFs will not refresh.

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

## Updating a feed

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

## Hex data&#x20;

If you need hex for your contract, the oracle will submit as a string. You can convert back to hex using this to be able to ABI decode.

```solidity
    function stringToBytes(string memory hexString) public pure returns (bytes memory) { bytes memory bstr = bytes(hexString); require(bstr.length >= 2, "Input string too short");
    // Skip '0x' prefix if present
    uint offset = 0;
    if(bstr[0] == '0' && (bstr[1] == 'x' || bstr[1] == 'X')) {
        offset = 2;
    }

    require((bstr.length - offset) % 2 == 0, "Hex string must have an even number of characters");

    bytes memory bytesArray = new bytes((bstr.length - offset) / 2);
    for (uint i = 0; i < bytesArray.length; i++) {
        bytes1 tmp1 = bstr[i*2 + offset];
        bytes1 tmp2 = bstr[i*2 + offset + 1];
        bytesArray[i] = bytes1(hexCharToUint(tmp1) * 16 + hexCharToUint(tmp2));
    }
    return bytesArray;
}

function hexCharToUint(bytes1 c) internal pure returns (uint8) {
    if (uint8(c) >= uint8(bytes1('0')) && uint8(c) <= uint8(bytes1('9'))) {
        return uint8(c) - uint8(bytes1('0'));
    }
    if (uint8(c) >= uint8(bytes1('a')) && uint8(c) <= uint8(bytes1('f'))) {
        return 10 + uint8(c) - uint8(bytes1('a'));
    }
    if (uint8(c) >= uint8(bytes1('A')) && uint8(c) <= uint8(bytes1('F'))) {
        return 10 + uint8(c) - uint8(bytes1('A'));
    }
    revert("Invalid hexadecimal character");
}
```

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

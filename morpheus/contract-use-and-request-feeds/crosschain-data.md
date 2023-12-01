# Crosschain Data

## Crosschain Data Lookup

The Crosschain Data Lookup `XCHAIN`feature offers two main functions, `XDATA` and `XBALANCE`, designed to allow for data extraction across various blockchains. This ability to perform crosschain lookups opens up a multitude of possibilities for complex applications.

### Parameters for XDATA/XBALANCE

* `RPC` - The RPC URL of the destination EVM compatible blockchain.
* `ADDRS` - The contract address to interact with.
* `DATA` - The encoded function call data. Must be ABI encoded, including fn signature. Make sure to check [https://docs.soliditylang.org/en/develop/abi-spec.html](https://docs.soliditylang.org/en/develop/abi-spec.html)

**Helper Script for** `DATA`&#x20;

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
apiEndpointPath = "XDATA?RPC=" + <mark style="color:purple;">EVMNETWORKRPCHERE</mark> + "\&ADDRS=" + <mark style="color:purple;">TARGETCONTRACTADDRSHERE</mark> + "\&DATA=" + <mark style="color:purple;">CALLDATAHERE</mark> + "\&FLAG=<mark style="color:purple;">0</mark>";

\
<mark style="color:purple;">**Purple values**</mark>** must be replaced with your values**

### XDATA Function

The `XDATA`function performs a crosschain lookup for any read-only contract function.

**Usage**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.12;

interface Morpheus {
    function getFeed(
        uint256 feedID
    )
        external
        view
        returns (
            uint256 value,
            uint256 decimals,
            uint256 timestamp,
            string memory valStr
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

contract CrosschainLookup {
    Morpheus morpheus = Morpheus(0x0000000000071821e8033345A7Be174647bE0706);
    mapping(address => mapping(address => uint256)) public userBalance;
    mapping(address => mapping(address => uint256)) public userBalanceFeed;
    string public RPC = "https://eth.llamarpc.com";
    address public owner;

    constructor() payable {
        owner = msg.sender;
    }

    function getBalance(address target, address TOKEN) public payable {
        string[] memory apiEndpoint = new string[](1);
        apiEndpoint[0] = "XCHAIN";

        // ABI encode the balanceOf function and the address
        bytes memory data = abi.encodeWithSignature(
            "balanceOf(address)",
            target
        );

        string[] memory apiEndpointPath = new string[](1);
        apiEndpointPath[0] = string.concat(
            "XDATA?RPC=",
            RPC,
            "&ADDRS=",
            bytesToHexString(addressToBytes(TOKEN)),
            "&DATA=",
            bytesToHexString(data),
            "&FLAG=0"
        );

        uint256[] memory decimals = new uint256[](1);
        decimals[0] = 0;

        uint256[] memory bounties = new uint256[](1);
        bounties[0] = .01 ether; // Replace with actual bounty value

        uint256[] memory feeds = morpheus.requestFeeds{value: .01 ether}(
            apiEndpoint,
            apiEndpointPath,
            decimals,
            bounties
        );
        userBalanceFeed[target][TOKEN] = feeds[0]; // Storing the feed ID here, to be decoded in setMyBalance
    }

    function addressToBytes(
        address _address
    ) public pure returns (bytes memory) {
        bytes20 addressBytes = bytes20(_address);
        bytes memory result = new bytes(20);
        for (uint i = 0; i < 20; i++) {
            result[i] = addressBytes[i];
        }
        return result;
    }

    function bytesToHexString(
        bytes memory data
    ) public pure returns (string memory) {
        bytes memory alphabet = "0123456789abcdef";

        bytes memory str = new bytes(2 + data.length * 2);
        str[0] = "0";
        str[1] = "x";
        for (uint i = 0; i < data.length; i++) {
            str[2 + i * 2] = alphabet[uint(uint8(data[i] >> 4))];
            str[3 + i * 2] = alphabet[uint(uint8(data[i] & 0x0f))];
        }
        return string(str);
    }

    function setBalance(address target, address token) public {
        (uint256 balance, uint256 timestamp,, ) = morpheus.getFeed(
            userBalanceFeed[target][token]
        );
       require(timestamp >= block.timestamp - 10000, "Data is too old");
        userBalance[target][token] = balance;
    }
}
```

In this usage example, `FLAG` is set to 0 which signifies a call to a function other than 'balanceOf'. The addresses are handled as strings as required by the node, all requests must be as a string, and you can use the tools in this sol to go from bytes to strs as needed.

### XBALANCE Function

The `XBALANCE` function performs a crosschain lookup of an account's balance in the native token.

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
* `valueStr (string[] memory)`: An array of the latest string values of the feeds corresponding to the provided feed IDs. This is where the raw hexadecimal ABI-encoded data from an `XDATA` request is returned if the `FLAG` was set to `0`. This raw data can be decoded using Solidity's ABI decoding functions. [This example](https://solidity-by-example.org/abi-decode/) provides a helpful guide on how to decode the ABI in Solidity. On the other hand, for `XBALANCE` requests or ERC20 balance requests with `FLAG` set to `1`, the balance is returned as a decimal string in the `value` array. To handle first do bytes(str) to then be able to decode.

##

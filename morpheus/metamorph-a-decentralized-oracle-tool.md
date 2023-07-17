# MetaMorph: A Decentralized Oracle Tool



## Introduction

MetaMorph is a flexible and robust Oracle service that allows developers to request data from multiple independent Oracle sources. This system gives developers more control and choice over where they source their data from. Key features include a quorum mechanism to ensure a sufficient number of sources have submitted data and threshold parameters to guarantee data freshness. The system also supports callback requests.

In this guide, we will go over the key aspects of using MetaMorph, providing detailed explanations and examples for various use cases.

## How to request data from multiple oracles

Requesting data from multiple oracles is a simple process in the MetaMorph contract. This can be achieved by calling the `requestFeed` function and specifying a list of `morpheus` addresses (representing the oracles), the `APIendpoint`, `APIendpointPath`, and `decimals` for the data you want, and an array of `bounties` to be paid to each oracle.

```solidity
function requestFeed(
        address[] memory morpheus,
        string memory APIendpoint,
        string memory APIendpointPath,
        uint256 decimals,
        uint256[] memory bounties
    ) external payable returns (uint256[] memory);
```

### Choice of data source

By providing an array of Morpheus (oracles) addresses, the MetaMorph contract allows developers to specify which oracles they want to source their data from. This feature provides a level of customization and can increase the trustworthiness of the data by diversifying the data sources.

### Independent oracles

The Morpheus oracles operate independently of one another. This feature means that each oracle provides its data, which is fetched from its specified API endpoint, and that data is not influenced by any other oracle. This design increases the robustness and decentralization of the Morpheus system.

### Threshold for fresh data

The MetaMorph contract allows developers to set a `threshold` parameter, which ensures that only fresh data is returned. If the timestamp of the data is within the threshold, the data is considered valid; otherwise, it is disregarded. This parameter can be set in both the `getFeeds` and `getFeedsQuorum` functions.

```solidity
function getFeeds(
        address[] memory morpheus,
        uint256[] memory IDs,
        uint256 threshold
    ) external view returns (uint256 value, string memory valStr, bytes memory valBytes);
```

### Quorum for source validation

The `getFeedsQuorum` function includes a `quorum` parameter, which ensures that a sufficient number of sources have submitted their data before a value is returned. This feature enhances the reliability of the data by requiring a minimum number of validations.

```solidity
function getFeedsQuorum(
        address[] memory morpheus,
        uint256[] memory IDs,
        uint256 threshold,
        uint256 quorum
    ) external view returns (uint256 value, string memory valStr, bytes memory valBytes);
```

### Requests with callbacks

The MetaMorph contract supports requests with callbacks. This feature allows developers to call the `requestFeedCallback` function, which triggers an event `dataCallbackRequested` after the feed request is made. When the data is ready, the `fillRequest` function can be called by watchers nodes, which triggers a callback to the specified receiver address with the fetched data.

```solidity
function requestFeedCallback(
        address[] memory morpheus,
        string memory APIendpoint,
        string memory APIendpointPath,
        uint256 decimals,
        uint256[] memory bounties,
        uint threshold,
        uint quorum,
        address receiveAddrs,
        uint256 bountyGuardian
    ) external payable returns (uint256[] memory);
```

**Parameters:**

* `morpheus[]` (address\[]): Array of addresses of the Oracle contracts to request data from.
* `APIendpoint` (string): The API endpoint from which the Oracle should fetch data.
* `APIendpointPath` (string): The specific path within the API from which the data should be fetched.
* `decimals` (uint256): Number of decimal places the returned data should have.
* `bounties[]` (uint256\[]): Array of bounties (payment) for each Oracle.
* `threshold` (uint): Maximum age of data to be accepted, in seconds.
* `quorum` (uint): Minimum number of Oracles that need to respond for the data to be accepted.
* `receiveAddrs` (address): Address to send the callback to once data is available.
* `bountyGuardian` (uint256): Bounty for the Guardian who fills the request.

**Returns:**

* `ids[]` (uint256\[]): Array of IDs of the feed data requested from each Oracle.
* requestID (uint256): ID of the data request.

### Conclusion

The MetaMorph contract is a powerful tool that allows developers to fetch, validate, and process data from multiple, independent oracles in a decentralized and customizable way. By leveraging features like threshold checks, quorum requirements, and callback functionality, developers can ensure that their data is secure and decentralized.\
\
Sample

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.0;
// The MetaMorph interface interface 
IMetaMorph { function requestFeedCallback( address[] memory morpheus, string memory APIendpoint, string memory APIendpointPath, uint256 decimals, uint256[] memory bounties, uint threshold, uint quorum, address receiveAddrs, uint256 bountyGuardian ) external payable returns (uint256[] memory); }
contract MyContract { IMetaMorph public metamorph;// This is the data we're interested in
uint256 public receivedData;

// Event to log the received data
event DataReceived(uint256 data);

// Set the address of the MetaMorph contract on deployment
constructor(address _metamorph) {
    metamorph = IMetaMorph(_metamorph);
}

// Request data from the MetaMorph contract
function requestData() external payable {
    address[] memory morpheus = new address[](3);
    morpheus[0] = 0xOracleAddress1; // replace with actual oracle address
    morpheus[1] = 0xOracleAddress2; // replace with actual oracle address
    morpheus[2] = 0xOracleAddress3; // replace with actual oracle address
    
    uint256[] memory bounties = new uint256[](3);
    bounties[0] = 0.001 ether; // replace with actual bounty
bounties[1] = 0.001 ether; // replace with actual bounty
bounties[2] = 0.001 ether; // replace with actual bounty

    metamorph.requestFeedCallback{value:0.004 ether}(
        morpheus,
        "https://api.example.com", 
        "data", 
        2, 
        bounties, 
        600, 
        1, 
        address(this), 
        0.001 ether
    );
}

// The callback function that the MetaMorph contract will call
function dataCallback(uint256 data) external {
    // Ensure the call is coming from the MetaMorph contract
    require(msg.sender == address(metamorph), "Only MetaMorph can call this function");
    receivedData = data;
    // Emit the event
    emit DataReceived(data);
}
}
```


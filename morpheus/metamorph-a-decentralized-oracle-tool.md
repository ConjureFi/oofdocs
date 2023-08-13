# MetaMorph: A Decentralized Oracle Tool

## Introduction

MetaMorph is a flexible and robust Oracle service that allows developers to request data from multiple independent Oracle sources. This system gives developers more control and choice over where they source their data from. Key features include a quorum mechanism to ensure a sufficient number of sources have submitted data and threshold parameters to guarantee data freshness. The system also supports callback requests.

You can explore some available oracles for your chosen network here. This is not all available oracles, and you are able to even host your own oracle yourself\
[https://morpheus.scry.finance/](https://morpheus.scry.finance/)

#### Build your own oracle

[https://docs.scry.finance/docs/morpheus/build-your-own-oracle](https://docs.scry.finance/docs/morpheus/build-your-own-oracle)

## How to request data from multiple oracles

### Interface

```solidity
// SPDX-License-Identifier: SCRY
pragma solidity 0.8.6;

interface IMetaMorph {
    function getFeeds(
        address[] calldata morpheus,
        uint256[] calldata IDs,
        uint256 threshold
    ) external view returns (uint256 value, string memory valStr, bytes memory valBytes);

    function getFeedsQuorum(
        address[] calldata morpheus,
        uint256[] calldata IDs,
        uint256 threshold,
        uint256 quorum
    ) external view returns (uint256 value, string memory valStr, bytes memory valBytes);

    function requestFeed(
        address[] calldata morpheus,
        string calldata APIendpoint,
        string calldata APIendpointPath,
        uint256 decimals,
        uint256[] calldata bounties
    ) external payable returns (uint256[] memory);

    function requestFeedCallback(
        address[] calldata morpheus,
        string calldata APIendpoint,
        string calldata APIendpointPath,
        uint256 decimals,
        uint256[] calldata bounties,
        uint threshold,
        uint quorum,
        address receiveAddrs,
        uint256 bountyGuardian
    ) external payable returns (uint256[] memory, uint requestID);

    function fillRequest(uint256 ID) external;

    function refillRequest(uint256 ID,uint guardianBounty) external payable;

    function updateFeeds(
        address[] calldata morpheus,
        uint256[] calldata IDs,
        uint256[] calldata bounties
    ) external payable;
}

```

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

### Deployments

**Sepolia**

[0xcFEf2da2bcd697769ca9548CD6672133F2adf95C](https://sepolia.etherscan.io/address/0xcFEf2da2bcd697769ca9548CD6672133F2adf95C)

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
    ) external payable returns (uint256[] memory,uint ID);
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

**You must implement the following in your contract that receives the callback**

### `requestCallback` Function

**Description**

The `requestCallback` function is called by the MetaMorph contract when the requested data is ready. It ensures that the call is coming from the MetaMorph contract and that the request ID matches the expected value. Once the data is validated, it is stored and an event is emitted to notify that the data has been received.

```solidity
function requestCallback(
        uint256 data,
        string calldata strdata,
        bytes calldata bytesdata,
        uint reqID
    ) external {
    
    }
```

**Parameters**

* `data` (`uint256`): The numerical data fetched by the Oracle.
* `strdata` (`string`): A string representation of the data fetched by the Oracle. This can be used to provide additional context or details about the data.
* `bytesdata` (`bytes`): A bytes representation of the data fetched by the Oracle. This can be used to provide the data in a different encoding or format.
* `reqID` (`uint`): The request ID associated with the data request. This must match the request ID that was returned by the `requestFeedCallback` function in the MetaMorph contract.

#### Security Considerations

* The function should be called by the MetaMorph contract. If the sender is not the MetaMorph contract, the transaction should be reverted
* The `reqID` parameter must match the `requestID` expected by the receiving contract. If there is a mismatch, the transaction will be reverted with the message "Request mismatch."

**Example Usage**

```solidity
function requestCallback(
        uint256 data,
        string calldata strdata,
        bytes calldata bytesdata,
        uint reqID
    ) external {
        require(
            msg.sender == address(metamorph),
            "Only MetaMorph can call this function"
        );
        require(requestID == reqID, "Request mismatch");
        receivedData = data;
        receivedStrData = strdata;
        mycustomAddrs = address(bytesdata);
        emit DataReceived(data,strdata,bytesdata);
    }
```

**Notes**

* Make sure to define the `metamorph` address and the `requestID` variable in your contract, as they are used in the requirements.
* The `strdata` and `bytesdata` parameters can be used to provide additional information about the data, depending on the specific use case and the API being queried.

### Conclusion

The MetaMorph contract is a powerful tool that allows developers to fetch, validate, and process data from multiple, independent oracles in a decentralized and customizable way. By leveraging features like threshold checks, quorum requirements, and callback functionality, developers can ensure that their data is secure and decentralized.

## **Sample**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// The MetaMorph interface interface
interface IMetaMorph {
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
    ) external payable returns (uint256[] memory,uint requestID);

    function refillRequest(uint256 ID, uint bountyGuardian) external payable;
}

contract MyContract {
    IMetaMorph public metamorph; // This is the data we're interested in
    uint256 public receivedData;
    uint bounties = 3;
    uint requestID;
    // Event to log the received data
    event DataReceived(uint256 data);

    // Set the address of the MetaMorph contract on deployment
    constructor(address _metamorph) {
        metamorph = IMetaMorph(_metamorph);
        requestData();
    }

    // Request data from the MetaMorph contract
    function requestData() internal {
        address[] memory morpheus = new address[](3);
        morpheus[0] = 0x0000000000071821e8033345A7Be174647bE0706; // replace with actual oracle address
        morpheus[1] = 0x0000000000071821e8033345A7Be174647bE0706; // replace with actual oracle address
        morpheus[2] = 0x0000000000071821e8033345A7Be174647bE0706; // replace with actual oracle address
        uint256[] memory bounty = new uint256[](3);
        bounty[0] = 0.001 ether; // replace with actual bounty
        bounty[1] = 0.001 ether; // replace with actual bounty
        bounty[2] = 0.001 ether; // replace with actual bounty
        (,requestID) = metamorph.requestFeedCallback{value: 0.004 ether}(
            morpheus,
            "https://api.example.com",
            "pathtodata",
            2, //dec
            bounty,
            600,
            2,
            address(this),
            0.001 ether
        );
    }

    // Refresh data from the MetaMorph contract
    function refreshData() public payable {
        metamorph.refillRequest{value: msg.value}(
            requestID,
            msg.value - ((msg.value * bounties) / (bounties + 2))
        );
    }

    // The callback function that the MetaMorph contract will call
    function requestCallback(
        uint256 data,
        string calldata strdata,
        bytes calldata bytesdata,
        uint reqID
    ) external {
        // Ensure the call is coming from the MetaMorph contract
        require(
            msg.sender == address(metamorph),
            "Only MetaMorph can call this function"
        );
        require(requestID == reqID, "Request mismatch");
        receivedData = data;
        // Emit the event
        emit DataReceived(data);
    }
}
```


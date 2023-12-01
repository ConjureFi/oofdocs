# Jury - Human Defined Questions

You can use **Jury** as the endpoint and the question in the path. This will then resolve by the oracle node when they question is answered. Be specific about what you want to know.

```solidity
pragma solidity ^0.8.0;

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
}

contract Jury {
    Morpheus public morpheus;
    mapping(uint256=> string ) public ID;
    uint oracleFee = 100000000000000;
    uint nID;
    constructor() {
        morpheus = Morpheus(0x0000000000071821e8033345A7Be174647bE0706);
    }

    function play(string memory question) public{
     require(msg.value>=oracleFee);
     request(question);
    }


    function request(string memory question) internal {
        string[] memory apiEndpoint = new string[](1);
        apiEndpoint[0] = "Jury";
        string[] memory apiEndpointPath = new string[](1);
        apiEndpointPath[0] = question;
        uint256[] memory decimals = new uint256[](1);
        decimals[0] = 0;
        uint256[] memory bounties = new uint256[](1);
        bounties[0] = oracleFee ;
        uint256[] memory feeds = morpheus.requestFeeds{value: oracleFee }(
            apiEndpoint,
            apiEndpointPath,
            decimals,
            bounties
        );
    ID[nID]= feeds[0];
    nID++;
    }

    function myReply() public returns (
            string memory valStr
        ) {
        (, , , string memory Value) = morpheus.getFeed(vrfID);
        require(Value != 0, "Oracle not ready");
     return (valStr);
    }
}

```

**NOT ALL ORACLES WILL SUPPORT THIS. Please check with the oracle first if they run nodes autonomously or are supporting Human Defined Questions.**

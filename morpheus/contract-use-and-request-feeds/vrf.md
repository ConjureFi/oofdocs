# VRF

You can `requestFeeds` for a VRF by putting 'vrf' as the endpoint, the salt you want to use as the path. The val will be a 256b uint VRF. Note each feedIDs a unique VRF so for a fresh VRF you need to use `requestFeeds` for each VRF. More info in\
[https://docs.scry.finance/docs/morpheus/vrf-hash-ranch](vrf.md#vrf)

**Sample**

<pre class="language-solidity"><code class="lang-solidity">// SPDX-License-Identifier: MIT
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

contract VRF{
    Morpheus public morpheus;
    uint vrfID;
    uint oracleFee = 100000000000000;
    address winner;
    address player;
    constructor() {
        morpheus = Morpheus(0x0000000000071821e8033345A7Be174647bE0706);
    }

    function play() public{
     require(msg.value>=oracleFee);
    require(player==address(0));
   player=msg.sender;
     requestVRF();
    }


    function requestVRF() internal {
        string[] memory apiEndpoint = new string[](1);
        apiEndpoint[0] = "vrf";
        string[] memory apiEndpointPath = new string[](1);
        apiEndpointPath[0] = "";
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
    vrfID= feeds[0];
    }

    function determineWinner() public {
        require(
            vrfID!=0
        );
        (uint256 vrfValue, , , ) = morpheus.getFeed(vrfID);
        require(vrfValue != 0, "Oracle not ready");
        uint256 roll = (vrfValue % 100) + 1;
        if (roll>51){winner=player;}
<strong>        player=address(0);
</strong>    }
}
</code></pre>

### **See for more info**

{% content-ref url="../vrf-hash-ranch.md" %}
[vrf-hash-ranch.md](../vrf-hash-ranch.md)
{% endcontent-ref %}

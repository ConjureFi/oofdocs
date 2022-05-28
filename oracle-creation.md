# Oracle Creation

### Creating an Oracle and Feeds

#### Step 1. Go to the factory at

[https://polygonscan.com/address/0x00f0fEED50926c27261dF37f7bCcC519d87525D0#writeContract](https://polygonscan.com/address/0x00f0fEED50926c27261dF37f7bCcC519d87525D0#writeContract) [https://etherscan.com/address/0x00f0fEED50926c27261dF37f7bCcC519d87525D0#writeContract](https://etherscan.com/address/0x00f0fEED50926c27261dF37f7bCcC519d87525D0#writeContract)

#### 2. Call oofMint

signers\_ (address\[]) - The singners for the oracle, these are responsible for feed updates, proposals and contract updates and earn fees

signerThreshold\_ (uint256) - The minimum signers needed to submit a feed and takes the median value submitted to authenticate a feed to update.

payoutAddress\_ (address) - Can be 0x0, in which case the fees will be split across all signers or can be a set address to take all fees, usable for cold wallets or gnosis/other contracts to handle the fees

subscriptionPassPrice\_ (uint256) - The price to purchase a subscription for 28 days to all oracle feeds, in wei

#### 3. OOFNode Setup

#### &#xD;[https://github.com/ConjureFi/OOFNode](https://github.com/ConjureFi/OOFNode)

00FNode's a simple, lightweight and easy to use open source node to use with the Open Oracle Framework to automate feed submission for onchain oracles and provide feeds, using a google spreadsheet for collaborative and transparent queries and making feed management easy for users. The node is simple to setup and will run autonomously, pulling from APIs in the spreadsheet and submitting the transactions to update the onchain feeds for any network via customizable RPC.



**Installation**

Simply clone into this repo



`git clone https://github.com/ConjureFi/OOFNode`



Then create a .env with the following parameters



`PK=PRIVATEKEY`

`RPC=RPCURLWITHAPIKEY`

`OOFAddress=OOFCONTRACTADDRESS`

`SHEETID=ID / 1syqS8Gpl7ZS9UC_Wr6giY057XebJu3bZKXhIDsN-DJ0`

`SHEETAPI=KEY`

`SHEETTITLE=ETHEREUM/POLYGON/RINKEBY`



Then cd into the dir and run



`node OOFNode.js`



The node will automatically start submitting feeds every hour from the provided private key to the provided OOF address.



**Feeds Setup**

Feeds can be created and setup based on the linked spreadhsheet by simply running **setupFeeds.js** which will then check and if needed create the feeds in the Oracle Feeds struct&#x20;

Sample Spreadsheet template to fork https://docs.google.com/spreadsheets/d/1syqS8Gpl7ZS9UC\_Wr6giY057XebJu3bZKXhIDsN-DJ0 to setup the node

#### 4. You're all done, you're now providing onchain feeds using your own custom oracle!

# Build Your Own Oracle

**Introduction**

Welcome to the Scry deployment guide. This comprehensive guide will walk you through the process of oracle deployment on any EVM network, complete with custom signers, API endpoints, and high-scale oracle feeds. Scry oracles are fully self-deployable, decentralized, and allow you to host your own nodes.

**Scry and Morpheus**

Scry enables high-scale data access, providing you with the data you need when you need it. Scry also introduces Morpheus, a framework that allows developers to request any API from web2 directly on-chain for their web3 apps in real-time. Morpheus makes it easier for developers to build decentralized applications that interact with the real world, bridging the gap between web2 and web3.

**Morpheus Nodes**

Morpheus nodes, self-hostable oracles, are a key component of the Scry ecosystem. They earn fees from bounty requests for data, enabling nodes to be fully autonomous and create passive revenue. Scry allows developers and projects to choose their data sources based on reputation and even source data from their own community.

## For Developers

**Preparation**

Before starting, familiarize yourself with Scry's documentation. Go to the deployments section in the docs to find your factory. If your network is unsupported, fork the GitHub repository\
[https://github.com/ScryProtocol/contracts](https://github.com/ScryProtocol/contracts)\
&#x20;and use your preferred framework. For deploying your oracle  , you can use `createMorph.js` in step 3 instead of `initialize.js`.

**Deployment Steps**

_Step 1: Clone the repository and install dependencies_

1. Clone the repo from github.com/scryprotocol/NodeDeploy.
2. Run `npm install` to install necessary dependencies.

_Step 2: Set up your oracle in the .env file_

`RPC=https://rpc.sepolia.org`

`OOFAddress=`

`PK=`

1. Set up the RPC for any EVM network where your contract is deployed (e.g., Goerli).
2. Set the OOFAddress to the oracle address you deployed.
3. Set the PK to your private key for your oracle signer.
4. In `initialize.js`, change the signers to the address for your private key.

_Step 3: Initialize the oracle_

1. In your environment, run `node initialize.js` to set up your oracle. You can skip this if you used createMorph.js to deploy the contract.

_Step 4: Run your Morpheus node_

1. Run `node morpheus.js` to start your node.
2. Host your node on an AWS EC2 free VPS or locally. Your node will automatically run, update all feeds, and earn fees.

## For Non-Technical Users

### Features

* Deploy your own autonomous oracle infrastructure to ANY EVM network in <60s.
* Support any API endpoint in realtime, allowing requests to be made fully onchain to the oracle fully permissionlessly
* Earn fees from requests for data, only filling requests if profitable
* 1 click deployment and setup. No need for any prereqs just launch out of the box ready to use. No developer experience or technical experience needed.
* Fully custom VRF and proof system with cryptographically secure 256b Hash RanCh VRFs

1. Download the binary

Linux [https://github.com/ScryProtocol/Contracts/raw/Scry/morpheus-linux.zip](https://github.com/ScryProtocol/Contracts/raw/Scry/morpheus-linux.zip)

MacOS [https://github.com/ScryProtocol/Contracts/raw/Scry/morpheus-mac.zip](https://github.com/ScryProtocol/Contracts/raw/Scry/morpheus-mac.zip)

Windows [https://github.com/ScryProtocol/Contracts/raw/Scry/morpheus-windows.zip](https://github.com/ScryProtocol/Contracts/raw/Scry/morpheus-windows.zip)

### Usage

Simply download the binaries. Put your private key for your oracle signer in the .env and run the node to deploy.

#### .env

<pre class="language-properties"><code class="lang-properties">RPC=
OOFAddress=
PK=
<strong>OPENAI_API_KEY=
</strong></code></pre>

Set the RPC for any EVM network where your contract is deployed (Goerli).&#x20;

OPTIONAL - Set the OOFAddress to the oracle address you deployed.

Set the PK to your private key for your oracle signer.

OPTIONAL - Set the OPENAI\_API\_KEY for chatGPT support.

For those using the non dev binaries use your prefered terminal such as cmd on windows. Then go to the binary location and do morpheus.exe or just run it as usual like any other app.

Oracles will automatically be deployed, setup and then run autonomously. Make sure to give a little in tokens for gas like ETH. Oracles will refill based on requests.

## Params OPTIONAL

\-a Used to set the oracle address if already deployed&#x20;

\-r Used to set the RPC for an EVM network&#x20;

\-pk Used to set the PK used for the oracle signer and deployment

Sample

`morpheus -a 0x00f0000000F11a5380Da5A184F0C563B5995fee2 -r https://sepolia.infura.io/v3/6822e4e6edc847829086404ffe6d5b2b -pk 0000000000000000000000000000000000000000000000000000000000`

**Support**

For support, join the Scry Discord or visit scry.finance to chat and learn more about the project.

Happy deploying!

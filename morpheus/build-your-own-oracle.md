# Build Your Own Oracle

**Introduction**

Welcome to the Scry deployment guide. This comprehensive guide will walk you through the process of oracle deployment on any EVM network, complete with custom signers, API endpoints, and high-scale oracle feeds. Scry oracles are fully self-deployable, decentralized, and allow you to host your own nodes.

**Scry and Morpheus**

Scry enables high-scale data access, providing you with the data you need when you need it. Scry also introduces Morpheus, a framework that allows developers to request any API from web2 directly on-chain for their web3 apps in real-time. Morpheus makes it easier for developers to build decentralized applications that interact with the real world, bridging the gap between web2 and web3.

**Morpheus Nodes**

Morpheus nodes, self-hostable oracles, are a key component of the Scry ecosystem. They earn fees from bounty requests for data, enabling nodes to be fully autonomous and create passive revenue. Scry allows developers and projects to choose their data sources based on reputation and even source data from their own community.

## For Developers

**Preparation**

Before starting, familiarize yourself with Scry's documentation.&#x20;

\
OPTIONAL - Fork the GitHub repository\
[https://github.com/ScryProtocol/contracts](https://github.com/ScryProtocol/contracts)\
&#x20;and use your preferred framework to deploy nostradamus.sol.&#x20;

\
Then fork into [https://github.com/ScryProtocol/morpheus](https://github.com/ScryProtocol/morpheus)\
For the tools and nodes.

OPTIONAL- For deploying your oracle manually, you can use `initialize.js`. It will perform initialize for you

If you run morpheous.js, it will perform both create and initialize for you if there's no oracle address set in the .env aswell as start the node.

**Deployment Steps**

_Step 1: Clone the repository and install dependencies_

1. Clone the repo from github.com/scryprotocol/morpheus\
   `git clone https://github.com/scryprotocol/morpheus`
2. Run `npm install` to install necessary dependencies.

_Step 2: Set up your oracle in the .env file_

`RPC=https://rpc.sepolia.org`

`OOFAddress=`

`PK=`

1. Set up the RPC for any EVM network where your contract is deployed (e.g., Goerli).
2. OPTIONAL - Set the OOFAddress to the oracle address you deployed leave empty to have morpheous.js deploy for you.
3. Set the PK to your private key for your oracle signer.

_Step 3:_ Run `node morpheus.js` to start your node.

1. You can use morpheus.js if do not wish to deploy using your own framework, if no oracles address is set an oracle will be deployed using the preset bytecode automatically.
2. Host your node on an AWS EC2 free VPS, other VPS or locally. Your node will automatically run, update all feed requests, batch requests and earn fees.

## For Non-Technical Users

### Features

* Deploy your own autonomous oracle infrastructure to ANY EVM network in <60s.
* Support any API endpoint in realtime, allowing requests to be made fully onchain to the oracle fully permissionlessly
* Earn fees from requests for data, only filling requests if profitable
* 1 click deployment and setup. No need for any prereqs just launch out of the box ready to use. No developer experience or technical experience needed.
* Fully custom VRF and proof system with cryptographically secure 256b Hash RanCh VRFs

1. Download the binary

Linux\
[morpheus-linux.zip](https://github.com/ScryProtocol/morpheus/raw/main/morpheus-linux.zip)

MacOS \
[morpheus-mac.zip](https://github.com/ScryProtocol/Morpheus/raw/main/morpheus-macos.zip)

Windows\
[morpheus-windows.zip](https://github.com/ScryProtocol/Morpheus/raw/main/morpheus-windows.zip)

### Usage

Simply download the binaries. Put your private key for your oracle signer in the .env and run the node to deploy.

#### .env

<pre class="language-properties"><code class="lang-properties">RPC=
OOFAddress=
PK=
<strong>OPENAI_API_KEY=
</strong></code></pre>

Set the **RPC** for any EVM network where your contract is deployed (Goerli).&#x20;

[https://chainlist.org/](https://chainlist.org/)

OPTIONAL - Set the **OOFAddress** to the oracle address you deployed. This address represents your independant oracle from your deployed contract/the contract that gets deployed by the scripts for your oracle and node. This is NOT an already deployed contract, it is fully independant and only controlled by you and your node.

Set the **PK** to your private key for your oracle signer.

OPTIONAL - Set the **OPENAI\_API\_KEY** for chatGPT support.

OPTIONAL - Set **WEBHOOK** to a URL based webhook to receive an update when a Jury request for human based requests are made. Can set WEBHOOKMSG to set a custom message in the '**msg**' field. **Content** returns data about the request for a default message with needed info. If you set this to a Discord Webhook, setting WEBHOOKMSG to @123YourDiscordIDhere123 it'll ping you with **content**.\
**Webhook payload**

```javascript
payload = {
          content: content,
          request: request,
          data: data,
          bounty: bounty,
          address: oracleAddress,
          timestamp: timestamp,
          msg:webhookMSG
        };
```

For those using the non dev binaries use your prefered terminal such as cmd on windows. Then go to the binary location and do morpheus.exe or just run it as usual like any other app.

Oracles will automatically be deployed, setup and then run autonomously. Make sure to give a little in tokens for gas like ETH. Oracles will refill based on requests.

## Params OPTIONAL

\-a Used to set the oracle address if already deployed&#x20;

\-r Used to set the RPC for an EVM network&#x20;

\-pk Used to set the PK used for the oracle signer and deployment

Sample

`morpheus -a 0x00f0000000F11a5380Da5A184F0C563B5995fee2 -r https://sepolia.infura.io/v3/6822e4e6edc847829086404ffe6d5b2b -pk 0000000000000000000000000000000000000000000000000000000000`

## Jury - Human Defined Questions

**jury.js**\
The script reads from a file named `requests.txt`. Each line in this file should contain an question and a feed ID, separated by a comma.

When the script runs, it will prompt the user for each request:

1. If the user types `skip`, the script will move to the next request.
2. If the user types `delete`, the script will remove the request from the `requests.txt` file.
3. Otherwise, the script will submit the user's input as the answer to the oracle.

Do not include ',' or '$' or any other non int based data for int based requests. You can submit int based data, strings or HEX.

## Testing

We have included a testing tool to make sure that your oracles are operational and live. Simply use test.js to have the script send a data request using the config to your oracle, using the set RPC and private key. You'll see your node pickup the transaction as long as its running, process and submit.

`node test.js`

**Support**

For support, join the Scry Discord or visit scry.finance to chat and learn more about the project.

Happy deploying!

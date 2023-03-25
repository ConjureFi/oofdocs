# Build Your Own Oracle

Welcome to the Scry deployment guide. This guide will take you through the simple process for oracle deployment to any EVM, with custom signers, and API endpoints and high scale oracle feeds. These oracles are fully self deployable and decentralized with you hosting your own nodes.

As you can see Scry allows for high scale data, giving you access to whatever data you need when you need it.

Scry also pioneers Morpheus a framework that allows developers to request any API from web2, directly onchain for their web3 apps in realtime. This tool enables real-time data requests, any API and data to be used, and can be used on any EVM network. Morpheus has been designed to make it easier for developers to build decentralized applications that can interact with the real world, thereby bridging the gap between web2 and web3.

Morpheus nodes, which are self hostable oracles, are a key component of the Scry ecosystem.

Morpheus nodes earn fees from bounty requests for data. When developers need a feed update, they request the endpoint and attach a fee in the native chain's asset for gas. This allows nodes to be fully autonomous, refueling themselves from their own fees, and create passive revenue.

By enabling anyone to fill requests for data, Scry allows developers and projects to choose where they source their data based on reputation for who they feel will be honest, as well as even source the data from their own community.

To start, make sure to check out our docs. Once ready go to the deployments in the docs to find your factory. If your network is not supported you can fork from the github repo. You can also use createmorph.js just run it in step 3 rather than init.js and itll deploy your oracle aswell

We will be using goerli to demo, but you can use any EVM, just use your preferred deployment framework and once you have your contract address, you can continue. You have 2 options you can either use etherscan to easily initialize the oracle. You can also do it directly using our scripts.

Step 1. To start clone into the repo at github.com/scryprotocol/NodeDeploy. Then npm install. Once ready go to the .env and set up your oracle.

Start with the RPC, you can use any RPC for any EVM network that your contracts been deployed to. Here we are using gorli. OOF Address for the oracle address you deployed. PK would be for your private key for your signer for the oracle. Then go into init.js and change the signers to the address for your private key.

Step 3. Once ready go back to your environment and run node init.js. The script will setup your oracle for you.

Step 4. You're now ready to run your node, run node morpheus dot j s and you're all done, you can simply use the script on an AWS EC2 free vps or you can host it locally. Your node will automatically run and update all feeds for you and earn fees.

If you need support just hop in our discord or come chat if you like what we've built. Find links on scry dot finance. Cheers.

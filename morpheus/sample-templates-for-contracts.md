---
description: For reference only, these are not audited, and notfor production use
---

# Sample Templates for Contracts

### Repo

### [https://github.com/ScryProtocol/Contracts/tree/main/templates](https://github.com/ScryProtocol/Contracts/tree/main/templates)

[#delta](sample-templates-for-contracts.md#delta "mention") - Options

[#deathroll](sample-templates-for-contracts.md#deathroll "mention") - Betting&#x20;

[#hedgeinsure](sample-templates-for-contracts.md#hedgeinsure "mention")-Insurance

## **Delta**

The `Delta` contract is an ERC20 token implementation for put based Hedge financial options. It utilizes arbitrary external data through **Morpheus** integration to update certain contract parameters such as the current price of an underlying asset.

[https://github.com/ScryProtocol/Contracts/blob/main/templates/HedgeDelta.sol](https://github.com/ScryProtocol/Contracts/blob/main/templates/HedgeDelta.sol)

#### Key Features at a Glance:

* **Effortless PUT Creation**: Customize a hedge option on your chosen asset, defining your terms for expiry and collateral.
* **Leverage Support**: Utilize leverage on your PUT, particularly beneficial for assets with low volatility.
* **Flexible Collateral**: While it initially accommodates stablecoins (like USDC), the protocol is built to handle various forms of collateral.
* **Your Choice of Price Source**: Select the API that will source your strike price.
* **Delta Payouts**: Receive the Delta (difference) between the strike and current price at expiry. PUT sellers can redeem collateral, less the Delta, at expiry.
* **Tokenization**: PUTs can be sold on any decentralized exchange (DEX) you prefer, allowing for liquidity.

**Key Components**

1. **ERC20 Implementation**: Inherits standard ERC20 functionalities such as token transfer, balance tracking, and allowance management.
2. **Oracle Integration**:
   * **Morpheus Oracles**: The contract interacts with multiple Oracle addresses (`morpheus`) to request and update price feeds.
   * **Price Update Mechanism**: The `updatePrice` function pulls data from the Oracles to update the `currentPrice` of the underlying asset.
   * **Feed Requests and Updates**: Functions `requestFeed` and `updateFeeds` manage Oracle feed requests and support respectively.
3. **Option Contract Features**:
   * **Initialization**: The `init` function sets up the option contract with parameters like expiry time, strike price, and Oracle details.
   * **Minting Options**: Users can mint options by depositing collateral through the `mint` function.
   * **Option Settlement**: The `redeem` function allows users to settle their options post-expiry, with outcomes depending on the strike price and current price.
4. **Collateral Management**:
   * Users deposit collateral when minting options, which is managed within the contract.
   * Functions like `unlock` and `redeem` handle the collateral based on option performance and expiry.
5. **Event Emissions**:
   * Events like `Initialized`, `Minted`, `Delta`, `Unlocked`, and `Redeemed` are emitted for tracking various contract actions.

**Contract: DeltaFactory**

The `DeltaFactory` contract is responsible for deploying `Delta` contracts. It allows users to create new Delta option contracts with specific parameters.

**Key Features**

1. **Delta Contract Creation**: Users can create new `Delta` contracts with specific option and Oracle configurations.
2. **Storage of Deployed Contracts**: Keeps a record of all deployed `Delta` contracts.
3. **Event Emissions**: Emits `deltaDeployed` event upon the creation of a new Delta contract.

**Oracle Use**

* Fetching real-time price data of underlying assets.
* Ensuring that the price used in contract functions like option settlement is up-to-date and accurate.
* Managing a decentralized and reliable data source, enhancing the contract's trustworthiness and functionality in a decentralized finance (DeFi) context.

## DeathRoll

The `DeathRoll` contract is a unique gaming contract leveraging smart contracts for a competitive betting game. It integrates with Morpheus to provide random numbers for game outcomes.

[https://github.com/ScryProtocol/Contracts/blob/main/templates/pvpDeathRoll.sol](https://github.com/ScryProtocol/Contracts/blob/main/templates/pvpDeathRoll.sol)

**Key Features at a Glance**

1. **Game Creation and Management**: Facilitates the creation and management of betting games between two players.
2. **Betting Mechanism**: Players can place bets in each game, with the smart contract managing the bet amounts and rules.
3. **Oracle Integration for Randomness**: Utilizes Morpheus Oracle for generating verifiable random numbers, essential for determining game outcomes.
4. **Withdrawal System**: Allows players to withdraw their winnings or refunded bets.
5. **Collector Management**: Enables changing the collector address and oracle fee.

**Key Components**

1. **Game Structure**: Each game is a struct containing player addresses, commitments, bet amounts, rolls, and other relevant details.
2. **Oracle Integration**:
   * **Morpheus Oracle**: Interacts with Morpheus Oracle to request random numbers (VRF - Verifiable Random Function).
   * **Request and Retrieve Oracle Data**: Functions to request and retrieve data from the Oracle to ensure fair game outcomes.
3. **Game Flow**:
   * **Game Creation**: Players can create games with specific bet amounts.
   * **Joining Games**: Players join existing games and place their bets.
   * **Determining Winners**: After both players have joined and bet, the contract determines the winner using Oracle-generated random numbers.
4. **Event Emissions**:
   * Events like `GameCreated`, `PlayerJoined`, and `WinnerDetermined` for tracking game creation, player participation, and outcomes.
5. **Withdrawal and Refunds**:
   * Players can withdraw their winnings or unplayed bet amounts.
   * A refund mechanism is in place for situations like unmet game conditions.
6. **Collector and Oracle Fee Management**:
   * The contract allows changing the collector's address and updating the oracle fee.

**Oracle Use**

* **Random Number Generation**: The Morpheus Oracle is used to provide random numbers, ensuring fair and unpredictable game outcomes.
* **Verifiability**: By using an Oracle, the contract ensures that the randomness used in games is verifiable and tamper-proof.

**Additional Considerations**

* **Security and Fair Play**: The contract's design emphasizes fair play by using a verifiable random number from an Oracle.
* **Economic Model**: A fee-collecting mechanism and the ability to adjust oracle fees reflect a dynamic economic model within the game.
* **Customization and Flexibility**: The contract allows for game customization and is adaptable to various player preferences and strategies.

Overall, the `DeathRoll` contract presents an innovative approach to decentralized gaming for a secure, fair, and engaging gaming experience.

## HedgeInsure

The `HedgeInsure` contract is an ERC20 token implementation, specifically designed for creating and managing insurance-like financial products. It integrates with the Morpheus system to dynamically adjust certain contract parameters based on external data feeds, such as market prices or other relevant financial data points.

[https://github.com/ScryProtocol/Contracts/blob/main/templates/HedgeInsure.sol](https://github.com/ScryProtocol/Contracts/blob/main/templates/HedgeInsure.sol)

**Key Features at a Glance**

1. **Customized Insurance Products**: Creation of hedging instruments that can act like insurance policies against market volatility or other conditions like hacks.
2. **Collateralization with ERC20 Tokens**: Uses ERC20 tokens as collateral for the insurance contracts.
3. **Dynamic Pricing with Oracle Integration**: The contract uses Morpheus Oracle for real-time data to adjust pricing and other key contract metrics.
4. **ERC20 Token Standard Compliance**: Adheres to the ERC20 standard for token implementation, ensuring compatibility with a wide range of Ethereum-based services and wallets.

**Key Components**

1. **ERC20 Token Functionality**: Implements standard ERC20 functions such as `transfer`, `balanceOf`, and `allowance`.
2. **Oracle Integration**:
   * **Morpheus Oracles**: Interacts with Morpheus Oracles to request real-time data feeds.
   * **Dynamic Data Update Mechanisms**: Functions like `updatePrice` and `updateFeeds` for managing data updates from Oracles.
3. **Insurance Contract Features**:
   * **Initialization**: Sets up insurance contracts with parameters like expiry, claim value, collateral token, and Oracle details.
   * **Minting and Collateral Management**: Users can mint insurance tokens by depositing collateral.
   * **Claim and Settlement**: Provides functionality for claiming insurance based on predefined conditions and Oracle data.
4. **Event Emissions**:
   * Emits events such as `Initialized`, `Minted`, `Delta`, `Unlocked`, and `Redeemed` for various contract actions.

**InsureHedgeFactory Contract**

This contract facilitates the creation of `HedgeInsure` contracts, allowing users to deploy new insurance product contracts with specific parameters.

**Key Features**

1. **Creation of HedgeInsure Contracts**: Streamlines the process of setting up new insurance products.
2. **Storage of Deployed Contracts**: Maintains a record of all deployed `HedgeInsure` contracts.
3. **Event Emissions**: Emits `deltaDeployed` event upon the creation of a new HedgeInsure contract.

**Oracle Use**

* **Real-time Data for Contract Adjustment**: Ensures that insurance contracts are dynamically adjusted based on real-time market data or other relevant external data points.
* **Enhanced Reliability and Trust**: The integration with Morpheus Oracle enhances the trustworthiness and reliability of the insurance products, providing up-to-date and accurate data for contract settlement.

Overall, the `HedgeInsure` contract introduces a novel approach to creating blockchain-based financial instruments that mimic traditional insurance products, leveraging ERC20 token standards and Oracle integration for a seamless and robust financial experience.


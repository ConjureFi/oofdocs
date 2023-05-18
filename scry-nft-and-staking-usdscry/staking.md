# Staking

## $SCRY Staking Process

The $SCRY token is utilized as a form of collateral within the Morpheus framework. Here's a step-by-step guide on how to stake $SCRY tokens using Etherscan. Please note that you will need to have Metamask or another web3 wallet installed and funded with $SCRY and Ether (for gas fees).

### Step 1: Navigate to the Contract

Go to Etherscan and find the $SCRY Staking Contract address. Once you have the address, input it into the search bar on the Etherscan homepage to pull up the contract.

### Step 2: Connect your Wallet

Click on the "Connect to Web3" button to connect your Metamask or other web3 wallet to Etherscan. This will allow Etherscan to interact with your wallet.

### Step 3: Approve the Contract

Before you can stake $SCRY tokens, you must first give the staking contract permission to move $SCRY tokens on your behalf. To do this, you need to call the `approve` function on the $SCRY token contract.

[https://etherscan.io/token/0x0000000000071821e8033345a7be174647be0706#writeContract#F1](https://etherscan.io/token/0x0000000000071821e8033345a7be174647be0706#writeContract#F1)

* Navigate to the $SCRY token contract page.
* Go to "Contract" -> "Write Contract".
* Connect your wallet if it's not connected yet.
* Find the `approve` function and fill in the following details:
  * `spender`: This will be the address of the Staking Contract.
  * `amount`: This is the amount of $SCRY tokens you want to stake. It needs to be in the smallest unit of the token, called "wei". Use a the + symbol by amount (uint256) and then choose 18 to turn say 1000 tokens to the right wei amount.

Press "Write" and confirm the transaction in your wallet.

### Step 4: Stake your Tokens

Once the approval transaction has been confirmed, you can stake your tokens.

* Go back to the Staking Contract on Etherscan.
* Navigate to "Contract" -> "Write Contract".
* Connect your wallet if it's not connected yet.
* Find the `stakeScry` function and fill in the following details:
  * `oracle`: This will be the address of the oracle you want to support.
  * `amount`: The amount of $SCRY tokens (in wei) you want to stake.

Press "Write" and confirm the transaction in your wallet.

Congratulations! You have successfully staked $SCRY tokens. You can verify this by going to "Contract" -> "Read Contract", then calling the `stake` function with the oracle's address. This will show the total amount of $SCRY tokens staked for that oracle, including your stake.

Remember that the contract also includes functionality to `unstake` and `withdraw` your tokens. Be sure to familiarize yourself with these functions and their restrictions before staking. Happy staking!

## $SCRY Token Withdrawal

The process of withdrawing your staked $SCRY tokens involves invoking certain functions of the smart contract. Here is a step-by-step guide on how to withdraw your $SCRY tokens using Etherscan.

### Step 1: Initiate Withdrawal

Before you can withdraw your tokens, you need to initiate a withdrawal. This begins a 7-day unlock period before you can actually withdraw your tokens. This is a security measure designed to prevent oracles from attacking and withdrawing their assets before they can be slashed.

* Navigate to the $SCRY Staking Contract on Etherscan.
* Go to "Contract" -> "Write Contract".
* Connect your wallet if it's not already connected.
* Find the `withdraw` function and fill in the following details:
  * `oracle`: This will be the address of the oracle from which you want to withdraw your staked tokens.
  * `amt`: The amount of $SCRY tokens (in wei) you want to withdraw.

Press "Write" and confirm the transaction in your wallet.

Once this transaction is confirmed, a 7-day unlock period begins. You will have to wait until this period ends before you can complete the withdrawal.

### Step 2: Complete the Withdrawal

Once the 7-day unlock period is over, you can complete the withdrawal process:

* Go back to the $SCRY Staking Contract on Etherscan.
* Navigate to "Contract" -> "Write Contract".
* Connect your wallet if it's not connected yet.
* Find the `unstake` function and fill in the following details:
  * `oracle`: This should be the same oracle address you used in step 1.
  * `amt`: This should be the same amount you specified in step 1.

Press "Write" and confirm the transaction in your wallet.

Congratulations! You have successfully withdrawn your staked $SCRY tokens. These tokens should now be back in your wallet.

Keep in mind that if the oracle was slashed during the 7-day unlock period, you may receive fewer tokens back than you originally staked. This is because a portion of the staked tokens are burned when an oracle is slashed.

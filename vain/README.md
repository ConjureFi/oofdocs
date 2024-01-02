# Vain

An open marketplace for mining vanity ETH addresses! Fully secure using ECC offsets of public keys to allow for miners to mine vanity addresses without access to the vanity address private key! Supports user wallet EOAs, contracts and create2 for leading 0s and custom vanity like 0x1337b33f!

### Contract

0x000000000001F04A9533e92d7AD4dDe7DC19a8F3

### Create a bounty

1. Approve Scry for Vain&#x20;

https://optimistic.etherscan.io/address/0x64ba55A341EC586A4C5d58d6297CdE5125aB55bC#writeContract&#x20;

Approve 0x000000000001F04A9533e92d7AD4dDe7DC19a8F3 \
for your desired bounty amount

You Can Get Scry On Optimism Here [https://app.uniswap.org/swap?inputCurrency=ETH\&outputCurrency=0x64ba55A341EC586A4C5d58d6297CdE5125aB55bC](https://app.uniswap.org/swap?inputCurrency=ETH\&outputCurrency=0x64ba55A341EC586A4C5d58d6297CdE5125aB55bC)

Go to [https://optimistic.etherscan.io/address/0x000000000001F04A9533e92d7AD4dDe7DC19a8F3#readContract](https://optimistic.etherscan.io/address/0x000000000001F04A9533e92d7AD4dDe7DC19a8F3#readContract)

2. Use createBounty&#x20;

**createBounty**&#x20;

Set to 0

**pubkey** (bytes)&#x20;

Your pubkey goes here. Check https://github.com/pr0toshi/profanity2/ for how to get your public key.

**custom** (bytes)&#x20;

For addresses that arent simply for n leading 0s, ie, 0x1337----. custom=0x1337

**n** (uint8)&#x20;

For n leading 0s, ie, 0x00000000, n=8

**flag** (uint8)&#x20;

0 user wallet EOA with n leading 0s&#x20;

1 contract with n leading 0s&#x20;

2 create2 contract with n leading 0s&#x20;

3 user wallet EOA custom&#x20;

4 contract custom

5 create2 contract custom

**locked** (uint8) Set to 0

**amount** (uint256) How much SCRY for the miner as a bounty.

**Notes**&#x20;

Recommended Bounty&#x20;

250 SCRY <10 characters&#x20;

2500 SCRY 11-12 characters

3. Once one has been found you can add your private key to the offset to get your new vanity addresses private key securely!&#x20;

## **How to**

### Open Source tool for getting public keys and adding private keys (Do not use for wallets with large value)

[https://codepen.io/Pro-Pro-the-scripter/pen/zYyyNbJ](https://codepen.io/Pro-Pro-the-scripter/pen/zYyyNbJ)

### Getting public key for mandatory `-z` parameter

Generate private key and public key via openssl in terminal (remove prefix "04" from public key):

```
$ openssl ecparam -genkey -name secp256k1 -text -noout -outform DER | xxd -p -c 1000 | sed 's/41534e31204f49443a20736563703235366b310a30740201010420/Private Key: /' | sed 's/a00706052b8104000aa144034200/\'$'\nPublic Key: /'
```

Derive public key from existing private key via openssl in terminal (remove prefix "04" from public key):

```
$ openssl ec -inform DER -text -noout -in <(cat <(echo -n "302e0201010420") <(echo -n "PRIVATE_KEY_HEX") <(echo -n "a00706052b8104000a") | xxd -r -p) 2>/dev/null | tail -6 | head -5 | sed 's/[ :]//g' | tr -d '\n' && echo
```

### Adding private keys (never use online calculators!)

#### Terminal:

Use private keys as 64-symbol hexadecimal string WITHOUT `0x` prefix:

```
(echo 'ibase=16;obase=10' && (echo '(PRIVATE_KEY_A + PRIVATE_KEY_B) % FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEFFFFFC2F' | tr '[:lower:]' '[:upper:]')) | bc
```

#### Python

Use private keys as 64-symbol hexadecimal string WITH `0x` prefix:

```
$ python3
>>> hex((PRIVATE_KEY_A + PRIVATE_KEY_B) % 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEFFFFFC2F)
```

### Get Scry on Optimism&#x20;

### https://app.uniswap.org/swap?inputCurrency=ETH\&outputCurrency=0x64ba55A341EC586A4C5d58d6297CdE5125aB55bC

### For info&#x20;

https://github.com/pr0toshi/profanity2/&#x20;

Chat&#x20;

https://discord.gg/49qDUcPy6t

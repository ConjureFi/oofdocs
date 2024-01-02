# Deep dive

A dive into secure vanity address generation using ECC offsets for mining and creating new public keys without exposing the original private key.

### TLDR

k: the original private key&#x20;

o: an offset&#x20;

G is the generator point

\+ denotes elliptic curve addition&#x20;

× denotes elliptic curve multiplicatio

Given a public key PubK, miners seek: PublicKeyoffset=o×G PublicKeyTarget=PubK+PublicKeyoffset address(PublicKeyTarget) starts with 0x00000000

Getting private key of PublicKeyTarget as the user Miner reveals o for bounty Verify PublicKeyTarget=PubK+(o×G) address(PublicKeyTarget) starts with 0x00000000 ​privateKeyTarget=(k+o)×G

But why? ​(k+o)×G=(k×G)+(o×G) And so (k+o)×G=PubK+PublicKeyoffset

No need to reveal k, only PubK, and o can be verified publicly to generate desired vanity and onchain for a bounty while being fully secure!

### Wut

Generating New Public Keys with ECC Offsets In ECC, every private key has a corresponding public key. This is achieved through a deterministic process that involves multiplying the private key by the generator point G of the elliptic curve. Public Key=Private Key×G

Now, suppose we introduce an "offset" into this equation. An offset can be another private key that is used to create an alternative public key without revealing any details about the original private key.

Given an original private key k and an offset o, you could generate a new public key like so:

PubK = New Public Key=(k+o)×G

The interesting property of ECC is that even though a new public key is generated, it is computationally infeasible to derive k or o from the new public key due to the hardness of the elliptic curve discrete logarithm problem (ECDLP).

Miners could be provided with a public key and tasked with finding an offset that satisfies a certain condition when used to generate a new public key. For example, they might need to find an offset that, when added to the original private key and multiplied by G, produces a new public key with a specific property (e.g., starting with a certain number of leading zeros).

This is possible as 2 public keys can be added for a new public key&#x20;

So given PubK they look for&#x20;

PublicKeyoffset=o×G&#x20;

PublicKeyTarget =PublicKeyoffset+PubK&#x20;

Check target PublicKey for address with n. If meets reqs publish o, then user can do k+o for private key of PublicKeyTarget

The original private key k is never revealed to the miners. They only work with the public key and try to find an offset o that satisfies the condition set by the protocol. Once a miner finds such an offset, they can present the new offset private key as a proof of their work and user can do k+o for private key of their new vanity address securely!

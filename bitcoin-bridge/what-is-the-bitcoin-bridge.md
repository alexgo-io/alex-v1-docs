---
description: Bitcoin Bridge is not a typical cross-chain bridge
---

# What is the Bitcoin Bridge

## Key to providing the "native-like" Bitcoin DeFi experience

Bitcoin Bridge is a key component of ALEX that abstracts the difference between L1 and L2 from the user experience, i.e. providing the "native-like” Bitcoin DeFi experience on L1, whereby users can use native BTC or L1 assets issued on Bitcoin to interact with L2 smart contracts.

Bitcoin Bridge is bi-directional or “two-way” bridge, meaning you can freely transfer assets between Bitcoin and its L2s and vice versa.

On Bitcoin, users interact with [Multisigs](what-is-the-bitcoin-bridge.md#multisigs) (currently operated by ALEX LAB Foundation, but to be upgraded to TSS-based ones soon) to lock assets to be bridged ("source asset"), and on L2s to receive the L2 asset ("destination asset").

Additionally, users on Bitcoin may provide additional data (`OP_RETURN`) to trigger certain smart contract interaction on their behalf automatically by Bitcoin Bridge.

On L2s or non-Bitcoin chains, users interact with "[Endpoints](what-is-the-bitcoin-bridge.md#endpoints)" on the source blockchain to lock assets to be bridged ("source asset"), and on the destination blockchain to receive the bridged asset ("destination asset").

Asset transfers from users to Endpoints are monitored by a group of "[validators](what-is-the-bitcoin-bridge.md#validators)", who produce cryptographic proofs that say "X amount of source asset are sent by address A on the source blockchain for address B on the destination blockchain to receive Y amount of destination asset."

There is a minimum threshold of such proofs that must be provided and verified before the assets received into Endpoint can be sent to the relevant address.

Bitcoin Bridge is being integrated with [Bitcoin Oracle](broken-reference) which provides the [necessary consensus infrastructure](../bitcoin-oracle/threshold-based-consensus/).

Upon meeting such minimum threshold, a "[relayer](what-is-the-bitcoin-bridge.md#relayers)" calls into Endpoint with the proofs to trigger the transfer of the received destination assets to the relevant address.

## Multisigs

Multisigs are Bitcoin wallets that are operated by multiple signers. In contrast to a typicall wallet requiring just one party to sign a transaction, a multisig requires multiple parties or signers to sign a transaction.

ALEX partnered with [Asigna](https://asigna.gitbook.io/asigna/introduction/about-asigna) who is "_a native multisig solution tailored for Bitcoin and Stacks made specifically to offer an extra layer of security to trusted wallets on both ecosystems._"

## Endpoints

Endpoints are the smart contracts that handle the asset transfers. They are owned by multisig contracts (for example, [Gnosis Safe](https://safe.global/) on Ethereum and [Executor DAO](https://explorer.stacks.co/txid/0xf4bd95ea0486e6a50ae632c613f1d72b2a5bbbc4211b494cd0f1d3443658544d?chain=mainnet) on Stacks) operated by ALEX LAB Foundation.

Users use Endpoints to trigger transfer of source assets. The destination assets are then sent by a relayer by producing cryptographic proofs.

That the assets are held by contracts owned by multisig contracts is important because this minimises the risk of a malicious actor stealing the private key and assets sent by users.

## Validators

Validators are responsible for producing cryptographic proofs, which must be verified by the Endpoints before transferring the destination assets to an address.&#x20;

[Bitcoin Oracle](broken-reference) runs the validator network for and secures the Bitcoin Bridge.

Validators are important because this set-up minimises the risk of a malicious actor taking over the system (for example, a relayer).

## Relayers

Relayers are responsible for triggering the destination asset transfer upon gathering a minimum threshold of cryptographic proofs produced by the validators.

Relayers, however, cannot move the assets at will and their role is limited only to delivering messages from/to multisigs and Bridge endpoint.


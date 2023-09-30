# Understanding the Bitcoin Oracle

<figure><img src="../.gitbook/assets/Screenshot 2023-09-30 at 10.52.53 PM.png" alt=""><figcaption></figcaption></figure>

## Temper-proof, censorshop-resistant, indexing

Bitcoin Oracle aims to provide a temper-proof, censorship-resistant, indexing of events of meta-protocols like BRC20, with Bitcoin chain as its ultimate source of truth, removing the needs to rely on a single, centralised, off-chain, indexer.

## Founded on the partnership

Bitcoin Oracle does not implement an on-chain version of "indexing engine" itself, which, while theoretically possible, is computationally very expensive, but rather runs a federated model that relies on a consortium of off-chain indexers to validate and submit the latest meta-protocol transactions to the on-chain smart contract written by ALEX, which keeps the validators in check by (1) implementing a consensus mechanism and (2) independently verifying that the purported Bitcoin tx is indeed mined (which only Bitcoin L2, like Stacks, can do).

The consensus is reached when a minimum threshold (i.e. "_m-of-n_") of pre-approved validators agree to a particular event.

## Combining the best of off-chain and on-chain

This combination of off-chain computation and on-chain verification allows the Bitcoin Oracle to scale without compromising its security as the governance around a particular meta-protocol evolves.

For example, the structure means disagreements over the state are easily resolved so long as they concern within the existing rules of the meta-protocol concerned, because Bitcoin network acts as the universal source of truth and all indexers do are indexing that truth according to the rules.

Disagreements stemming from those that are outside the current rules require update of the rules, but does not require an upgrade of the on-chain indexer, vastly simplifying the process.

## Open to all to participate

The Indexer is actively developed by ALEX and is released under MIT license. We encourage all those interested to check out the below and help contribute!

#### GitHub&#x20;

[https://github.com/alexgo-io/bitcoin-oracle](https://github.com/alexgo-io/bitcoin-oracle)

#### Latest deployment (developer preview)

[https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.indexer-dev-preview-6?chain=mainnet](https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.indexer-dev-preview-6?chain=mainnet)


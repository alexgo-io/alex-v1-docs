# Why Bitcoin Oracle - BRC20

## Bitcoin does not understand you

The DeFi landscape is teeming with possibilities, and at the heart of it all, Bitcoin DeFi is set to take center stage. In the coming years, we expect a surge in growth revolving around Ordinals, BRC20 tokens, Bitcoin L2 in particular Stacks.

A quick retrospect shows how Ordinals have brilliantly showcased the potential of the Bitcoin blockchain. This revolutionary platform has turned the table, illustrating that Bitcoin’s ledger is more than just a settlement layer.

The unique character of BRC20 tokens, built on a fundamentally non-fungible blockchain, sparks intrigue among many. Even though Bitcoin’s blockchain doesn’t enforce the token standard rules, and the token operation is often slow and inefficient, the allure of Bitcoin being more than just a payment system is irresistible.

In order to enforce the token standard rules, a number of "off-chain" indexers has, therefore, been developed and are in use to address these constraints. However, the risk of tampering with a centralized off-chain indexer cannot be ignored, and many in the field, including us, consider the establishment of an on-chain, tamper-proof, and censorship-resistant indexer to be pivotal for wider BRC20 adoption.

To that end, we’re collaborating with pioneers like Domo, the creator of the BRC20 standard, and the major off-chain indexers like \[BIS], OKX, Hiro and UniSat to construct an "indexer of indexers." This concept capitalizes on Stacks’ unique attributes as Bitcoin’s L2, leading the charge for decentralized consensus on BRC20 indexing.

## Temper-proof, censorshop-resistant, indexing

Bitcoin Oracle aims to provide a temper-proof, censorship-resistant, indexing of events of meta-protocols like BRC20, with Bitcoin chain as its ultimate source of truth, removing the needs to rely on a single, centralised, off-chain, indexer.

<figure><img src="../../.gitbook/assets/Screenshot 2023-09-30 at 10.52.53 PM.png" alt=""><figcaption></figcaption></figure>



## Founded on the partnership

Bitcoin Oracle does not implement an on-chain version of "indexing engine" itself, which, while theoretically possible, is computationally very expensive, but rather runs a federated model that relies on a consortium of off-chain indexers to validate and submit the latest meta-protocol transactions to the on-chain smart contract written by ALEX, which keeps the validators in check by (1) implementing a consensus mechanism and (2) independently verifying that the purported Bitcoin tx is indeed mined (which only Bitcoin L2, like Stacks, can do).

The consensus is reached when a minimum threshold (i.e. "_m-of-n_") of pre-approved validators agree to a particular event.

## Combining the best of off-chain and on-chain

This combination of off-chain computation and on-chain verification allows the Bitcoin Oracle to scale without compromising its security as the governance around a particular meta-protocol evolves.

For example, the structure means disagreements over the state are easily resolved so long as they concern within the existing rules of the meta-protocol concerned, because Bitcoin network acts as the universal source of truth and all indexers do are indexing that truth according to the rules.

Disagreements stemming from those that are outside the current rules require update of the rules, but does not require an upgrade of the on-chain indexer, vastly simplifying the process.

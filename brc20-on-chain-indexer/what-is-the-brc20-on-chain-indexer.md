# What is the BRC20 On-chain Indexer

<figure><img src="../.gitbook/assets/Screenshot 2023-08-18 at 5.35.12 PM.png" alt=""><figcaption></figcaption></figure>

The DeFi landscape is teeming with possibilities, and at the heart of it all, Bitcoin DeFi is set to take center stage. In the coming years, we expect a surge in growth revolving around Ordinals, BRC20 tokens, Bitcoin L2 in particular Stacks.

A quick retrospect shows how Ordinals have brilliantly showcased the potential of the Bitcoin blockchain. This revolutionary platform has turned the table, illustrating that Bitcoin’s ledger is more than just a settlement layer.

The unique character of BRC20 tokens, built on a fundamentally non-fungible blockchain, sparks intrigue among many. Even though Bitcoin’s blockchain doesn’t enforce the token standard rules, and the token operation is often slow and inefficient, the allure of Bitcoin being more than just a payment system is irresistible.

In order to enforce the token standard rules, a number of "off-chain" indexers has, therefore, been developed and are in use to address these constraints. However, the risk of tampering with a centralized off-chain indexer cannot be ignored, and many in the field, including us, consider the establishment of an on-chain, tamper-proof, and censorship-resistant indexer to be pivotal for wider BRC20 adoption.&#x20;

To that end, we’re collaborating with pioneers like Domo, the creator of the BRC20 standard, as well as the major off-chain indexers like BestinSlot.xyz, OKX and, soon, Hiro, to construct an "indexer of indexers."  This concept capitalizes on Stacks’ unique attributes as Bitcoin’s L2, leading the charge for decentralized consensus on BRC20 indexing.&#x20;

The setup involves off-chain indexers submitting and validating events to the on-chain indexer, which then verifies each transaction through Stacks, either accepting or rejecting them. Wallets and decentralized applications can query this on-chain indexer for valid events, marking an exciting step in our mission to transform the face of Bitcoin DeFi.

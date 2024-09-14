# What is the Bitcoin Oracle

## Cross-chain messaging and consensus layer for all and any off-chain computation engines

Bitcoin Oracle is a cross-chain messaging and consensus layer for all and any off-chain computation engines that builds on Bitcoin.

Bitcoin Oracle provides infrastructure that&#x20;

* allows two or more off-chain computation engines on Bitcoin to exchange messages, and
* provides the consensus model to validate such cross-chain messages.

The consensus is deemed to have reached when a minimum threshold (i.e. "_m-of-n_") of the relevant community agree to a particular event.

End consumers can then verify the consensus before making a decision or taking an action with respect to that particular event, thus enhancing the security assumptions.

For example, Bitcoin Oracle secures [Xlink](what-is-the-bitcoin-oracle.md#example-xlink). When Xlink identifies a particular cross-chain transfer to process, it pulls the relevant consensus data from Bitcoin Oracle, verify them using its smart contract before processing them on the relevant destination chain.

## Flexible threshold-based consensus model

Using the "on-demand" consensus data, dapps and other end consumers of such data can reach the consensus based on a minimum threshold (i.e. "_m-of-n_") of the relevant community agree to a particular event.

Bitcoin Oracle provides a consensus model framework that allows the end consumer to customise their consensus model by optimising across the trust and the security budget.

Each end consumer may specify a number of required (i.e. trusted) and optional (i.e. non-trusted) validators as its consensus model.

For example, for each event to validate, the end consumer may specify that the required validators must agree and a certain threshold (say 51%) of all validators (including required and optional) must agree.

A fast derivation of consensus is achieved through [Threshold Sampling](what-is-the-bitcoin-oracle.md#threshold-sampling) among nodes.

Some may choose to have only required validators, in which case, effectively, a federated concensus model is run. In this case, a trust element is introduced to eliminate the security budget constraint.&#x20;

Some others may choose to have only optional validators, but with staking/slashing mechanism. In this case, the security budget enabled by the staking/slashing mechanism means the consensus model can be run trustlessly.

### Threshold Sampling

Threshold sampling allows Bitcoin Oracle to derive consensus from the network of nodes without having to download all the consensus data from its data layer. Bitcoin Oracle engages in several rounds of random sampling for subset of the network and, with each successful round, consensus confidence grows. When Bitcoin Oracle achieves a set confidence threshold, an event is deemed to have reached consensus, subject to validation by all trusted nodes.

## Example - XLink

[XLink](https://app.xlink.network) is a key component of any projects building on Bitcoin that abstracts the difference between L1 and L2 from the user experience, i.e. providing the "native-like” Bitcoin DeFi experience on L1, whereby users can use native BTC or L1 assets issued on Bitcoin to interact with L2 smart contracts.

XLink is bi-directional or “two-way” bridge, meaning you can freely transfer assets between Bitcoin and its L2s and vice versa.

XLink incorporates a mixture of required and optional validators to secure its infrastructure, where the consensus (100% threshold) of the required validators must be met to validate the consensus (51% threshold) reached by all the validators.&#x20;

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>Bitcoin Oracle secures XLink</p></figcaption></figure>

This model brings the following benefits:

* It lowers the barrier to entry to join the validator network, encouraging more members of its community to participate in securing the XLink infrastructure.
* Security budget is replaced with a set of trusted validators, which essentially acts as the secondary check against the consensus derived from the network.
* Larger part of the incentives can be allocated to encouraging the community to join and actively participate in securing the network.
* Smaller part of the incentives needs to be alloacted to the "insurance" fund.
* 51% threshold and the lower barrier to entry to join the network means it is expensive for the required validators to take over the validator network.
* Likewise, malicious actors cannot take over the validator network even if they own all optional validators (which is very unlikely) unless they also take over every required validator.


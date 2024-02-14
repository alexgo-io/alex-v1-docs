# Threshold-based Consensus

## Consensus model tailor made for each consumer

Using the "on-demand" consensus data, dapps and other end consumers of such data can reach the consensus based on a minimum threshold (i.e. "_m-of-n_") of the relevant community agree to a particular event.

Bitcoin Oracle provides a consensus model framework that allows the end consumer to customise their consensus model by optimising across the trust and the security budget.

Each end consumer may specify a number of required (i.e. trusted) and optional (i.e. untrusted) validators as its consensus model.

For each event to validate, all required validators must agree **and** a certain threshold (say 51%) of all validators (including required and optional) must agree.

Some may choose to have only required validators, in which case, effectively, a federated concensus model is run. In this case, a trust element is introduced to eliminate the security budget constraint.&#x20;

Some others may choose to have only optional validators, but with staking/slashing mechanism. In this case, the security budget enabled by the staking/slashing mechanism means the consensus model can be run trustlessly.

Bitcoin Bridge, for example, will be incorporating a mixture of required and optional validators (with 51% threshold) to secure its BRC20 bridge, which brings the following benefits:

* It lowers the barrier to entry to join the validator network, encouraging more members of its community to participate in securing the bridge.
* Security budget is replaced with a set of trusted validators, which essentially acts as the secondary check against the consensus derived from the network.
* Larger part of the incentives can be allocated to encouraging the community to join and actively participate in securing the bridge.
* Smaller part of the incentives needs to be alloacted to the "insurance" fund.
* 51% threshold and the lower barrier to entry to join the network means it is expensive for the required validators to take over the validator network.
* Likewise, malicious actors cannot take over the validator network even if they own all optional validators (which is very unlikely) unless they also take over every required validator.


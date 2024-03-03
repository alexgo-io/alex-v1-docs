# Integrations

Integration requires a deployment of an endpoint smart contract on the target chain.

For EVM-based chains, such endpoints are currently deployed on [Ethereum](https://etherscan.io/address/0xfd9f795b4c15183bdba83da08da02d5f9536748f) and [BNB Chain](https://bscscan.com/address/0xb3955302e58fffdf2da247e999cd9755f652b13b).

Below is a typical integration process, which may take between 1 and 3 weeks.

## Endpoint deployment (1 week)

Endpoints are the smart contracts that handle the asset transfers. They are owned by multisig contracts (for example, [Gnosis Safe](https://safe.global/) on Ethereum and [Executor DAO](https://explorer.stacks.co/txid/0xf4bd95ea0486e6a50ae632c613f1d72b2a5bbbc4211b494cd0f1d3443658544d?chain=mainnet) on Stacks) operated by ALEX LAB Foundation (with a plan to decentralisation).

Users use Endpoints to trigger transfer of source assets. The destination assets are then sent by a relayer by producing cryptographic proofs.

## Configuration of endpoint (< 1 week)

Once the endpoint is deployed, it can be configured to meet the needs of the source chain.

The configuration parameters include, among others,:

* Approved list of tokens to be supported on the Bridge,
* Approved list of validators whose cryptographic proofs of token transfer event are accepted,
* Approved list of relayers who can submit the cryptographic proofs from the validators,
* Validator threshold, and
* Fee schedule

## Integration with Bitcoin Oracle (> 1 week)

Bitcoin Bridge scales by partnering with [Bitcoin Oracle](broken-reference) which runs the validation network.&#x20;

Bitcoin Oracle observes every endpoint on the Bitcoin Bridge and produces a set of cryptographic proofs for the relevant destination chain to process.

Bitcoin Oracle produces the [consensus data](../../bitcoin-oracle/on-demand-consensus-data.md) based on the computation from the off-chain engines and provides a [consensus model framework](../../bitcoin-oracle/threshold-based-consensus/) that allows the end consumer to customise their consensus model by optimising across the trust and the security budget.

Bitcoin Bridge uses a mixture of required and optional validators (with 51% threshold) to secure its bridge, which brings the following benefits:

* It lowers the barrier to entry to join the validator network, encouraging more members of its community to participate in securing the bridge.
* Security budget is replaced with a set of trusted validators, which essentially acts as the secondary check against the consensus derived from the network.
* Larger part of the incentives can be allocated to encouraging the community to join and actively participate in securing the bridge.
* Smaller part of the incentives needs to be alloacted to the "insurance" fund.
* 51% threshold and the lower barrier to entry to join the network means it is expensive for the required validators to take over the validator network.
* Likewise, malicious actors cannot take over the validator network even if they own all optional validators (which is very unlikely) unless they also take over every required validator.

## Wrapped asset deployment (optional)

Where required, an wrapped asset may be deployed on the destination chains to allow the bridging of the asset from its source chain to the destimation chains.

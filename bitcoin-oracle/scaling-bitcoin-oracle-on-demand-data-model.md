# Scaling Bitcoin Oracle - "on demand" data model



## Consensus layer for all and any off-chain computation engines

Bitcoin Oracle acts as a consensus layer for all and any off-chain computation engines that relies on the Bitcoin data.

The consensus is reached when a minimum threshold (i.e. "_m-of-n_") of the relevant community agree to a particular event.

End consumers can then verify the consensus before making a decision or taking an action with respect to that particular event, thus enhancing the security assumptions.

For example, Bitcoin Oracle is used by Xlink for their [BRC20 Bridge](https://app.xlink.network/bridge/brc20-bridge/peg-in), which uses a [smart contract on Stacks](https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.btc-bridge-endpoint-v1-10?chain=mainnet) to verify the consensus and save the validated data on-chain.

When Xlink identifies a particular BRC20 transfer to process, it pulls the relevant consensus data from Bitcoin Oracle, verify them using its smart contract before saving them on-chain.

This combination of the off-chain computation and the on-chain verification protects Xlink users from a potential security issues associated with BRC20, including that of [double spend](https://en.wikipedia.org/wiki/Double-spending).

## Enc consumer of consensus data dictates how it is verified

An important point to note is that, while Bitcoin Oracle produces the consensus data based on the computation from the off-chain indexers, the verification of such consensus data can be implemented by the end consumer as they see fit.

For example, wallet integrating Bitcoin Oracle may implement a client-side verification of the consensus data, instead of relying on an on-chain verification.

A website that wants to display users' BRC20 balances, but do not need to verify the consensus data, may simply use the data without verification.

A BRC20 trading dapp on an EVM-compatible Bitcoin L2 may implement their own smart contract written in Solidity to verify the consensus data, before initiating other smart contract interactions.

## So how can one fetch the consensus data from Bitcoin Oracle?

### Summary data

If you need only the summary data, without its associated consensus data:

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/bitcoin-oracle/user-balance" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/bitcoin-oracle/bitcoin-tx-indexed" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

### Full consensus data

If you need the full consensus data:

{% swagger src="https://api.bitcoin-oracle.network/swagger-ui-yaml" path="/v1/brc20" method="post" %}
[https://api.bitcoin-oracle.network/swagger-ui-yaml](https://api.bitcoin-oracle.network/swagger-ui-yaml)
{% endswagger %}

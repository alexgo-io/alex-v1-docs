# What is the Bitcoin Oracle

## Consensus layer for all and any off-chain computation engines

Bitcoin Oracle acts as a consensus layer for all and any off-chain computation engines that relies on the Bitcoin data.

The consensus is reached when a minimum threshold (i.e. "_m-of-n_") of the relevant community agree to a particular event.

End consumers can then verify the consensus before making a decision or taking an action with respect to that particular event, thus enhancing the security assumptions.

For example, Bitcoin Oracle is used by Xlink for their [BRC20 Bridge](https://app.xlink.network/bridge/brc20-bridge/peg-in), which uses a [smart contract on Stacks](https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.btc-bridge-endpoint-v1-10?chain=mainnet) to verify the consensus and save the validated data on-chain.

When Xlink identifies a particular BRC20 transfer to process, it pulls the relevant consensus data from Bitcoin Oracle, verify them using its smart contract before saving them on-chain.

This combination of the off-chain computation and the on-chain verification protects Xlink users from a potential security issues associated with BRC20, including that of [double spend](https://en.wikipedia.org/wiki/Double-spending).

## Open to all to participate

The Bitcoin Oracle is actively developed by ALEX and is released under MIT license. We encourage all those interested to check out the below and help contribute!

{% embed url="https://github.com/alexgo-io/bitcoin-oracle" %}

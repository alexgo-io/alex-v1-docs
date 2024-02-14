# Bitcoin L2s

On L2s or non-Bitcoin chains, users interact with "Endpoints" on the source blockchain to lock assets to be bridged ("source asset"), and on the destination blockchain to receive the bridged asset ("destination asset").

Endpoints are the smart contracts that handle the asset transfers. They are owned by multisig contracts (for example, [Gnosis Safe](https://safe.global/) on Ethereum and [Executor DAO](https://explorer.stacks.co/txid/0xf4bd95ea0486e6a50ae632c613f1d72b2a5bbbc4211b494cd0f1d3443658544d?chain=mainnet) on Stacks) operated by ALEX LAB Foundation.

Users use Endpoints to trigger transfer of source assets. The destination assets are then sent by a relayer by producing cryptographic proofs.

That the assets are held by contracts owned by multisig contracts is important because this minimises the risk of a malicious actor stealing the private key and assets sent by users.

## Endpoints

<table><thead><tr><th width="181">Chain</th><th>Contract address</th></tr></thead><tbody><tr><td>Stacks (BRC20)</td><td><a href="https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.brc20-bridge-endpoint-v1-07?chain=mainnet">https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.brc20-bridge-endpoint-v1-07?chain=mainnet</a></td></tr><tr><td>Stacks (BTC)</td><td><a href="https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.btc-bridge-endpoint-v1-11?chain=mainnet">https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.btc-bridge-endpoint-v1-11?chain=mainnet</a></td></tr></tbody></table>


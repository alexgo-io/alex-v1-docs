# Understanding the Bitcoin Bridge

## Custody of assets and monitoring of reserves

Bitcoin Bridge is custodial in that BTC and BRC20 tokens being bridged are held in a Multisig wallet on Bitcoin. Users can monitor the reserves real-time at [https://app.alexlab.co/bridge-reserve](https://app.alexlab.co/bridge-reserve).

### BTC Multisig

{% embed url="https://mempool.space/address/bc1pylrcm2ym9spaszyrwzhhzc2qf8c3xq65jgmd8udqtd5q73a2fulsztxqyy" %}

### BRC20 Multisig

{% embed url="https://mempool.space/address/bc1p06r44ervnukj3kxnqt863sz9hly5m7f80k7l94aplnd6z2tnrzvstdkzsq" %}

### Latest circulating supply of aBTC

You can check the latest circulating supply of aBTC or other L2 BRC20 tokens by calling `get-total-supply` function of the relevant contract (please note the number is in 8-digit fixed notation, i.e. the last 8 digits represent the decimals).&#x20;

Alternatively the same is also available at, for example, for aBTC

{% embed url="https://api.alexlab.co/v1/stats/total_supply/token-abtc" %}

## Design overview

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Bitcoin Bridge also is bi-directional or “two-way” bridge, meaning you can freely transfer assets between Bitcoin and Stacks and vice versa.

On Bitcoin, users interact with Multisigs (operated by ALEX LAB Foundation) to lock assets to be bridged ("source asset"), and on Stacks to receive the L2 asset ("destination asset").

On Stacks, users interact with Endpoints to burn the L2 asset ("source asset") and on Bitcoin to receive the L1 assets ("destination asset").

Asset transfers from users to [Multisigs](understanding-the-bitcoin-bridge.md#custody-of-assets-and-monitoring-of-reserves) and [Endpoints](understanding-the-bitcoin-bridge.md#endpoints) are monitored by a group of "[relayers](understanding-the-bitcoin-bridge.md#relayers)", who calls into Multisigs and Endpoints to trigger the transfer of the received destination assets to the relevant address.

Additionally, users on Bitcoin may provide additional data (`OP_RETURN`) to trigger certain smart contract interaction on their behalf automatically by Bitcoin Bridge.

### Endpoints

Endpoints are the smart contracts that handle the asset transfers. They are owned by multisig contracts ([Executor DAO](https://explorer.stacks.co/txid/0xf4bd95ea0486e6a50ae632c613f1d72b2a5bbbc4211b494cd0f1d3443658544d?chain=mainnet) on Stacks) operated by ALEX LAB Foundation.

That the assets are held by contracts owned by multisig contracts is important because this minimises the risk of a malicious actor stealing the private key and assets sent by users.

CoinFabrik audited the Endpoints.

* Bridge Endpoints: [https://cdn.alexlab.co/pdf/ALEX\_Audit\_202310\_Bitcoin\_Oracle\_and\_Bridge.pdf](https://cdn.alexlab.co/pdf/ALEX\_Audit\_202310\_Bitcoin\_Oracle\_and\_Bridge.pdf)

### Relayers

Relayers are responsible for triggering the destination asset transfer process.


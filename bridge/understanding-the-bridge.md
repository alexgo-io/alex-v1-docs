# Understanding the Bridge

## Custory of assets and monitoring of reserves

ALEX Bridge is custodial in that the tokens being bridged are held in contracts on Ethereum. Users can monitor the reserves real-time at [https://etherscan.io/address/0xfd9f795b4c15183bdba83da08da02d5f9536748f](https://etherscan.io/address/0xfd9f795b4c15183bdba83da08da02d5f9536748f).

## Design overview

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

ALEX Bridge also is bi-directional or “two-way” bridge, meaning you can freely transfer assets between Stacks and Ethereum and vice versa.

Users interact with "[Endpoints](understanding-the-bridge.md#endpoints)" on the source blockchain to lock assets to be bridged ("source asset"), and on the destination blockchain to receive the bridged asset ("destination asset").

Asset transfers from users to Endpoints are monitored by a group of "[validators](understanding-the-bridge.md#validators)", who produce cryptographic proofs that say "X amount of source asset are sent by address A on the source blockchain for address B on the destination blockchain to receive Y amount of destination asset."

There is a minimum threshold of such proofs that must be provided and verified before the assets received into Endpoint can be sent to the relevant address.

Upon meeting such minimum threshold, a "[relayer](understanding-the-bridge.md#relayers)" calls into Endpoint with the proofs to trigger the transfer of the received destination assets to the relevant address.

### Endpoints

Endpoints are the smart contracts that handle the asset transfers. They are owned by multisig contracts ([Gnosis Safe](https://safe.global/) on Ethereum and [Executor DAO](https://explorer.stacks.co/txid/0xf4bd95ea0486e6a50ae632c613f1d72b2a5bbbc4211b494cd0f1d3443658544d?chain=mainnet) on Stacks) operated by ALEX LAB Foundation.

Users use Endpoints to trigger transfer of source assets. The destination assets are then sent by a relayer by producing cryptographic proofs.

That the assets are held by contracts owned by multisig contracts is important because this minimises the risk of a malicious actor stealing the private key and assets sent by users.

CoinFabrik audited the Endpoints.

* Bridge Endpoints: [https://cdn.alexlab.co/pdf/ALEX\_Audit\_bridge\_coinfabrik\_202212.pdf](https://cdn.alexlab.co/pdf/ALEX\_Audit\_bridge\_coinfabrik\_202212.pdf)
* Bridge backends and endpoints: [https://cdn.alexlab.co/pdf/ALEX\_Audit\_Bridge\_2023-04.pdf](https://cdn.alexlab.co/pdf/ALEX\_Audit\_Bridge\_2023-04.pdf)

### Validators

Validators are responsible for producing cryptographic proofs, which must be verified by the Endpoints before transferring the destination assets to an address. Validators consist of ALEX LAB Foundation members and active community contributors.&#x20;

Validators are important because this set-up minimises the risk of a malicious actor taking over the system (for example, a relayer).

CoinFabrik audited the codes relavant to Validator.

* Bridge backends and endpoints: [https://cdn.alexlab.co/pdf/ALEX\_Audit\_Bridge\_2023-04.pdf](https://cdn.alexlab.co/pdf/ALEX\_Audit\_Bridge\_2023-04.pdf)

### Relayers

Relayers are responsible for triggering the destination asset transfer upon gathering a minimum threshold of cryptographic proofs produced by the validators.

CoinFabrik audited the codes relavant to Relayer.

* Bridge backends and endpoints: [https://cdn.alexlab.co/pdf/ALEX\_Audit\_Bridge\_2023-04.pdf](https://cdn.alexlab.co/pdf/ALEX\_Audit\_Bridge\_2023-04.pdf)






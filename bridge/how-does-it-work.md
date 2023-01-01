# How does it work?

As mentioned [before](what-is-bridge.md), ALEX Bridge partners with Wrapped to bring a bridge experience that is both secure and responsible.

## Custory of assets and monitoring of reserves

ALEX Bridge is custodial in that the tokens being bridged are held in contracts on Ethereum and Stacks owned and controlled by Wrapped. Users can monitor the reserves real-time at [https://open.wrapped.com/reserves/](https://open.wrapped.com/reserves/).

## Design overview

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

ALEX Bridge also is bi-directional or “two-way” bridge, meaning you can freely transfer assets between Stacks and Ethereum and vice versa.

Users interact with "Endpoints" on the source blockchain to send assets to be bridged ("source asset") to Wrapped. Upon receiving the source assets, Wrapped then sends the destination assets to the Endpoint on the destination blockchain.

Asset transfers from users to Wrapped and from Wrapped to Endpoints are monitored by a group of "validators", who produce cryptographic proofs that say "X amount of source asset are sent by address A on the source blockchain for address B on the destination blockchain to receive Y amount of destination asset."

There is a minimum threshold of such proofs that must be provided and verified before the assets received into Endpoint can be sent to the relevant address.

Upon meeting such minimum threshold, a "relayer" calls into Endpoint with the proofs to trigger the transfer of the received destination assets to the relevant address.

### Endpoints

Endpoints are the smart contracts that handle the asset transfers. They are owned by multisig contracts ([Gnosis Safe](https://safe.global/) on Ethereum and [Executor DAO](https://explorer.stacks.co/txid/0xf4bd95ea0486e6a50ae632c613f1d72b2a5bbbc4211b494cd0f1d3443658544d?chain=mainnet) on Stacks) operated by ALEX LAB Foundation.

Users use Endpoints to trigger transfer of source assets to Wrapped. The destination assets are sent to the Endpoints, which are then sent by a relayer by producing cryptographic proofs.

That the assets are held by contracts owned by multisig contracts is important because this minimises the risk of a malicious actor stealing the private key and assets sent by users and Wrapped.

CoinFabrik audited the Endpoints.

### Validators

Validators are responsible for producing cryptographic proofs, which must be verified by the Endpoints before transferring the destination assets to an address. Validators consist of ALEX LAB Foundation members and active community contributors.&#x20;

Validators are important because this set-up minimises the risk of a malicious actor taking over the system (for example, a relayer) and hijacking the destination assets as they arrive from Wrapped.

### Relayers

Relayers are responsible for triggering the destination asset transfer upon gathering a minimum threshold of cryptographic proofs produced by the validators.






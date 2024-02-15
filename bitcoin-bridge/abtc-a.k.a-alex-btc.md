# 🟧 aBTC, a.k.a ALEX BTC

### 1 aBTC = 1 BTC

aBTC, or ALEX BTC, is a fully-functional SIP010 token on Stacks, where 1 aBTC represents 1 BTC locked in the multisig operated by ALEX LAB Foundation.

Its role is somewhat similar to the role WETH plays for ETH - it allows BTC to interact with smart contracts on Bitcoin.

When a BTC holder interacts with smart contracts on Stacks, under the hood, [Bitcoin Bridge](broken-reference/) receives BTC into its [multisig](abtc-a.k.a-alex-btc.md#multisig-address) and triggers minting of the corresponding aBTC, which is then sent to the relevant smart contract to interact on behalf of the BTC holder.

aBTC holders can also interact with smart contracts on Stacks (just like they would do with any SIP010 tokens) to receive BTC into their Bitcoin wallet, in which case, Bitcoin Bridge burns aBTC and sends the relevant BTC from its multisig.

### Enabler of Bitcoin Finance

So if we say the combination of Bitcoin as the data layer and Stacks as its smart contract layer is a complete circulatory system that drives the body of Bitcoin economy, Bitcoin Bridge is its blood vessels and aBTC its blood.

### So how is aBTC different from [sBTC](https://sbtc.tech)?

We are introducing aBTC in anticipation of the roll-out of sBTC, to demonstrate to BTC holders what is possible when Bitcoin has its own smart contract layer and prepare our community for the sBTC roll-out.

aBTC gives us an early opportunity to develop the kind of user experience, _that_ Bitcoin DeFi experience, we want to have when sBTC is released and identify the areas of improvement to help its development.

Bitcoin Bridge will readily integrate sBTC when it is available, alongside aBTC.

### So how is aBTC different from [xBTC](https://open.wrapped.com/coins/XBTC)?

ALEX is a big user / supporter of xBTC, which will continue.

xBTC and aBTC are complementary because they follow different custodial models. The former uses institutional custodian and requires KYC for wrap/unwrap, while the latter uses a community-owned multisig and does not require KYC.

We are introducing aBTC because it makes possible the kind of tight integration of BTC with Stacks smart contract we want, to deliver that "native-like" DeFi experience on Bitcoin, which is not possible with xBTC.

### High-level summary of comparison between aBTC, xBTC and sBTC

<table><thead><tr><th></th><th>aBTC</th><th width="186">xBTC</th><th>sBTC</th></tr></thead><tbody><tr><td>Issuer</td><td>ALEX</td><td>Wrapped</td><td>Stacks</td></tr><tr><td>Custodial model</td><td>Community-owned multisig</td><td>Institutional custodian</td><td>Open membership multisig</td></tr><tr><td>Integration with Bitcoin Bridge</td><td>Yes</td><td>No</td><td>Yes</td></tr><tr><td>KYC</td><td>No</td><td>Yes</td><td>No</td></tr><tr><td>Availability</td><td>Now</td><td>Now</td><td>2024</td></tr></tbody></table>

### Where can I find more information on aBTC?

#### Reserve

{% embed url="https://app.alexlab.co/bridge-reserve" %}

#### Contract deployment

{% embed url="https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-abtc?chain=mainnet" %}

#### Multisig address

{% embed url="https://mempool.space/address/bc1pylrcm2ym9spaszyrwzhhzc2qf8c3xq65jgmd8udqtd5q73a2fulsztxqyy" %}

#### Token logo

<figure><img src="https://token-images.alexlab.co/token-abtc" alt=""><figcaption></figcaption></figure>

###

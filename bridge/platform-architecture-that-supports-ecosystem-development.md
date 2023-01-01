# What is Bridge?

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Developed in partnership with [Wrapped](https://app.gitbook.com/s/wFwxyo05u2LDudAxiuAv/), ALEX Bridge connects Stacks and Ethereum, allowing users to move crypto assets between the two blockchains.

ALEX Bridge will initially support the exchange of USDC (ERC20) and xUSD (Stacks), with a plan to add support for xALEX/ALEX, WBTC/xBTC, ETH/xETH and others.

## Why do we need a bridge to move crypto assets between blockchains? <a href="#cdf8" id="cdf8"></a>

If you want to move tokens from one blockchain to another, you’ll need to use a blockchain bridge to transfer those assets.

Blockchain assets are often not compatible with one another — Stacks uses SIP-10 tokens and Ethereum uses ERC-20. To solve this incompatibility, blockchain bridges create synthetic derivatives that represent an asset from another blockchain.

So when a bridge is used to send USDC tokens from Ethereum to a Stacks wallet, that wallet will receive a token that has been “[wrapped](https://www.coindesk.com/learn/what-are-wrapped-tokens/)” by the bridge — converted to a token based on the target blockchain, in this case from USDC to xUSD, which are pegged 1:1.

## What are the main types of blockchain bridges? <a href="#2393" id="2393"></a>

Bridges are either **custodial** (also known as centralized or trusted) or **noncustodial** (decentralized or trustless). The difference is in who controls the tokens that are used to create the bridged assets.

ALEX Bridge is custodial; USDC is wrapped into xUSD by Wrapped with a multisig contract on Ethereum.

ALEX Bridge is also bi-directional or “two-way” bridge, meaning you can freely transfer assets between Stacks and Ethereum and vice versa.

## Is ALEX Bridge safe? <a href="#caef" id="caef"></a>

As with all crypto technology, risk is real whether using a centralized or decentralized bridge. Some of the more novel decentralized bridges are relatively untested and even those that have been tested are still subject to exploits.

ALEX Bridge uses Wrapped ([https://wrapped.com/](https://wrapped.com/)), a long-established and reputable tech partner that has already collaborated with Stacks in creating xBTC. Wrapped is focused on building wrapped bridge technologies on multiple blockchains, including Ethereum, Solana, Polkadot and Stacks.

ALEX Bridge uses both ALEX and Wrapped technologies, enabling us to better segregate the risk. With fine-tuning and careful testing, we are getting ready to bring a reliable and easy-to-use bridge to all Stacks users.

## What is Wrapped? <a href="#0f40" id="0f40"></a>

Wrapped was [launched by Anchorage and Tokensoft](https://blog.tokensoft.io/tokensoft-partners-with-anchorage-to-bring-wrapped-layer-one-assetsto-ethereum-e08c4a3b72c), and uses both [Tokensoft](https://www.tokensoft.io/) and Anchorage technologies. In this partnership, Anchorage provides custody of the underlying assets as a qualified custodian. Tokensoft, the leading technology platform for companies issuing digital assets, administers the digital representation of the asset on the multiple networks.

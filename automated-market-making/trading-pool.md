# Our Design

## Introduction

In this section you will find an overview of the ALEX "Trading Pool" and its automated market making (AMM) protocol.

At ALEX, we build DeFi primitives targeting developers looking to build ecosystem on Bitcoin. As such, we focus on trading of crypto assets with Bitcoin as the settlement layer. At the core of this focus is the AMM protocol, which allows users to exchange one crypto asset for another in a trustless manner.

Trading Pool implements Generalized Mean Equation and, with a suitable parameterisation, supports both risky pairs (i.e. $$x y=L$$), stable pairs (i.e. $$x +y=L$$) and any linear combination in-between (i.e. Curve).

Trading Pool is parameterised with a single parameter $$t$$. $$t$$ can be between 0 and 1, with $$t=1$$ being equivalent of constant product formula (i.e. Uniswap V2) and $$t=0$$ being equivalent of constant sum formula (i.e. mStable). $$0<t <1$$ then gives a Curve-like formula.

Regarding its implementation, the Trading Pool protocol consists of a set of smart contracts built on the Stacks blockchain that facilitate trading operations within the ALEX DeFi ecosystem. Below, there are listed the main features of the pool.

{% hint style="info" %}
For more of the theory and fundaments behind the Alex AMM protocol refer to the [ALEX AMM Whitepaper](../whitepaper/automated-market-making-of-alex/). If you are looking for technical details and implementation design please refer to the [Developers Protocol Contracts section](../developers/protocol-contracts/#alex-dao-amm-trading-pool).
{% endhint %}

{% hint style="danger" %}
**Caution on fixed notation**: Please note we use 8-digit fixed notation to represent decimals. If you interact directly with any of our contracts, you must provide all numbers in the correct format. For example, 1 should be passed as 10,000,000 (= 1e8), i.e. 1.00000000.
{% endhint %}

## Pool management

### Pool creation

A pair can be registered (i.e. a pool can be created) by calling `create-pool` function indicating the traits of the two tokens (`token-x` and `token-y`), the factor $$t$$, the governance address (`pool-owner`) and the initial liquidity.

Trading Pool is permission-less in that anyone can register a pair with initial liquidity, so long as the two tokens are pre-approved (this is to prevent introducing malicious tokens to the platform).

### Pool governance

Certain privileged functions are available to `pool-owner` to govern the pool. The `pool-owner` address is set at the time of a pool creation. ALEX DAO, as part of its governance, has the power to update and replace the `pool-owner` address. Therefore, you can view this as ALEX DAO delegating the governance of each pool to its respective `pool-owner`.

[Refer to the comprehensive list of pool-governed setters](../developers/protocol-contracts/amm-pool-v2-01.clar.md#setters).

## Pool liquidity operations

Users can participate by adding (injecting liquidity with function `add-to-position`) or reducing (withdrawing with function `reduce-position`) assets positions in a specific pool that deals with a pair of tokens. When users add assets, they receive pool tokens (a.k.a. LP Tokens), which represent their share of the pool and potential earnings. When withdrawing assets, users return pool tokens.

### Adding liquidity

When adding liquidity to a pool, you need to specify the amount of token-x and have the option of specifying the maximum amount of token-y you are willing to pair with token-x (i.e. slippage control). If adding liquidity requires more token-y than the maximum you specified, then the call will fail. You must also have at least the amount of token-x and the maximum amount of token-y in your wallet - otherwise the call will fail.

Once the liquidity is added, the pool will mint a pool token as a proof of proportional ownership of the pool liquidity. The number of the pool token being minted is proportional to the amount of liquidity you added compared to the existing liquidity at the pool.

The pool token is transferable and may be used at other protocols (for example, as a collateral).

### Removing liquidity

When removing liquidity from a pool, you need to specify the percentage of your pool tokens that you want to liquidate, i.e. between 0 and 1.

The percentage will be converted to the number of pool tokens to be burnt and the corresponding amount of token-x and token-y will be sent to you.

## Trading

When a pool for a specific token pair is funded, it allows users to exchange those tokens, with a fee for each swap. Users can swap one token with another by calling `swap-x-for-y` or `swap-y-for-x`. As the names imply, `swap-x-for-y` swaps token-x into token-y and `swap-y-for-x` swaps token-y into token-x.

Users can specify the slippage limit (the minimum amount of the desired target token they expect to receive: `min-dy` and `min-dx`, respectively), so that the call fails if the swapped amount does not meet your target.

### Swap helper and routing

It may not be reasonable to expect developers or users to remember the correct order of token pairs. Therefore, we provide `swap-helper` function that helps choose between `swap-x-for-y` and `swap-y-for-x` and swaps `token-x` into `token-y` without users having to know the correct order.

Sometimes, a direct swap isn't possible. In such cases, the system employs intermediate tokens to complete the exchange. For example, swapping Token-A to Token-C might require an intermediate swap through Token-B. This process is known as a multi-hop or multi-step swap. It is intended for scenarios where a direct pool for Token-A/Token-C does not exist, but there are pools for Token-A/Token-B and Token-B/Token-C. To facilitate multi-hop swaps, we provide three helper routing functions: `swap-helper-a`, `swap-helper-b`, and `swap-helper-c`. These functions support multi-hop swaps involving two, three, and four pools, respectively.

## Helper functions

In addition to the swap helpers and routing functions, we provide a few helpful functions.

### Oracle

Trading Pool provides two types of on-chain oracles - instant and resilient. Their implementations are similar to Uniswap V2.

Instant oracle (`get-oracle-instant`) gives you the latest pool-implied price (i.e. most up-to-date), but is subject to a higher manipulation risk. Instant oracle may be suitable for, for example, arbitrage or liquidation.

Resilient oracle (`get-oracle-resilient`) on the other hand gives you a trade-weighted price that is therefore more resilient to potential manipulation but is less up to date. Resilient oracle may be more suitable for, for example, benchmarking to lending and borrowing.

### Liquidity provision

In certain cases, prior information is necessary to perform operations effectively. For example, to determine the slippage limit, you need to know the specific amounts of token-x and token-y required to mint a certain number of pool tokens. Additionally, it can be useful to know how many pool tokens can be minted or burnt if a specific amount of token-x and token-y are provided. The relevant helper functions for these operations are: `get-position-given-mint`, `get-position-given-burn` and `get-token-given-position`.

## Glossary

### Base Token

The base token is the cryptocurrency token that a user currently possesses and submits during a swap transaction.

### Dx

The amount of the token-x involved in liquidity and trading operations.

### Dy

The amount of the token-y involved in liquidity and trading operations.

### Factor

The factor is a multiplier (scaling factor) defined within a token pair pool, playing a critical role in determining the value of `dy` (amount of target token) given `dx` (amount of base token) and the pool balances of each token. Together with the token principals, the factor constitutes the pool identifier that is utilized to retrieve the pool details.

### Fee

The cost associated with performing a swap or other operations within the platform. It is deducted from each transaction on the "in" leg (i.e., token-x for `swap-x-for-y` and token-y for `swap-y-for-x`). The fee to be calculated is set at the [pool creation](trading-pool.md#pool-creation) and may be updated through the governance. Part of the fee may be [rebated](trading-pool.md#fee-rebate) to liquidity providers as a reward.

### Fee Rate

The percentage of the transaction amount that is taken as a fee during a swap or other operations.

### Fee Rebate

The portion of the swap fee that is reinvested into the relevant pool's liquidity, causing the pool's invariant to increase slightly after each transaction. This mechanism is similar to that of Uniswap V2.

### In given out

If you want to know the amount of token-x you may need to provide to get a target amount of token-y (or vice versa), you can use `get-x-in-given-y-out` and `get-y-in-give-x-out`, respectively.

### In given price

If you are an arbitrageur, you may want to know the amount of token-x or token-y you need to provide to rebalance the pool-implied price to a target. In such a case, you can use `get-x-given-price` or `get-y-given-price`.

### Liquidity Positions

When a user provides liquidity to a pool, they are said to be adding liquidity positions (using the `add-to-position` function). The reverse operation occurs when users withdraw their assets, thus reducing their positions (using the `reduce-position` function). Both operations involve the minting and burning of LP Tokens, respectively, to represent the user's share of the pool.

### Maximum Dy (`max-dy`)

In the context of an add positions transaction, this amount specifies the maximum quantity of token-y that the user is willing to deposit. If the required amount exceeds this specified limit, the transaction will be reverted with the error `ERR-EXCEEDS-MAX-SLIPPAGE`.

### Minimum Dy (`min-dy`)

In the context of a swap transaction, this amount defines the minimum quantity of the target token that the user expects to receive. If the resulting amount falls below this specified threshold, the transaction will be reverted with the error `ERR-EXCEEDS-MAX-SLIPPAGE`.

### Out given in

Sometimes you may want to know the expected amount of token-y if you were to swap certain amount of token-x. Or you may want to know the expected amount of token-x for some token-y. Two read-only functions - `get-y-given-x` and `get-x-given-y` will do that for you.

### Pool token / LP Token

Pool token, also known as "LP Token" (Liquidity Provider Token); issued to users who contribute assets to a liquidity pool, representing their share of the pool and potential earnings. The token contract is `token-amm-pool-v2-01` and it implements [SIP013](https://github.com/stacksgov/sips/pull/42). Each pool is mapped to a unique id (`pool-id`) with associated liquidity mapped to the balance under that id (`token-id`).

### Ratio

The ratio represents the relationship between the amounts of a token pair involved in pool operations. Each pool has two predefined values, `max-in-ratio` and `max-out-ratio`, which set the maximum amounts that can be involved in swap operations.

### Slippage

In the Automated Market Maker (AMM) Pool contract, slippage refers to the difference between the calculated amount of the target token and the configured maximum or minimum limit during a transaction. If no limit is set, the default limits are `u340282366920938463463374607431768211455` (the maximum value for `uint` type in Clarity language: `2**128 - 1`) for the maximum and `u0` for the minimum. These limits are enforced within the pool contract and are validated using the custom error `ERR-EXCEEDS-MAX-SLIPPAGE`.

### Swap Transaction

A swap transaction refers to a type of operation whereby a user exchanges a given quantity of one cryptocurrency token for another. Within the context of the ALEX Trading Pool, swap transactions are executed using predefined token pairs existing within a pre-established liquidity pool.

### Target Token

Also known as the "quoted" token, the target token is the cryptocurrency token that a user will receive as a result of a swap transaction.

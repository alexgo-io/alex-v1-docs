# Trading Pool

Please refer to our [white paper](../whitepaper/automated-market-making-of-alex/) for a more rigorous treatment on the subject.

At ALEX, we build DeFi primitives targeting developers looking to build ecosystem on Bitcoin, enabled by [Stacks](https://www.stacks.co/). As such, we focus on trading, lending and borrowing of crypto assets with Bitcoin as the settlement layer and Stacks as the smart contract layer. At the core of this focus is the automated market making ("AMM") protocol, which allows users to exchange one crypto asset with another trustlessly.

Trading Pool implements Generalised Mean Equation and, with a suitable parameterisation, supports both risky pairs (i.e. $$x y=L$$), stable pairs (i.e. $$x +y=L$$) and any linear combination in-between (i.e. Curve).

Trading Pool is parameterised with a single parameter $$t$$. $$t$$ can be between 0 and 1, with $$t=1$$ being equivalent of constant product formula (i.e. Uniswap V2) and  $$t=0$$ being equivalent of constant sum formula (i.e. mStable). $$0<t <1$$ then gives a Curve-like formula.

## <mark style="color:red;">Caution on fixed notation</mark>

Please note we use 8-digit fixed notation to represent decimals. If you interact directly with any of our contracts, you must provide all numbers in the correct format.

For example, 1 should be passed as 10,000,000 (= 1e8), i.e. 1.00000000.

## Pool creation

A pair can be registered (i.e. a pool can be created) by calling `create-pool` with the parameters including the traits of the two tokens (`token-x` and `token-y`), the factor $$t$$, the governance address  (`pool-owner`) and the initial liquidity.

Trading Pool is permission-less in that anyone can register a pair with initial liquidity, so long as the two tokens are pre-approved (this is to prevent introducing malicious tokens to the platform).

### Pool governance

Certain privileged functions are available to `pool-owner` to govern the pool. The `pool-owner` address is set at the time of a pool creation. ALEX DAO, as part of its governance, has the power to update and replace the `pool-owner` address. Therefore, you can view this as ALEX DAO delegating the governance of each pool to its respective `pool-owner`.

* `set-fee-rate-x` and `set-fee-rate-y`: set the swap fee (% of swap amount) of `token-x` and `token-y`, respectively. Both `fee-rate-x` and `fee-rate-y` are zero by default;
* `set-start-block` and `set-end-block`: set the block heights before and after, respectively, which the pool is not available. Both `start-block` and `end-block` is set to `u340282366920938463463374607431768211455` by default;
* `set-threshold-x` and `set-threshold-y`:  set the amount of `token-x` and `token-y`, respectively, below which a minimum % slippage is applied. Both `threshold-x` and `threshold-y` are zero by default;
* `set-oracle-enabled`: add or remove the pool from the on-chain price oracle. Oracle is disabled by default;
* `set-oracle-average`: set the exponential moving average factor for the `oracle-resilient`. Please note this call will reset the existing `oracle-resilient` value. The `oracle-average` is zero by default. We recommend 0.99e8.

## Liquidity provision

Liquidity can be added to or removed from the pool any time by calling `add-to-position` or `reduce-position`, respectively.

### Adding liquidity

When adding liquidity to a pool, you need to specify the amount of token-x and have the option of specifying the maximum amount of token-y you are willing to pair with token-x (i.e. slippage control). If adding liquidity requires more token-y than the maximum you specified, then the call will fail. You must also have at least the amount of token-x and the maximum amount of token-y in your wallet - otherwise the call will fail.

Once the liquidity is added, the pool will mint a pool token as a proof of proportional ownership of the pool liquidity. The number of the pool token being minted is proportional to the amount of liquidity you added compared to the existing liquidity at the pool.

The pool token is transferrable and may be used at other protocols (for example, as a collateral).

### Removing liquidity

When removing liquidity from a pool, you need to specify the percentage of your pool tokens that you want to liquidate, i.e. between 0 and 1.

The percentage will be converted to the number of pool tokens to be burnt and the corresponding amount of token-x and token-y will be sent to you.

### Pool tokens

The pool token implements [SIP013](https://github.com/stacksgov/sips/pull/42). Each pool is mapped to a unique id (`pool-id`) with associated liquidity mapped to the balance under that id.

### Fee rebates

Trading Pool re-invests any fee rebates to the relevant pool liquidity, i.e. the invariant increases slightly after each transaction, similar to Uniswap V2.

## Trading

Users can swap one token with another by calling `swap-x-for-y` or `swap-y-for-x`. As the names imply, `swap-x-for-y` swaps token-x into token-y and `swap-y-for-x` swaps token-y into token-x.&#x20;

In both cases you can specify your slippage limit (`min-dy` and `min-dx`, respectively), so that the call fails if the swapped amount does not meet your target.

### Fee

Fee is deducted from each transaction on the "in" leg, i.e. token-x for `swap-x-for-y` and token-y for `swap-y-for-x`. Fee is set at the [pool creation](trading-pool.md#pool-creation) and may be updated through the governance.

Part of the fee may be [rebated](trading-pool.md#fee-rebates) to liquidity providers as a reward.

### Swap helper and routing

It may not be reasonable to expect developers or users to remember the correct order of token pairs. Therefore we provide `swap-helper` function that helps choose between `swap-x-for-y` and `swap-y-for-x` and swaps token-x into token-y without users having to know the correct order.

It is also useful to be able to combine multiple swaps into one. For example, it will be useful to be able to swap xUSD into xBTC in one transaction, based on two pools of STX/xUSD and STX/xBTC, instead of having to perform two swaps. To that end, we provide three helper functions - `swap-helper-a`, `swap-helper-b` and `swap-helper-c` that facilitates "multi-hop" swaps of two/three/four pools, respectively.&#x20;

## Helper functions

In addition to the swap helpers and routing functions, we provide a few helpful functions.

### Oracle

Trading Pool provides two types of on-chain oracles - instant and resilient. Their implementations are similar to Uniswap V2.

Instant oracle (`get-oracle-instant`) gives you the latest pool-implied price (i.e. most up-to-date), but is subject to a higher manipulation risk. Instant oracle may be suitable for, for example, arbitrage or liquidation.

Resilient oracle (`get-oracle-resilient`) on the other hand gives you a trade-weighted price that is therefore more resilient to potential manipulation but is less up to date. Resilient oracle may be more suitable for, for example, benchmarking to lending and borrowing.

### Out given in

Sometimes you may want to know the expected amount of token-y if you were to swap certain amount of token-x. Or you may want to know the expected amount of token-x for some token-y. Two read-only functions - `get-y-given-x` and `get-x-given-y` will do that for you.

### In given out

If you want to know the amount of token-x you may need to provide to get a target amount of token-y (or vice versa), you can use `get-x-in-given-y-out` and `get-y-in-give-x-out`, respectively.

### In given price

If you are an arbitrageur, you may want to know the amount of token-x or token-y you need to provide to rebalance the pool-implied price to a target. In such a case, you can use `get-x-given-price` or `get-y-given-price`.

### Liquidity provision

Lastly, it will be useful to know, for example, to determine the slippage limit, how many token-x and token-y must be provided to mint certain number of pool tokens, to burn certain number of pool tokens, or how many pool tokens may be minted or burnt if certain number of token-x and token-y are provided. The relevant helper functions are `get-position-given-mint`, `get-position-given-burn` and `get-token-given-position`.






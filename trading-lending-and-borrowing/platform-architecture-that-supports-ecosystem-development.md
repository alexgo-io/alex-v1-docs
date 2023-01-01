# Our Design

ALEX allows for implementation of arbitrary trading strategies and borrows its design from [Balancer V2](https://docs.balancer.fi).

**Equation** abstracts rebalancing and market making logic, while **Pool** encapsulates the value of a strategy. Pool abstraction allows aggregation of the assets of all ALEX pools into a single **vault**, bringing many advantages over traditional DEX architecture.

## Equation

Equation triggers Pool rebalancing. This allows creation of any arbitrary rebalancing strategies to be deployed as a pool.

### Generalised Mean Equation

Generalised Mean Equation is an invariant function associated with generalised mean that underpins the [non-lending Trading Pools](trading-pool.md) at ALEX. With a suitable parameterisation, Generalised Mean Equation supports both risky pairs (i.e. $$x y=L$$), stable pairs (i.e. $$x +y=L$$) and any linear combination in-between (i.e. Curve)

### Weighted Equation

Weighted Equation is the most basic Equation of all and is a fork of [Balancer](https://balancer.fi/whitepaper.pdf), which generalised the constant product AMM popularised by Uniswap. We implement the following formula:

$$
V=\prod_{i}B_{i}^{w_{i}}
$$

Where $$V$$is a constant, $$B_{i}$$ is the balance of token i and $$w_{i}$$ is the weight of token i in the pool.

As the price of each token changes, arbitrageurs rebalance the pool by making trades. This maintains the desired weighting of the value held by each token whilst collecting trading fees from the traders. [Collateral Rebalancing Pool](collateral-rebalancing-pool.md) implements the Weighted Equation.

### Yield Token Equation

Yield Token Equation drives [Yield Token Pool](automated-market-making-designed-for-lending-protocols.md). It follows [Yield Space](https://yield.is/YieldSpace.pdf) and is designed specifically to facilitate efficient trading between ayToken and Token. Our main contribution is to extend the model to allow for capital efficiency from liquidity provision perspective (inspired by [Uniswap V3](https://uniswap.org/whitepaper-v3.pdf)).

For example, if a pool is configured to trade between 0% and 10% APY, the capital efficiency can improve to 40x compared to when the yield can trade between $$-\infty$$ and $$+\infty$$.

## Pool

Pools handle the logic of dynamic trading strategies, whose token rebalancing are then handled by Vault. Rebalancing logic is driven by Equation. Pool issues Pool Token to liquidity providers. The number of Pool Tokens are determined based on a liquidity provider's relative contribution to the Pool. Pool Tokens thus represent proportional ownership of that Pool, or assets in that Pool.

### Trading Pool

Trading Pool implements Generalised Mean Equation and, with a suitable parameterisation, supports both risky pairs (i.e. $$x y=L$$), stable pairs (i.e. $$x +y=L$$) and any linear combination in-between (i.e. Curve).

### Collateral Rebalancing Pool

Collateral Rebalancing Pool ("CRP") uses Weighted Equation and dynamically rebalances between Token and Collateral. CRP dynamically rebalances collateral to ensure the ayToken minted (i.e. the loan) remains solvent especially in an adverse market environment (i.e. the value of the loan does not exceed the value of collateral).

### Yield Token Pool

Yield Token Pool ("YTP") uses Yield Token Equation and is designed specifically to facilitate efficient trading between ayToken and Token.

## Vault

Vault holds and manages the assets of all ALEX pools. The separation of pool and vault has many benefits including, among others, cheaper transaction costs for users and quicker learning curve for developers when building custom pools on ALEX.

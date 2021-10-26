# Liquidity Mining

## Introduction

Liquidity mining program, in which liquidity providers \(LPs\) are rewarded for providing passive liquidity to facilitate trading activities, is relatively new to the crypto world. The topic has attracted community-wide attention since Compound introduced its COMP token in the mid of 2020 partly as a way to reward the LPs on their platform. However, compensating LPs or market makers are commonplace in conventional finance, in which the role of market makers and their contribution to a functioning market is well documented.

ALEX acknowledges the importance of LPs to the platform's success and would like to have in place appropriate incentives to participate in our ground-breaking DeFi protocol. As the one-stop shop built on Stacks, ALEX offers a decentralised exchange \(DEX\) with an embedded constant product automatic market making \(AMM\). We are also a platform of loanable fund \(PLF\) delivering fixed-term and fixed-rate loans to users who wish to utilise their cryptos better and earn interest.

A good incentive scheme aligns ALEX's interest in developing business and improving market efficiencies with LP's interests. At ALEX, we find the right balance with the introduction of our governance token “ALEX” as a key tool to achieve that goal.

## Liquidity Mining

On ALEX LPs can participate in either DEX or PLF. Our liquidity mining program means LPs of a pool collect a specific portion of the transaction fees incurred on that pool in terms of ALEX token after each block. For example, let's say the fees accumulated for a given pool at a given block equals $50,000. Assume 50% of the fees is distributed, equivalent to $25,000. If the price of the ALEX token is $20, 1,250 \(25,000/20\) tokens will then be distributed to the liquidity providers of the pool.

Our token distribution plan calls for 18.4% of the total initial supply of ALEX tokens to be reserved for the liquidity mining program. The policy is subject to change, should the community deem it necessary in the future.

### Decentralised Exchange \(DEX\)

ALEX DEX employs the constant product form of AMM, which has been widely adopted in the Defi community due to its simplicity in form and interpretability in economics.

Being an LP in DEX is straightforward. The only requirement is to deposit two assets in the relevant trading pool to participate in liquidity for swap transactions. The tokens should be added in proportion to the existing balance. For example, we assume a pool of USDC/BTC comprising 10mm USDC and 200 BTC. Injecting liquidity equal to 1% of the current pool requires the addition of 100,000 USDC and 2 BTC, increasing the pool balance to 10.1mm USDC and 202 BTC.

LPs participate in the transaction fees generated in the pool. Liquid pairs, such as BTC/USDC, often are subject to lower transaction cost, whereas illiquid pairs would typically be subject to a higher transaction fee reflecting the assets' tradability.

A common problem facing LPs is impermanent loss, as LPs end up buying tokens as their value goes up and vice versa. The loss is relative to a portfolio called "buy-and-hold", which consists of the same balance of tokens as LP's initial deposit and stays constant. Loss resulting from market-making activities is not unusual especially in highly competitive markets, as the counterparty's gain would naturally become the market maker's loss. As "impermanent" implies, such a loss could be temporary, if, for example, the price moves back to the initial level LP enters the trading pool.

In general, the loss is often manageable if the market is mean-reverting. A market with momentum in certain direction may be subject to a rising impermanent loss, but it is often offset by an increase in transaction fees due to more active trading. LPs’ long-term commitment, therefore, is important to any DeFi protocols, as it would create a virtuous circle that generates profit and guarantees protocol continuity.

### Platform of Loanable Fund \(PLF\)

ALEX PLF offers fixed term and fixed rate loans by trading a token called Yield Token. The difference of its price and par is approximately the interest. Yield Token is explicitly designed for Yield Token Pool \(YTP\). Exchanging of Yield Token and Token is equivalent to borrowing and lending activity.

Providing liquidity to YTP requires both Token and Yield Token. There are two approaches that an LP can consider when obtaining Yield Tokens:

One is to mint Yield Tokens after depositing collateral at the designated Collateral Rebalancing Pool \(CRP\). Minting Yield Token is similar to buying bonds from the issuer in the "primary market" in conventional finance. CRP requires over-collaterisation and might not be suitable for LPs who have a limited balance sheet. It could benefit large market makers seeking to boost liquidity and become a leading LP of YTP.

The other way to obtain Yield Token is by swapping tokens in the existing pool. This is similar to "secondary market" in bond terms which don't involve token minting. We expect most LPs to adopt this approach due to its simplicity in execution. However, the amount of Yield Token available for purchase would be constrained by the outstanding pool balance. Furthermore, purchasing Yield Token increases its demand, which could subsequently reduce the interest rate, potentially resulting in an arbitrage opportunity against the LP.

As ALEX YTP incorporates cap and floor to the market making, the capital efficiency for LPs will improve. In an extreme case of a 0% interest rate, LP requires depositing no Yield Tokens at all. This is the case when no pool exists for the current contract, and thus a new pool needs to be initiated.

#### Example 

An existing YTP pool has 9,512 USDC and 10,513 yield-USDC, with an interest rate floor at 0%. With time to maturity t at 0.99, the balance implies an interest rate of 10%. In this example, because the pool only deals with the positive interest rate \(i.e. interest rate floor at 0%\), the actual balance of yield-USDC is 513, while the remaining 10,000 is “virtual”.

Rachel has 1,000 USDC and hopes to be an LP of the pool. She deposits all her USDC to YTP, among which a portion would be converted to yield-USDC. The table below demonstrates this process.

![](https://lh3.googleusercontent.com/evIpW1hhUjEJvMCfZwPojdvPSixJtaIjg1yQVYPH3RPVBNsgdb47_TNTfxo74c9-NNVM0SBKZmFsPrllsOaQeAPf_kzOudymTC6Djql-3Ks2YWto_PpcSdMQpMBOd2I8fKvOvew=s0)

Regarding the actual token balance, USDC would be increased by 1,000 – the capital Rachel contributes. In contrast, yield-USDC remains unchanged because all the swapped yield-USDC would be returned to the liquidity pool to optimise coin usage. As USDC increases in the pool, virtual yield-USDC balance would increase proportionally. According to our calculation, YTP would end up consisting of 10,512 USDC and 11,515 yield-USDC. However, the interest rate reduces to 9.11% because the newly added USDC represents more than 10% of the original token balance – big enough to reduce interest by almost 0.9%.

![](https://lh5.googleusercontent.com/iw0y-OVBAABnsPBZVjzW0e8sgqEONlRYl6tKLX0SafIHm4_EB6Dx7kbOt595_T17UmraswME45UkdJOk3SIdeinz4K9F_R8YS8b3z_ERHxAk7S-W0gYBfSOjhdjWCapmK6q3JuU=s0)

Figures above further illustrate the change in the liquidity after Rachel’s participation. Green point shows the initial balance with interest rate at 10%. Black point displays the balance after Rachel swaps some of her USDC for yield-USDC. Red point updates the liquidity pool as Rachel injects all her capital and increases pool liquidity by around 10%. Left and right chart shows the liquidity information without and with capital efficiency respectively.

LP's PnL depends on the outlook of the interest rate path, how long LP intends to stay in the pool and the transaction fees generated in the pool.

If the interest rate is expected to increase in the future, there will be more borrowers than lenders on the market, as borrowers hope to lock the current low rate. Consequently, borrowers would mint Yield Token and sell it to LPs in exchange for Token. LP behaves like a lender in this case and earns interest while providing liquidity.

If the interest rate is deemed elevated and is expected to decrease, more lenders will enter the market to secure the high interest. In this case LP serves like a borrower, and principal and interest needs to be returned when the contract matures.

In reality, the interest rate path is hard to predict. Unlike conventional finance, where the interest rate is associated with central bank base rate policy, the cryptocurrency interest rate tends to be driven purely by demand and supply and may lack clear direction. In the case of no transaction cost, LP return would be approximately flat, because the market would be two ways involving both borrowers and lenders. LPs would simply connect two parties and trade in-and-out without warehousing any positions.

Like typically is the case for market makers across all financial products, the primary source of income for LP is not from betting the market direction correctly, but the bid-offer spread from each transaction.


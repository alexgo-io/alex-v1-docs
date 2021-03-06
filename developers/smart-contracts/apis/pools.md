---
description: >-
  Pools are smart contracts which handles the logic of swaps and liquidity
  addition & removal.  They handle logic of dynamic trading strategies. Check
  Pool Protocol for details.
---

# Pools

* [Fixed Weight Pool](pools.md#fixed-weight-pool)
* [Collateral Rebalancing Pool](pools.md#collateral-rebalancing-pool)
* [Yield Token Pool](pools.md#fixed-weight-pool-1)
* [Liquidity Bootstrapping Pool](pools.md#liquidity-bootstrapping-pool)

## Fixed Weight Pool <a href="#fixed-weight-pool" id="fixed-weight-pool"></a>

Fixed weight pool is a pool driven by `weighted-equation` with fixed weights (i.e. similar to Uniswap):

### **get-pool-count**

**Prototype:**

`(define-read-only (get-pool-count))`

**Input:** `void`

**Output:** `bool | uint`

Get the number of currently existing pools.

### **get-pools**

**Prototype:**

`(define-read-only (get-pools))`

**Input:** `void`

**Output:** `bool | uint`

Get the list of currently existing pools.

### **get-pool-details**

**Prototype:**

`(define-read-only (get-pool-details (token-x-trait) (token-y-trait) (weight-x) (weight-y))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint`

**Output:** `bool | uint`

Gets the details of pool which matches the given parameter. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### **get-pool-contracts**

**Prototype:**

`(define-read-only (get-pool-contracts (pool-id))`

**Input:** `uint`

**Output:** `bool | uint`

Gets the details of pool using pool id. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### \*\*create-pool \*\*

**Prototype:**

`(define-public (create-pool (token-x-trait) (token-y-trait) (weight-x) (weight-y) (the-pool-token) (the-vault) (dx) (dy))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, <pool-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Creates a pool using the given parameter. Created pool can be reached through the unique id which is allocated through this function. dx and dy is initially added to the created pool.

### **add-to-position**

**Prototype:** `(define-public (add-to-position (token-x-trait) (token-y-trait) (weight-x) (weight-y) (the-pool-token) (the-vault) (dx) (dy))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, <pool-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Add Liquidity to the given pool. First retrieve the existing pool with token traits, then add liquidity of x and y to the specified pool. Adding the liquidity to the pool should conform with equation. Liquidity Provider receives pool token as a reward, which represents the ownership of the liquidity provided pool.

### **reduce-position**

**Prototype:** `(define-public (reduce-position (token-x-trait) (token-y-trait) (weight-x) (weight-y) (the-pool-token) (the-vault) (percent))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, <pool-token-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Remove Liquidity from the given pool. First retrieve the existing pool with token traits, then remove liquidity of x and y to the specified pool using the requested user (tx-sender)'s pool token. Removing the liquidity to the pool should conform with equation. Liquidity Provider receives corresponding dx and dy to his own vault while the used pool token is burnt.

### **get-oracle-resilient**

\*\*Prototype: \*\*`(define-read-only (get-oracle-resilient (token-x-trait) (token-y-trait) (weight-x) (weight-y))`

\*\*Input: \*\*`<ft-trait>, <ft-trait>, uint, uint`

**Output:** `uint | uint`

Returns pool-implied oracle price that is less up-to-date but more resilient to manipulation.

### get-oracle-instant

\*\*Prototype: \*\*`(define-read-only (get-oracle-resilient (token-x-trait) (token-y-trait) (weight-x) (weight-y))`

\*\*Input: \*\*`<ft-trait>, <ft-trait>, uint, uint`

**Output:** `uint | uint`

Returns pool-implied oracle price that is more up-to-date but less resilient to manipulation.

### **swap-x-for-y**

**Prototype:**`(define-public (swap-x-for-y (token-x-trait) (token-y-trait) (weight-x) (weight-y) (the-equation) (dx))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, <equation-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between the two tokens in a pool. With given dx, find the corresponding amount for swapping to dy using equation. Then conduct swapping by transferring between tx-sender and pool.

### **swap-y-for-x**

**Prototype:**`(define-public (swap-x-for-y (token-x-trait) (token-y-trait) (weight-x) (weight-y) (the-equation) (dy))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, <equation-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between the two tokens in a pool. With given dy, find the corresponding amount for swapping to dx using equation. Then conduct swapping by transferring between tx-sender and pool.

### **get-x-given-y**

\*\*Prototype: \*\*`(define-read-only (get-x-given-y (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y) (dy))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, uint`

**Output:** `bool | uint`

Returns a dx which conforms the weighted equation using dy. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dy that corresponds to given dx.

### \*\*\*\*

### **get-y-given-x**

\*\*Prototype: \*\*`(define-read-only (get-x-given-y (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y) (dx))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, uint`

**Output:** `bool | uint`

Returns a dy which conforms the weighted equation using dx. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given dy.

### **set-fee-to-address**

\*\*Prototype: \*\*`(define-public (set-fee-to-address (token-x-trait ) (token-y-trait ) (weight-x) (weight-y) (address))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, principal`

**Output:** `bool | uint`

Set the platform fee collector address of given pool. Platform fee gathered on each pool action will be collected to the given address after execution.

### **get-fee-to-address**

\*\*Prototype: \*\*`(define-read-only (get-fee-to-address (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint`

**Output:** `bool | uint`

Get the platform fee collector address which is currently set on the given pool.

### **get-fees**

\*\*Prototype: \*\*`(define-read-only (get-fees (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint`

**Output:** `bool | uint`

Get the current platform fee of given pool.

### collect-fees

\*\*Prototype: \*\*`(define-public (collect-fees (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint`

**Output:** `bool | uint`

Collect the platform fee from the collector address which is set on the given pool. The balance of each token in collector address will be initialized back to zero.

### get-token-given-position

\*\*Prototype: \*\*`(define-read-only (get-token-given-position (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y) (dx) (dy))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, uint, uint`

**Output:** `bool | uint`

Returns a token which conforms the weighted equation using dx and dy. This API is used to maintain the pool ratio using the equation and use to get token.

### get-x-given-price

\*\*Prototype: \*\*`(define-read-only (get-x-given-price (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y) (price))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, uint`

**Output:** `bool | uint`

Returns a dx which conforms the weighted equation using price. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given price.

### get-position-given-mint

\*\*Prototype: \*\*`(define-read-only (get-position-given-mint (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y) (token))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, uint`

**Output:** `bool | uint`

Returns a position which conforms the weighted equation using mint token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token mint. Mainly wraps the weighted equation of 'get-position-given-mint' for the easy usage in smart contract.

### get-position-given-burn

\*\*Prototype: \*\*`(define-read-only (get-position-given-burn (token-x-trait ) (token-y-trait ) (weight-x ) (weight-y) (token))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint, uint`

**Output:** `bool | uint`

Returns a position which conforms the weighted equation using burnt token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token burn. Mainly wraps the weighted equation of 'get-position-given-burn' for the easy usage in smart contract.

## Collateral Rebalancing Pool

Collateral Rebalancing pool is a pool which users inject their collateral and mint ayToken(against Token) which represents the proportional ownership of the pool. Unlike Fixed weight pool, pools can be identified with 'token', 'collateral' and 'expiry', where by using weighted equation, it is possible to obtain a weight dynamically for the various operations in collateral rebalancing pool. Dynamic weighting in this context depends on [Black & Scholes delta](https://en.wikipedia.org/wiki/Black%E2%80%93Scholes\_model). Collateral Rebalancing pool is mainly used by Borrower and Arbitrageurs. You can find detailed use cases [here](https://docs.alexgo.io/protocol/collateral-rebalancing-pool).

### **get-pool-count**

**Prototype:**

`(define-read-only (get-pool-count))`

**Input:** `void`

**Output:** `bool | uint`

Get the number of currently existing pools.

### **get-pools**

**Prototype:**

`(define-read-only (get-pools))`

**Input:** `void`

**Output:** `bool | uint`

Get the list of currently existing pools.

### **get-pool-details**

**Prototype:**

`(define-read-only (get-pool-details (token-x-trait) (token-y-trait) (weight-x) (weight-y))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint`

**Output:** `bool | uint`

Gets the details of pool which matches the given parameter. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### **get-pool-contracts**

**Prototype:**

`(define-read-only (get-pool-contracts (pool-id))`

**Input:** `uint`

**Output:** `bool | uint`

Gets the details of pool using pool id. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### **get-weight-x**

**Prototype:**

`(define-read-only (get-weight-x (token) (collateral) (expiry))`

**Input:** `<ft-trait> <ft-trait> uint`

**Output:** `bool | uint`

Gets the parameter corresponding weight through the internal calculation. Weight retrieving formula is implemented inside this function.

### \*\*create-pool \*\*

**Prototype:**

`(define-public (create-pool (token) (collateral) (the-yield-token) (the-vault)(dx) (dy))`

**Input:** `<ft-trait>, <ft-trait>,<yield-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Creates a pool using the given parameter. Created pool can be reached through the unique id which is allocated through this function. dx and dy is initially added to the created pool.

### **add-to-position**

**Prototype:** `(define-public (add-to-position (token) (collateral) (expiry) (the-yield-token) (the-vault)(dx) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint <yield-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Adds Liquidity to the given pool. First retrieve the existing pool with token traits, then add liquidity of x and y to the specified pool. Adding the liquidity to the pool should conform with equation. Liquidity Provider receives ayToken as a reward, which represents the ownership of the liquidity provided pool. For example, borrower can inject collateral(usda) to the CRP pool and mint ayToken(ayUsda).

### \*\*\*\*

### **reduce-position**

**Prototype:** `(define-public (reduce-position (token) (collateral) (expiry) (the-yield-token) (the-vault) (percent))`

**Input:** `<ft-trait>, <ft-trait>, uint, <yield-token-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Remove Liquidity from the given pool. First retrieve the existing pool with token traits, then remove liquidity of x and y to the specified pool using the requested user (tx-sender)'s pool token. Removing the liquidity to the pool should conform with equation. Liquidity Provider receives corresponding dx and dy to his own vault while the used pool token is burnt.

### \*\*\*\*

### **swap-x-for-y**

**Prototype:**`(define-public (swap-x-for-y (token) (collateral) (expiry) (the-vault) (dx))`

**Input:** `<ft-trait>, <ft-trait>, uint, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between the two tokens in a pool. With given dx, find the corresponding amount for swapping to dy using weighted equation. Then conduct swapping by transferring between tx-sender and pool. For the CRP use cases, arbitrageurs uses to gain profit by swapping in the pool.

### **swap-y-for-x**

**Prototype:**`(define-public (swap-y-for-x (token) (collateral) (expiry) (the-vault) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between the two tokens in a pool. With given dy, find the corresponding amount for swapping to dx using weighted equation. Then conduct swapping by transferring between tx-sender and pool. For the CRP use cases, arbitrageurs uses to gain profit by swapping in the pool.

### **get-x-given-y**

\*\*Prototype: \*\*`(define-read-only (get-x-given-y (token) (collateral) (expiry) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a dx which conforms the weighted equation using dy. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given dy.

### \*\*\*\*

### **get-y-given-x**

\*\*Prototype: \*\*`(define-read-only (get-x-given-y (token) (collateral) (expiry) (dx))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a dy which conforms the weighted equation using dx. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dy that corresponds to given dx.

### \*\*\*\*

### **set-fee-to-address**

\*\*Prototype: \*\*`(define-public (set-fee-to-address (token) (collateral) (expiry) (address))`

**Input:** `<ft-trait>, <ft-trait>, uint, principal`

**Output:** `bool | uint`

Set the platform fee collector address of given pool. Platform fee gathered on each pool action will be collected to the given address after execution.

### **get-fee-to-address**

\*\*Prototype: \*\*`(define-read-only (get-fee-to-address (token) (collateral) (expiry))`

**Input:** `<ft-trait>, <ft-trait>, uint`

**Output:** `bool | uint`

Get the platform fee collector address which is currently set on the given pool.

### **get-fees**

\*\*Prototype: \*\*`(define-read-only (get-fees (token) (collateral) (expiry))`

**Input:** `<ft-trait>, <ft-trait>, uint`

**Output:** `bool | uint`

Get the current platform fee of given pool.

### collect-fees

\*\*Prototype: \*\*`(define-public (collect-fees (token) (collateral) (expiry))`

**Input:** `<ft-trait>, <ft-trait>, uint`

**Output:** `bool | uint`

Collect the platform fee from the collector address which is set on the given pool. The balance of each token in collector address will be initialized back to zero.

### get-token-given-position

\*\*Prototype: \*\*`(define-read-only (get-token-given-position (token) (collateral) (expiry) (dx) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint, uint`

**Output:** `bool | uint`

Returns a token which conforms the weighted equation using dx and dy. This API is used to maintain the pool ratio using the equation and use to get token.

### get-x-given-price

\*\*Prototype: \*\*`(define-read-only (get-x-given-price (token) (collateral) (expiry) (price))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a dx which conforms the weighted equation using price. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given price.

### get-position-given-mint

\*\*Prototype: \*\*`(define-read-only (get-position-given-mint (token) (collateral) (expiry) (shares))`

**Input:**`<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a position which conforms the weighted equation using amount of minted token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token mint. Mainly wraps the weighted equation of 'get-position-given-mint' for the easy usage in smart contract.

### get-position-given-burn

\*\*Prototype: \*\*`(define-read-only (get-position-given-burn (token) (collateral) (expiry) (shares))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint`

**Output:** `bool | uint`

Returns a position which conforms the weighted equation using amount of burnt token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token burn. Mainly wraps the weighted equation of 'get-position-given-burn' for the easy usage in smart contract.

##

## Yield Token Pool <a href="#fixed-weight-pool" id="fixed-weight-pool"></a>

Yield Token Pool is the main pool which all the entities (Arbitrageurs, Borrower, Lender and LP) exerts their own influence. On swapping in yield token pool, users should pay the platform fee which to the pre-designated address. Each pair of tokens have its own unique yield token pool contract. For details please check our [whitepaper](https://docs.alexgo.io/whitepaper/automated-market-making-of-alex).

### **get-max-expiry**

**Prototype:**

`(define-read-only (get-max-expiry))`

**Input:** `void`

**Output:** `bool | uint`

Get the maximum expiry of current Yield Token Pool

### **get-t**

**Prototype:**

`(define-read-only (get-t (expiry)))`

**Input:** `uint`

**Output:** `bool | uint`

Get the computed value of given expiry for the usage in yield token equation

### **get-pool-count**

**Prototype:**

`(define-read-only (get-pool-count))`

**Input:** `void`

**Output:** `bool | uint`

Get the number of currently existing pools.

### **get-pools**

**Prototype:**

`(define-read-only (get-pools))`

**Input:** `void`

**Output:** `bool | uint`

Get the list of currently existing yield token pools.

### **get-pool-details**

**Prototype:**

`(define-read-only (get-pool-details (token-x-trait))`

**Input:** `<yield-token-trait>`

**Output:** `bool | uint`

Gets the details of yield token pool which matches the given token trait. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### **get-pool-contracts**

**Prototype:**

`(define-read-only (get-pool-contracts (pool-id))`

**Input:** `uint`

**Output:** `bool | uint`

Gets the details of yield token pool using pool id. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### \*\*create-pool \*\*

**Prototype:**

`(define-public (create-pool (token-x-trait) (token-y-trait) (the-pool-token) (the-vault) (dx) (dy))`

**Input:** `<yield-token-trait>, <ft-trait>, <pool-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Create a yield-token pool using the given parameter. Created pool can be reached through the unique id which is allocated through this function. dx and dy is initially added to the created pool.

### **add-to-position**

**Prototype:** `(define-public (add-to-position (token-x-trait)(the-pool-token) (the-vault) (dx) (dy))`

**Input:** `<yield-token-trait>, <ft-trait>, <pool-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Add Liquidity to the given pool. First retrieve the existing pool with token traits, then add liquidity of x and y to the specified pool. Adding the liquidity to the pool should conform with equation. Liquidity Provider receives pool token as a reward, which represents the ownership of the liquidity provided pool.

### \*\*\*\*

### **reduce-position**

**Prototype:** `(define-public (reduce-position (token-x-trait) (token-y-trait), (the-pool-token) (the-vault) (percent))`

**Input:** `<yield-token-trait>, <ft-trait>, <pool-token-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Remove Liquidity from the given pool. First retrieve the existing pool with token traits, then remove liquidity of x and y to the specified pool using the requested user (tx-sender)'s pool token. Removing the liquidity to the pool should conform with equation. Liquidity Provider receives corresponding dx and dy to his own vault while the used pool token is burnt.

### \*\*\*\*

### **swap-x-for-y**

**Prototype:**`(define-public (swap-x-for-y (token-x-trait) (token-y-trait) (the-vault) (dx))`

**Input:** `<yield-token-trait>, <ft-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between yield token and target token in a pool. With given dx, find the corresponding amount for swapping to dy using equation. Then conduct swapping by transferring between tx-sender and pool.

### **swap-y-for-x**

**Prototype:**`(define-public (swap-y-for-x (token-x-trait) (token-y-trait) (the-vault) (dy))`

**Input:** `<yield-token-trait>, <ft-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between yield token and target token in pool. With given dy, find the corresponding amount for swapping to dx using equation. Then conduct swapping by transferring between tx-sender and pool.

### **get-x-given-y**

\*\*Prototype: \*\*`(define-read-only (get-x-given-y (token-x-trait) (dy))`

**Input:** `<yield-token-trait> uint`

**Output:** `bool | uint`

Returns a dx which conforms the weighted equation using dy. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given dy.

### \*\*\*\*

### **get-y-given-x**

\*\*Prototype: \*\*`define-read-only (get-y-given-x (token-x-trait) (dx))`

**Input:** `<yield-token-trait>, uint`

**Output:** `bool | uint`

Returns a dy which conforms the weighted equation using dx. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dy that corresponds to given dx.

### \*\*\*\*

### **set-fee-to-address**

\*\*Prototype: \*\*`(define-public (set-fee-to-address (token-x-trait) (address))`

**Input:** `<yield-token-trait>, principal`

**Output:** `bool | uint`

Set the platform fee collector address of given yield token pool. Platform fee gathered on each pool action will be collected to the given address after execution.

### **get-fee-to-address**

\*\*Prototype: \*\*`(define-read-only (get-fee-to-address (token-x-trait))`

**Input:** `<yield-token-trait>`

**Output:** `bool | uint`

Get the platform fee collector address which is currently set on the yield token pool.

### **get-fees**

\*\*Prototype: \*\*`(define-read-only (get-fees (token-x-trait))`

**Input:** `<yield-token-trait>`

**Output:** `bool | uint`

Get the current platform fee of given yield token pool.

### collect-fees

\*\*Prototype: \*\*`(define-public (collect-fees (token-x-trait) (token-xy-trait))`

**Input:** `<yield-token-trait>, <ft-trait>`

**Output:** `bool | uint`

Collect the platform fee from the collector address which is set on the given pool. The balance of each token in collector address will be initialized back to zero.

### get-token-given-position

\*\*Prototype: \*\*`(define-read-only (get-token-given-position (token-x-trait) (dx))`

**Input:** `<yield-token-trait>, uint`

**Output:** `bool | uint`

Returns a token which conforms the weighted equation using dx and yield token. This API is used to maintain the pool ratio using the yield token equation and use to get token.

### get-x-given-price

\*\*Prototype: \*\*`(define-read-only (get-x-given-price (token-x-trait) (price))`

**Input:** `<yield-token-trait>, uint`

**Output:** `bool | uint`

Returns a dx which conforms the yield token equation using price. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given price.

### get-position-given-mint

\*\*Prototype: \*\*`(define-read-only (get-position-given-mint (token-x-trait) (shares))`

**Input:**`<yield-token-trait>, uint`

**Output:** `bool | uint`

Returns a position which conforms the yield token equation using amount of minted token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token mint. Mainly wraps the yield token equation of 'get-position-given-mint' for the easy usage in smart contract.

### get-position-given-burn

\*\*Prototype: \*\*`(define-read-only (get-position-given-burn (get-position-given-mint (token-x-trait) (shares))`

**Input:** `<yield-token-trait>, uint`

**Output:** `bool | uint`

Returns a position which conforms the yield token equation using amount of burnt token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token burn. Mainly wraps the yield token equation of 'get-position-given-burn' for the easy usage in smart contract.

## Liquidity Bootstrapping Pool

Liquidity Bootstrapping pool is used for initializing all Yield Token Pools, which makes ayToken as a base token and Token to become a target Token. Detailed use cases and explanation can be ion our [White Paper](../../../protocol/liquidity-bootstrapping-pool.md).

### **get-pool-count**

**Prototype:**

`(define-read-only (get-pool-count))`

**Input:** `void`

**Output:** `bool | uint`

Get the number of currently existing pools.

### **get-pools**

**Prototype:**

`(define-read-only (get-pools))`

**Input:** `void`

**Output:** `bool | uint`

Get the list of currently existing pools.

### **get-pool-details**

**Prototype:**

`(define-read-only (get-pool-details (token-x-trait) (token-y-trait) (expiry))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Gets the details of pool which matches the given parameter. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### **get-pool-contracts**

**Prototype:**

`(define-read-only (get-pool-contracts (pool-id))`

**Input:** `uint`

**Output:** `bool | uint`

Gets the details of pool using pool id. Notice that pools are predefined map data structure, using token traits and weights as key and various detailed attributes as values.

### **get-weight-x**

**Prototype:**

`(define-read-only (get-weight-x (token-x-trait) (token-y-trait) (expiry))`

**Input:** `<ft-trait> <ft-trait> uint`

**Output:** `bool | uint`

Gets the parameter corresponding weight through the internal calculation. Weight retrieving formula is implemented inside this API.

### \*\*create-pool \*\*

**Prototype:**

`(define-public (create-pool (token-x-trait) (token-y-trait) (weight-x-0) (weight-x-1) (expiry) (the-pool-token) (the-vault) (dx) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint, uint, <pool-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Creates a pool using the given parameter. Created pool can be reached through the unique id which is allocated through this function. dx and dy is initially added to the created pool.

### **add-to-position**

**Prototype:** `(define-public (add-to-position (token-x-trait) (token-y-trait) (expiry) (the-pool-token) (the-vault) (dx) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, <pool-token-trait>, <vault-trait>, uint, uint`

**Output:** `bool | uint`

Adds Liquidity to the given pool. First retrieve the existing pool with token traits, then add liquidity of x and y to the specified pool. Adding the liquidity to the pool should conform with equation. Liquidity Provider receives ayToken as a reward, which represents the ownership of the liquidity provided pool.

### \*\*\*\*

### **reduce-position**

**Prototype:** `(define-public (reduce-position (token-x-trait) (token-y-trait) (expiry) (the-pool-token) (the-vault) (percent))`

**Input:** `<ft-trait>, <ft-trait>, uint, <pool-token-trait>, <vault-trait>, uint`

**Output:** `bool | uint`

Remove Liquidity from the given pool. First retrieve the existing pool with token traits, then remove liquidity of x and y to the specified pool using the requested user (tx-sender)'s pool token. Removing the liquidity to the pool should conform with equation. Liquidity Provider receives corresponding dx and dy to his own vault while the used pool token is burnt.

### \*\*\*\*

### **swap-x-for-y**

**Prototype:**`(define-public (swap-x-for-y (token-x-trait) (token-y-trait) (expiry) (the-vault) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between the two tokens in a pool. With given dx, find the corresponding amount for swapping to dy using weighted equation. Then conduct swapping by transferring between tx-sender and pool.

### **swap-y-for-x**

**Prototype:**`(define-public (swap-y-for-x (token-x-trait) (token-y-trait) (expiry) (the-vault) (dx))`

**Input:** `<ft-trait>, <ft-trait>, uint, <vault-trait>, uint`

**Output:** `bool | uint`

Allows swapping between the two tokens in a pool. With given dy, find the corresponding amount for swapping to dx using weighted equation. Then conduct swapping by transferring between tx-sender and pool.

### **collect-fees**

**Prototype:**`(define-public (collect-fees (token-x-trait) (token-y-trait) (expiry))`

**Input:** `<ft-trait>, <ft-trait>, uint`

**Output:** `bool | uint`

Collects the accrued platform fee. Transaction occurs from pool to designated fee-to-address principal. As a result, platform fees collected in pool (fee-balance) is reseted to zero.

### \*\*\*\*

### **get-x-given-y**

\*\*Prototype: \*\*`(define-read-only (get-x-given-y (token-x-trait) (token-y-trait) (expiry) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a dx which conforms the weighted equation using dy. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given dy.

### \*\*\*\*

### **get-y-given-x**

\*\*Prototype: \*\*`(define-read-only (get-x-given-y (token-x-trait) (token-y-trait) (expiry) (dx))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a dy which conforms the weighted equation using dx. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dy that corresponds to given dx.

### \*\*\*\*

### **set-fee-to-address**

\*\*Prototype: \*\*`(define-public (set-fee-to-address (token-x-trait) (token-y-trait) (expiry) (address))`

**Input:** `<ft-trait>, <ft-trait>, uint, principal`

**Output:** `bool | uint`

Set the platform fee collector address of given pool. Platform fee gathered on each pool action will be collected to the given address after execution.

### **get-fee-to-address**

\*\*Prototype: \*\*`(define-read-only (get-fee-to-address (token-x-trait) (token-y-trait) (expiry))`

**Input:** `<ft-trait>, <ft-trait>, uint`

**Output:** `bool | uint`

Get the platform fee collector address which is currently set on the given pool.

### **get-fees**

\*\*Prototype: \*\*`(define-read-only (get-fees (token-x-trait) (token-y-trait) (expiry))`

**Input:** `<ft-trait>, <ft-trait>, uint`

**Output:** `bool | uint`

Get the current platform fee of given pool.

### get-token-given-position

\*\*Prototype: \*\*`(define-read-only (get-token-given-position (token-x-trait) (token-y-trait) (expiry) (dx) (dy))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint, uint`

**Output:** `bool | uint`

Returns a token which conforms the weighted equation using dx and dy. This API is used to maintain the pool ratio using the equation and use to get token.

### get-x-given-price

\*\*Prototype: \*\*`(define-read-only (get-x-given-price (token-x-trait) (token-y-trait) (expiry) (price))`

**Input:** `<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a dx which conforms the weighted equation using price. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx that corresponds to given price.

### get-position-given-mint

\*\*Prototype: \*\*`(define-read-only (get-position-given-mint (token-x-trait) (token-y-trait) (expiry) (shares))`

**Input:**`<ft-trait>, <ft-trait>, uint, uint`

**Output:** `bool | uint`

Returns a position which conforms the weighted equation using amount of minted token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token mint. Mainly wraps the weighted equation of 'get-position-given-mint' for the easy usage in smart contract.

### get-position-given-burn

\*\*Prototype: \*\*`(define-read-only (get-position-given-burn (token-x-trait) (token-y-trait) (expiry) (shares))`

**Input:** `<token-x-trait>, <token-y-trait>, uint, uint`

**Output:** `bool | uint`

Returns a position which conforms the weighted equation using amount of burnt token. This API is used to maintain the pool ratio using the equation and use to get an adequate amount of dx and dy that corresponds to given token burn. Mainly wraps the weighted equation of 'get-position-given-burn' for the easy usage in smart contract.

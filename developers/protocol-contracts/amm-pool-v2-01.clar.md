# Pool

#### Location: [`alex-dao-2/contracts/extensions/amm-pool-v2-01.clar`](https://github.com/alexgo-io/alex-dao-2/blob/main/contracts/extensions/amm-pool-v2-01.clar)

This document provides comprehensive technical documentation for the primary contract in ALEX's AMM Trading Pool system. The contract encompasses several core operations, including pool creation, liquidity operations (adding or removing assets), LP token management (minting and burning tokens that represent a user's share of the pool and potential earnings), and token swapping (facilitating the exchange of tokens within an existing and funded pool while charging a corresponding fee). This contract is complemented by two auxiliary contracts: a REGISTRY contract that handles the persistence of pool information, and a VAULT contract that secures the assets and manages the reserves accumulated from the fees. For detailed information about these auxiliary contracts, please refer to their respective technical documentation: [amm-registry-v2-01.clar](amm-registry-v2-01.clar.md) and [amm-vault-v2-01.clar](amm-vault-v2-01.clar.md).

## Storage

### Variables: (data-var)

* `paused` (bool) This data variable acts as a flag to determine and control the operational status of the contract within the system. When set to 'paused,' it will block all position and swap transactions.

### Constants

* `x_a_list_no_deci`
* `x_a_list`

#### Mathematical constants

These symbolic constants are employed to define and restrict decimal precision and boundary limits in calculations within the system.

* `ONE_8`
* `UNSIGNED_ONE_8`
* `MAX_NATURAL_EXPONENT`
* `MIN_NATURAL_EXPONENT`
* `MILD_EXPONENT_BOUND`
* `MAX_POW_RELATIVE_ERROR`

## Contract calls (interactions)

* `executor-dao` Calls are made to verify whether a certain contract-caller is designated as an extension.
* `amm-registry-v2-01` This contract is called to manage and configure the data and settings of AMM pools. In this document, it is referred to as the 'registry'.
* `amm-vault-v2-01` This contract is called to execute token transfers during the reduction of positions and swapping operations. Additionally, calls are made to add fees charged to the reserves.
* `token-amm-pool-v2-01` In operations to add or reduce positions, interactions with this contract involve minting and burning of LP (Liquidity Provider) Tokens and retrieving their balance amounts. In this document, it is referred to as the 'LP Token'.
* Tokens (`token-x-trait`, `token-y-trait`, `token-z-trait`, etc.) During the process of adding positions and executing swaps, a trait is used to invoke the relevant tokens to perform the necessary transfers involved in the transaction. This trait is a customized version of the Stacks' standard definition for Fungible Tokens (`sip-010`), with support for 8-digit fixed notation.

## Features

### POOL features

1. `create-pool` This function establishes a liquidity pool for a specified token pair (token-x/token-y). It starts by verifying that the `tx-sender` is not blacklisted through the [`is-blocklisted-or-default` function](amm-pool-v2-01.clar.md#governance-features). Following this validation, the function delegates the pool creation task to the registry. The pools are uniquely identified by their `token-x`, `token-y`, and `factor` values. Upon successful creation, the function automatically invokes `add-to-position` function to initialize token positions for the specified pair within the new pool.\
   **Input**:

```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(factor uint)
(pool-owner principal)
(dx uint)
(dy uint)
```

2. `add-to-position` The `add-to-position` function adds asset positions to an existing liquidity pool by transferring the respective tokens from the `tx-sender` to the system vault contract. It updates the pool details in the registry contract and mints LP tokens to the sender, representing their share of the pool and potential earnings. This minting operation is tracked using a unique pool identifier (POOL\_ID).\
   **Input**:

```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(factor uint)
(dx uint)
(max-dy (optional uint))
```

3. `reduce-position` This function performs the inverse operation of `add-to-position` by allowing users to withdraw asset positions from an existing token pair pool. Upon meeting all requirements (such as the operational status of the pool contract, a valid percentage, and sufficient liquidity pool supply), the function transfers the calculated amount from the vault to the sender and burns the corresponding LP tokens. Note that this function cannot be invoked by blacklisted senders.\
   **Input**:

```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(factor uint)
(percent uint)
```

4. **Swap tokens functions**: The swap tokens functions enable token exchanges between two given tokens within the liquidity pool. For a swap to successfully execute, the pool contract must contain sufficient liquidity for the specified token pair. The basic steps are: a. transferring a specified amount of token-x from the sender to the system vault b. transferring a calculated amount of token-y from the system vault to the sender while considering swap fees and expected minimum amounts c. registering the fee in token-x within the vault d. updating the pool registry's liquidity status\
   \
   These steps are mirrored for swaps involving the reverse token pair (token-y/token-x).\
   \
   If no direct pool exists for the desired pair, the contract provides helper functions to facilitate multi-hop swaps using intermediate token pools. This process, also known as multi-step swap, allows the exchange via a route like token-x/intermediate-token and intermediate-token/token-y. Note that this feature requires prior knowledge of the intermediary pools to connect the desired pair in the swap.\
   \
   The current protocol version supports up to 4-pools-route operations which are implemented in the following functions:

* `swap-helper` Swaps a given token-x for a required token-y. 1 pool route: token-x/token-y.\
  **Input**:

```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(factor uint)
(dx uint)
(min-dy (optional uint))
```

* `swap-helper-a` Swaps a given token-x for a required token-z. 2 pools route: token-x/token-y - token-y/token-z.\
  **Input**:

```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(token-z-trait <ft-trait>)
(factor-x uint)
(factor-y uint)
(dx uint)
(min-dz (optional uint))
```

* `swap-helper-b` Swaps a given token-x for a required token-w. 3 pools route: token-x/token-y - token-y/token-z - token-z/token-w.\
  **Input**:

```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(token-z-trait <ft-trait>)
(token-w-trait <ft-trait>)
(factor-x uint)
(factor-y uint)
(factor-z uint)
(dx uint)
(min-dw (optional uint))
```

* `swap-helper-c` Swaps a given token-x for a required token-v. 4 pools route: token-x/token-y - token-y/token-z - token-z/token-w - token-w/token-v.\
  **Input**:

```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(token-z-trait <ft-trait>)
(token-w-trait <ft-trait>)
(token-v-trait <ft-trait>)
(factor-x uint)
(factor-y uint)
(factor-z uint)
(factor-w uint)
(dx uint)
(min-dv (optional uint))
```

**Note**: all these helpers use the swap supporting functions `swap-x-for-y` and `swap-y-for-x`.

### Supporting features

The following functions are tools to assist the off-chain activities.

1. Fee helpers (`fee-helper`, `fee-helper-a`, `fee-helper-b`, `fee-helper-c`) These functions retrieve current fees for an existing pool based on the specified tokens and factors.
2. Rate helpers (`get-helper`, `get-helper-a`, `get-helper-b`, `get-helper-c`) These functions retrieve exchange rates for a given amount in an existing pool based on the specified tokens and factors.

**Note**: The above functions, along with their variations, support intermediary routes similar to those in the swapping functions (e.g., token-x/token-y, token-x/token-y-token-y/token-z, etc.).

### Governance features

1. `is-dao-or-extension` This standard protocol function checks whether a caller (`tx-sender`) is the DAO executor or an authorized extension, delegating the extensions check to the `executor-dao` contract.\
   **Input**: None.
2. `is-blocklisted-or-default` A read-only feature that verifies if a given address is blacklisted in the registry contract.\
   **Input**:

```lisp
(sender principal)
```

3. `is-paused` A read-only function that checks the operational status of the contract.\
   **Input**: None.
4. `pause` A public function, governed through the `is-dao-or-extension`, that can change the contract's operational status.\
   **Input**:

```lisp
(new-paused bool)
```

### Getter and Setter functions

All getter and setter functions in the contract handle pool information, delegating their retrieval or update operations to the corresponding functions in the registry contract [amm-registry-v2-01.clar](amm-registry-v2-01.clar.md).

#### Setters

The following is the complete list of setter functions for pool configurations. All set configuration functions are restricted to the respective pool owner or ALEX admin operators (see function `is-dao-or-extension`).

* `set-fee-rate-x` and `set-fee-rate-y`: set the swap fee (% of swap amount) of `token-x` and `token-y`, respectively. Both `fee-rate-x` and `fee-rate-y` are zero by default.

* `set-start-block` and `set-end-block`: set the block heights before and after, respectively, which the pool is not available. Both `start-block` and `end-block` is set to `u340282366920938463463374607431768211455` by default.

* `set-threshold-x` and `set-threshold-y`: set the amount of `token-x` and `token-y`, respectively, below which a minimum % slippage is applied. Both `threshold-x` and `threshold-y` are zero by default.

* `set-max-in-ratio` and `set-max-out-ratio`: set the maximum ratio values used to calculate the highest amount that can be deposited (IN) or exchanged (OUT) within the pool.

* `set-oracle-enabled`: add or remove the pool from the on-chain price oracle. Oracle is disabled by default.

* `set-oracle-average`: set the exponential moving average factor for the `oracle-resilient`. Please note this call will reset the existing `oracle-resilient` value. The `oracle-average` is zero by default. We recommend `0.99e8`.

#### Getters

**Main getters**

* `get-invariant` (consumes the preceding registry's `get-switch-threshold`)
* `get-max-ratio-limit`
* `get-pool-details`
* `get-pool-details-by-id`
* `get-pool-exists`
* `get-switch-threshold`

**Pool configuration getter functions (query the registry via the aforementioned `get-pool-details` function)**

* `check-pool-status`
* `get-balances`
* `get-start-block`
* `get-end-block`
* `get-fee-rate-x`
* `get-fee-rate-y`
* `get-fee-rebate`
* `get-max-in-ratio`
* `get-max-out-ratio`
* `get-oracle-average`
* `get-oracle-enabled`
* `get-oracle-resilient`
* `get-oracle-instant`
* `get-pool-owner`
* `get-price`
* `get-threshold-x`
* `get-threshold-y`

**Pool token information getter functions (query the registry via the aforementioned `get-pool-details` function)**

* `get-x-given-price`
* `get-y-given-price`
* `get-y-given-x`
* `get-x-given-y`
* `get-y-in-given-x-out`
* `get-x-in-given-y-out`
* `get-position-given-mint`
* `get-position-given-burn`
* `get-token-given-position`

#### Internal helper functions

**Token helpers**

These helper functions facilitate the retrieval of specific token data using another known data as input:

* `get-x-given-price-internal`
* `get-y-given-price-internal`
* `get-y-given-x-internal`
* `get-x-given-y-internal`
* `get-y-in-given-x-out-internal`
* `get-x-in-given-y-out-internal`
* `get-position-given-burn-internal`
* `get-position-given-mint-internal`
* `get-price-internal`
* `get-token-given-position-internal`

**Mathematical helpers**

These helper functions aid in various calculations within the context of 
the contract, such as amounts, percentages, etc. Some of these functions rely on predefined constants, as specified in [mathematical constants](amm-pool-v2-01.clar.md#mathematical-constants): `accumulate_division`, `accumulate_product`, `div-down`, `div-up`, `exp-fixed`, `exp-pos`, `ln-fixed`, `ln-priv`, `log-fixed`, `mul-down`, `mul-up`, `pow-down`, `pow-fixed`, `pow-priv`, `pow-up`, `rolling_div_sum`, `rolling_sum_div`.


## Errors defined in the contract

* `ERR-BLOCKLISTED`
* `ERR-EXCEEDS-MAX-SLIPPAGE`
* `ERR-INVALID-EXPONENT`
* `ERR-INVALID-LIQUIDITY`
* `ERR-INVALID-POOL`
* `ERR-MAX-IN-RATIO`
* `ERR-MAX-OUT-RATIO`
* `ERR-NO-LIQUIDITY`
* `ERR-NOT-AUTHORIZED`
* `ERR-ORACLE-AVERAGE-BIGGER-THAN-ONE`
* `ERR-ORACLE-NOT-ENABLED`
* `ERR-OUT-OF-BOUNDS`
* `ERR-PAUSED`
* `ERR-PERCENT-GREATER-THAN-ONE`
* `ERR-POOL-ALREADY-EXISTS`
* `ERR-PRODUCT-OUT-OF-BOUNDS`
* `ERR-SWITCH-THRESHOLD-BIGGER-THAN-ONE`
* `ERR-X-OUT-OF-BOUNDS`
* `ERR-Y-OUT-OF-BOUNDS`

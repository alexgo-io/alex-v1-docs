# amm-pool-v3.clar

- Location: `./contracts/amm-pool-v3.clar`

<!-- - [Deployed contract]() -->

The `amm-pool-v3` contract implements the core logic of DAMM (Discrete Automated Market Maker), a concentrated liquidity protocol designed to maximize capital efficiency by organizing liquidity into discrete price ranges called **bins** or **ticks**.

Unlike traditional AMMs that spread liquidity across the entire price curve, DAMM allows liquidity providers (LPs) to allocate funds into specific price intervals. Each price bin behaves as an independent constant product pool, with a custom virtual balance system to keep swaps bounded within the configured range.

This contract supports the full lifecycle of DAMM pools:
- **Pool Creation** – DAO-controlled creation of new pools between two fungible tokens, specifying bin size and fee configuration.
- **Liquidity Provision** – LPs add liquidity to specific ticks within a pool, receiving fungible LP tokens for each tick.
- **Liquidity Management** – LPs can reduce their positions by percentage, claiming back their share of assets and accrued fees from a specific tick.
- **Swapping** – Supports swaps of token X for token Y (and vice versa) within a tick.
- **Administrative Control** – The ALEX DAO can pause or sunset pools and adjust fee parameters at any time.
- **Fee Claiming** – Accumulated fees can be withdrawn by the DAO to a target address.

## Features

### Public

#### `add-to-position`

This function allows a user to add liquidity to a specific price bin (tick) within a DAMM pool. Unlike traditional AMMs where liquidity is distributed evenly across all prices, here the user targets a defined price range. The `min-price` and `max-price` parameters act as a safety check, even if the user selects a bin, they might not want to enter at just any price inside that range. These bounds let users specify a narrower range of acceptable prices for their deposit. If the actual price (after adding liquidity) falls outside of this min-max window, the transaction will revert to protect the user from entering at an unfavorable rate. The function accepts a max amount of both tokens (`dx`, `dy`) and bounds on the acceptable price range (`min-price`, `max-price`).

It internally calculates the actual amounts of each token that can be added given the pool's current balances and fee configuration. If the price after the addition falls outside the user-defined bounds, the transaction reverts.

If this is the first liquidity added to the tick, it sets the virtual balances and initializes the price bin. Otherwise, it uses the existing balances to calculate a fair proportional LP minting.

This function interacts with the `amm-liquidity-token-v3` contract to mint LP tokens for the position, and with the respective SIP-010 token contracts to transfer the user's tokens into the pool. All funds are held directly by the `amm-pool-v3 contract`, there is no external vault.

##### Parameters

| Name            | Type         |
|-----------------|--------------|
| `token-x-trait` | `<ft-trait>` |
| `token-y-trait` | `<ft-trait>` |
| `pool-id`       | `uint`       |
| `tick`          | `int`        |
| `dx`            | `uint`       |
| `dy`            | `uint`       |
| `min-price`     | `uint`       |
| `max-price`     | `uint`       |


#### `reduce-position`

This function allows a user to reduce their liquidity position in a specific tick of a DAMM pool by a given percentage. Internally, it calculates the user's LP token balance for the selected bin, burns the requested portion, and sends back the proportional share of token X and token Y to the user.

The function interacts with the `amm-liquidity-token-v3` contract to burn LP tokens, and with the respective SIP-010 token contracts to transfer assets back to the user. All liquidity is held directly by the `amm-pool-v3` contract, there is no external vault mechanism involved.

The `percent` parameter represents the portion of the position to withdraw, using fixed-point math with 8 decimal precision. For example:

- `100_000_000` means 100% (full withdrawal)  
- `50_000_000` means 50%  
- `10_000_000` means 10%

##### Parameters

| Name            | Type         |
|-----------------|--------------|
| `token-x-trait` | `<ft-trait>` |
| `token-y-trait` | `<ft-trait>` |
| `pool-id`       | `uint`       |
| `tick`          | `int`        |
| `percent`       | `uint`       |

#### `swap-x-for-y-ioc`

This function allows users to swap a specific amount of token X (`dx`) for token Y in a selected bin (`tick`) of a DAMM pool, using an **Immediate-Or-Cancel (IOC)** strategy. The swap aims to provide the best possible rate for token Y. The effective upper price limit for the swap is the more restrictive of either the user-defined `max-price` or the natural upper price boundary (`price-end`) of the specified tick. If the requested `dx` would push the price beyond this effective cap, only a partial amount (or none) of `dx` will be swapped.

The function validates the pool's status and token traits, calculates how much of token X can be swapped without breaching the price cap, applies fees and potential rebates, and executes the transfers. If conditions are not met (e.g. price exceeds `max-price` or insufficient liquidity), it returns zero values for all outputs.

##### Parameters

| Name             | Type        |
|------------------|-------------|
| `token-x-trait`  | `<ft-trait>` |
| `token-y-trait`  | `<ft-trait>` |
| `pool-id`        | `uint`       |
| `tick`           | `int`        |
| `dx`             | `uint`       |
| `max-price`      | `uint`       |

#### `swap-y-for-x-ioc`

This function is the reverse of `swap-x-for-y-ioc`, the execution logic is similar. The effective lower price limit for the swap is the less restrictive of either the user-defined `min-price` or the natural lower price boundary (`price-start`) of the specified tick. If the requested `dy` would push the price below this effective cap, only a partial amount (or none) will be swapped. If conditions are not met, it returns zero values for all outputs.

##### Parameters

| Name            | Type         |
|-----------------|--------------|
| `token-x-trait` | `<ft-trait>` |
| `token-y-trait` | `<ft-trait>` |
| `pool-id`       | `uint`       |
| `tick`          | `int`        |
| `dy`            | `uint`       |
| `min-price`     | `uint`       |

### Governance Features

The following functions are guarded by the `is-dao-or-extension` function. This implies that only the ALEX DAO or an enabled extension can use these features.

#### `is-dao-or-extension`

Standard protocol function to check whether the `contract-caller` is an enabled extension within the DAO or the `tx-sender` is the DAO itself. The extension check is delegated to the `executor-dao` contract.

#### `create-pool`

Creates a new DAMM pool between two SIP-010 fungible tokens.

This function configures a pool with a specific `bin-size`, fee rates for each token, and a fee rebate percentage. It performs several validations before pool creation:

- Ensures that both tokens are different and verifies the `bin-size` is one of the allowed values (`u1`, `u5`, `u10`, `u20`).
- Checks that a pool with the same token pair (`token-x`, `token-y`) and `bin-size` does not already exist. It also verifies that a pool with the reversed token pair (`token-y`, `token-x`) and the same `bin-size` is not already registered.
- Validates that `fee-rate-x` and `fee-rate-y` are strictly less than 1e8 (`ONE_8`) and that `fee-rebate` does not exceed 100%.

The pool is stored using a new `pool-id` generated by incrementing `pool-id-nonce`, and it is inserted into both the `pools` map and the `pool-id-by-token` index for lookup by token pair. The pool starts in an active state (not paused or sunset), and the owner is set to the supplied `pool-owner`. Finally, it emits an event with the pool ID, the pool parameters, and the transaction sender.

##### Parameters

| Name           | Type         |
| -------------- | ------------ |
| `token-x-trait`| `<ft-trait>` |
| `token-y-trait`| `<ft-trait>` |
| `bin-size`     | `uint`       |
| `pool-owner`   | `principal`  |
| `fee-rate-x`   | `uint`       |
| `fee-rate-y`   | `uint`       |
| `fee-rebate`   | `uint`       |

#### `update-pool-fee`

Updates the fee configuration for an existing pool. It sets new values for the swap fee on each token and the rebate returned to liquidity providers. The function also validates that the new fee values for token X and token Y are both less than `ONE_8` (i.e. below 100%). Finally, it emits an event with the pool ID, the updated fees, the fee rebate, and the transaction sender.

##### Parameters

| Name         | Type   |
| ------------ | ------ |
| `pool-id`    | `uint` |
| `fee-rate-x` | `uint` |
| `fee-rate-y` | `uint` |
| `fee-rebate` | `uint` |

#### `set-pool-status`

Updates the operational status of a pool. It can mark a pool as `paused` (temporarily disabling activity) or `sunset` (permanently deactivating the pool), affecting its availability for swaps and actions like adding or removing liquidity. Finally, it emits an event with the pool ID, the deactivation and pause status, and the transaction sender.

##### Parameters

| Name      | Type   |
| --------- | ------ |
| `pool-id` | `uint` |
| `sunset`  | `bool` |
| `paused`  | `bool` |

#### `claim-fees`

Allows the DAO to withdraw accumulated swap fees from a specific pool and tick. The function resets the stored fees to zero and transfers the collected amounts of both tokens to the specified address using the corresponding token contracts.

##### Parameters

| Name              | Type        |
| ----------------- | ----------- |
| `token-x-trait`   | `<ft-trait>` |
| `token-y-trait`   | `<ft-trait>` |
| `pool-id`         | `uint`      |
| `tick`            | `int`       |
| `fee-to-address`  | `principal` |

### Getters

#### `get-pool-count`

#### `get-pool-id`

##### Parameters

| Name | Type |
|------|------|
| `key` | `{ token-x: principal, token-y: principal, bin-size: uint }` |

#### `get-pool`

##### Parameters

| Name | Type |
|------|------|
| `pool-id` | `uint` |

#### `get-pool-balances`

##### Parameters

| Name | Type |
|------|------|
| `pool-id` | `uint` |
| `tick`    | `int`  |

#### `get-liquidity-token-id`

##### Parameters

| Name | Type |
|------|------|
| `pool-id` | `uint` |
| `tick`    | `int`  |

#### `parse-liquidity-token-id`

##### Parameters

| Name | Type |
|------|------|
| `token-id` | `uint` |

#### `tick-to-price`

##### Parameters

| Name | Type |
|------|------|
| `bin-size` | `uint` |
| `tick`     | `int`  |

#### `get-price-bounds`

##### Parameters

| Name | Type |
|------|------|
| `bin-size` | `uint` |
| `tick`     | `int`  |

#### `get-virtual-balances`

##### Parameters

| Name        | Type |
|-------------|------|
| `bin-size`  | `uint` |
| `tick`      | `int`  |
| `balance-x` | `uint` |
| `balance-y` | `uint` |

## Storage

### `pool-id-nonce`

| Data     | Type |
|----------|------|
| Variable | `uint` |

A global counter used to assign unique identifiers to new pools. It is incremented each time a pool is created through the `create-pool` function.

### `pools`

| Data | Type |
|------|------|
| Map  | `uint => { token-x: principal, token-y: principal, bin-size: uint, pool-owner: principal, fee-rate-x: uint, fee-rate-y: uint, fee-rebate: uint, sunset: bool, paused: bool }` |

Stores configuration data for each pool, indexed by `pool-id`. It includes the token pair, bin size, fee parameters, and status flags (`paused` and `sunset`). This map is used to validate pool existence and retrieve parameters during pool creation, swaps, and liquidity operations.

### `pool-id-by-token`

| Data | Type |
|------|------|
| Map  | `{ token-x: principal, token-y: principal, bin-size: uint } => uint` |

Maps a token pair and bin size to its corresponding `pool-id`. This provides a way to retrieve an existing pool’s ID based on its asset configuration.

### `pool-supply`

| Data | Type |
|------|------|
| Map  | `{ pool-id: uint, tick: int } => { total-supply: uint, balance-x: uint, balance-y: uint, fee-x: uint, fee-y: uint }` |

Stores the state of each bin in a pool, indexed by `pool-id` and `tick`. It tracks the total LP token supply, token balances, and accumulated fees for that specific price range within the pool.

## Contract calls

- `executor-dao`: called by governance functions (e.g., `create-pool`, `update-pool-fee`) via `is-dao-or-extension` to verify if the `contract-caller` is an authorized extension or if `tx-sender` is the DAO itself.
- `amm-liquidity-token-v3`: used to mint and burn LP tokens that represent user positions in specific ticks.
- `token-x-trait` / `token-y-trait`: represent the fungible tokens involved in each pool. These trait contracts are used to perform transfers during swaps and liquidity updates.

## Errors

| Error Name                              | Value         |
| --------------------------------------- | ------------- |
| `ERR-NOT-AUTHORIZED`                    | `(err u1000)` |
| `ERR-POOL-PAUSED`                       | `(err u1001)` |
| `ERR-POOL-SUNSET`                       | `(err u1002)` |
| `ERR-POOL-NOT-FOUND`                    | `(err u1003)` |
| `ERR-INVALID-PAIR`                      | `(err u1004)` |
| `ERR-POOL-ALREADY-EXISTS`              | `(err u2000)` |
| `ERR-INVALID-PRICE`                     | `(err u2001)` |
| `ERR-INVALID-TOKEN-TRAIT`              | `(err u2002)` |
| `ERR-INVALID-BIN-SIZE`                 | `(err u2003)` |
| `ERR-PERCENT-GREATER-THAN-ONE`         | `(err u2004)` |
| `ERR-ZERO-PERCENT`                      | `(err u2005)` |
| `ERR-INVALID-AMOUNT`                    | `(err u2006)` |
| `ERR-POSITION-NOT-FOUND`               | `(err u2007)` |
| `ERR-PERCENT-GREATER-THAN-OR-EQUALS-ONE` | `(err u2008)` |
| `ERR-INSUFFICIENT-LIQUIDITY`           | `(err u2009)` |
| `ERR-STORAGE-FAILURE`                  | `(err u3001)` |
| `ERR-INVALID-LP-STATE`                 | `(err u3002)` |
| `ERR-MAX-POOLS-ALLOWED`                | `(err u3003)` |
| `ERR-TICK-OUT-OF-RANGE`                | `(err u3004)` |

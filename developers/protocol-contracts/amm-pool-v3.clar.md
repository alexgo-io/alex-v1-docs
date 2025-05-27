# amm-pool-v3.clar

<!-- - [Deployed contract]() -->

The `amm-pool-v3` contract implements the core logic of DAMM (Discrete Automated Market Maker), a concentrated liquidity protocol designed to maximize capital efficiency by organizing liquidity into discrete price ranges called **bins** or **ticks**.

Unlike traditional AMMs that spread liquidity across the entire price curve, DAMM allows liquidity providers (LPs) to allocate funds into specific price intervals. Each price bin behaves as an independent constant product pool, with a custom virtual balance system to keep swaps bounded within the configured range.

This contract supports the full lifecycle of DAMM pools:
- **Pool Creation** – DAO-controlled creation of new pools between two fungible tokens, specifying bin size and fee configuration.
- **Liquidity Provision** – LPs add liquidity to specific ticks within a pool, receiving fungible LP tokens for each tick.
- **Liquidity Management** – LPs can reduce their positions by percentage, claiming back their share of assets and accrued fees.
- **Swapping** – Supports swaps of token X for token Y (and vice versa) within a tick.
- **Administrative Control** – The DAO can pause or sunset pools and adjust fee parameters at any time.
- **Fee Claiming** – Accumulated fees can be withdrawn by the DAO to a target address.

## Features

### Public

#### `add-to-position`

This function allows a user to add liquidity to a specific price bin (tick) within a DAMM pool. Unlike traditional AMMs where liquidity is distributed evenly across all prices, here the user targets a defined price range. The function accepts a max amount of both tokens (`dx`, `dy`) and bounds on the acceptable price range (`min-price`, `max-price`).

It internally calculates the actual amounts of each token that can be added given the pool's current balances and fee configuration. If the price after the addition falls outside the user-defined bounds, the transaction reverts.

If this is the first liquidity added to the tick, it sets the virtual balances and initializes the price bin. Otherwise, it uses the existing balances to calculate a fair proportional LP minting.

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

The function interacts with the `amm-liquidity-token-v3` contract to burn LP tokens, and with the respective SIP-010 token contracts to transfer assets back to the user. It ensures the position exists, the pool is active, and the reduction is within valid bounds.

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

This function allows users to swap a specific amount of token X (`dx`) for token Y in a selected bin (`tick`) of a DAMM pool, under an Immediate-Or-Cancel (IOC) execution logic. That means the swap will only proceed if the resulting price stays within the user-defined `max-price` limit. If the conditions are not met, the function either cancels the operation or executes the portion of the swap that can be fulfilled without exceeding the price cap.

The function starts by validating the pool's status and confirming that the token traits passed as arguments match the registered pool configuration. It then calculates how much of the input token X can be used in the swap without breaching the price limit. This calculation relies on the virtual balances model used in DAMM, which ensures that trading remains bounded within the configured bin.

Once the maximum amount of input (`actual-dx`) is determined, the contract applies the relevant fee and rebate logic: a percentage of `actual-dx` is taken as a fee, and part of that fee may be rebated back into the pool. The output amount of token Y (`actual-dy`) is then calculated using a modified constant product formula based on updated virtual reserves.

If the calculated swap is valid, the function performs the following actions:
- Transfers `actual-dx` of token X from the user to the pool contract.
- Transfers `actual-dy` of token Y from the pool contract back to the user.
- Updates the internal balances and fee accounting for the bin in storage.

If the swap doesn't meet the required price conditions or if liquidity is insufficient, it returns zeroed amounts, aligning with the IOC semantics.

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

This function executes a swap from token Y to token X within a specific price bin (tick), using the Immediate or Cancel (IOC) strategy. The user specifies a maximum input (`dy`) and a minimum acceptable price (`min-price`), and the function attempts to perform the swap without breaching that price constraint.

Internally, the function uses the virtual balances model to determine how much of token Y can be swapped while keeping the output price within the valid range for the tick. It computes the resulting amount of token X (`dx`) that would be received, applies fees, and then executes the transfers:

- Transfers `dy` from the user to the contract
- Transfers `dx` from the contract to the user
- Updates internal balances, fee records, and emits a print event

If the conditions can't be met, the function returns `0` values for all fields, indicating that the swap was canceled.

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

#### `is-dao-or-extension`

This standard protocol function checks whether a caller (`tx-sender`) is the DAO executor or an authorized extension, delegating the extensions check to the `executor-dao` contract.

#### `create-pool`

A public function, governed through the `is-dao-or-extension`, that creates a new DAMM pool between two SIP-010 fungible tokens.

This function configures a pool with a specific `bin-size`, fee rates for each token, and a fee rebate percentage. It performs several validations before pool creation:

- Ensures that both tokens are different and the bin size is within the allowed list (`ALLOWED-BIN-SIZES`).
- Checks that a pool with the same token pair and bin size does not already exist.
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

A public function, governed through `is-dao-or-extension`, that updates the fee configuration for an existing pool. It sets new values for the swap fee on each token and the rebate returned to liquidity providers. The function also validates that the new fee values for token X and token Y are both less than `ONE_8` (i.e. below 100%). Finally, it emits an event with the pool ID, the updated fees, the fee rebate, and the transaction sender.

##### Parameters

| Name         | Type   |
| ------------ | ------ |
| `pool-id`    | `uint` |
| `fee-rate-x` | `uint` |
| `fee-rate-y` | `uint` |
| `fee-rebate` | `uint` |

#### `set-pool-status`

A public function, governed through `is-dao-or-extension`, that updates the operational status of a pool. It can mark a pool as `paused` (temporarily disabling activity) or `sunset` (permanently deactivating the pool), affecting its availability for swaps and actions like adding or removing liquidity. Finally, it emits an event with the pool ID, the deactivation and pause status, and the transaction sender.

##### Parameters

| Name      | Type   |
| --------- | ------ |
| `pool-id` | `uint` |
| `sunset`  | `bool` |
| `paused`  | `bool` |

#### `claim-fees`

A public function, governed through `is-dao-or-extension`, that allows the DAO to withdraw accumulated swap fees from a specific pool and tick. The function resets the stored fees to zero and transfers the collected amounts of both tokens to the specified address using the corresponding token contracts.

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

- `executor-dao`: calls are made to verify whether a certain contract-caller is designated as an extension.
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

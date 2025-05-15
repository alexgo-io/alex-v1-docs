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

The `percent` parameter represents the amount of the position to withdraw, using fixed-point math with 8 decimal precision. For example:

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

This function allows users to swap a specific amount of token X (`dx`) for token Y in a selected bin (`tick`) of a DAMM pool, under an Immediate-Or-Cancel (IOC) execution logic. That means the swap will only proceed if the resulting price stays within the user-defined `max-price` limit. If the conditions are not met, the function cancels the operation or executes it partially — never exceeding the price cap.

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

The pool is stored using a new `pool-id` generated by incrementing `pool-id-nonce`, and it is inserted into both the `pools` map and the `pool-id-by-token` index for lookup by token pair. The pool starts in an active state (not paused or sunset), and the owner is set to the supplied `pool-owner`.

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

A public function, governed through `is-dao-or-extension`, that updates the fee configuration for an existing pool. It sets new values for the swap fee on each token and the rebate returned to liquidity providers.

##### Parameters

| Name         | Type   |
| ------------ | ------ |
| `pool-id`    | `uint` |
| `fee-rate-x` | `uint` |
| `fee-rate-y` | `uint` |
| `fee-rebate` | `uint` |

#### `set-pool-status`

A public function, governed through `is-dao-or-extension`, that updates the operational status of a pool. It can mark a pool as `paused` or `sunset`, affecting its availability for swaps and liquidity actions.

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
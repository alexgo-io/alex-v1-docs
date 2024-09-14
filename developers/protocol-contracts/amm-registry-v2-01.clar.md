# Registry

#### Location: [`alex-dao-2/contracts/aux/amm-registry-v2-01.clar`](https://github.com/alexgo-io/alex-dao-2/blob/main/contracts/aux/amm-registry-v2-01.clar)

This document provides comprehensive technical details for the registry contract within ALEX's Automated Market Maker (AMM) Trading Pool system. The contract primarily functions as a persistence module for all pool-related information needed by the main contract [amm-pool-v2-01.clar](amm-pool-v2-01.clar.md).

To achieve this, the contract allows for the creation and updating of pools. Pool creation involves persisting an entry in a datamap, using `token-x`, `token-y`, and `factor` as the key and containing all relevant pool information.

Additionally, the contract includes configuration getters and setters that support position and swap operations. It is also responsible for managing a list of blocklisted operators.

## Storage

### Variables: (datamap)

* `pools-data-map` (datamap
                        key:
                        {
                            token-x: principal,
                            token-y: principal,
                            factor: uint
                        }
                        value:
                        {
                            pool-id: uint,
                            total-supply: uint,
                            balance-x: uint,
                            balance-y: uint,
                            pool-owner: principal,
                            fee-rate-x: uint,
                            fee-rate-y: uint,
                            fee-rebate: uint,
                            oracle-enabled: bool,
                            oracle-average: uint,
                            oracle-resilient: uint,
                            start-block: uint,
                            end-block: uint,
                            threshold-x: uint,
                            threshold-y: uint,
                            max-in-ratio: uint,
                            max-out-ratio: uint
                        }
                    )
A datamap structure that persists complete pool information. The map key consists of the unique pool identifier `{token-x, token-y, factor}`, and the map value contains detailed pool attributes.

* `pools-id-map` (datamap key: uint value: { token-x: principal, token-y: principal, factor: uint })
A datamap structure that facilitates the retrieval of pool details using the pool ID as the key. The stored values include `token-x` and `token-y` principals, and `factor`.

* `blocklist` (datamap key: principal value: bool)
A datamap structure that stores a persisted list of blocklisted addresses for operating within the Alex Trading Pool.
 
### Variables: (data-var)

* `pool-nonce` (uint)
A persisted variable used to generate a new pool ID incrementally. The stored value represents the last pool ID that was created.

* `switch-threshold` (uint)
An internal variable used to set a fixed threshold for calculations. It is initialized with `u80000000` and can be retrieved and modified using `get-switch-threshold` and `set-switch-threshold` functions. The value of `switch-threshold` must be less than or equal to the constant `ONE_8`. This value is crucial for the mathematical formulas used within the `amm-pool-v2-01.clar` contract.

* `max-ratio-limit` (uint)
This variable sets the upper limit for the ratio in a token pool. These ratios are evaluated during each pool swap operation to determine the maximum amount that can be deposited or exchanged in the pool. It is initialized with the value of the constant `ONE_8`.

#### Mathematical constants

This symbolic constant is employed to define and restrict decimal precision to 8 decimal places.

* `ONE_8` It is declared as `u100000000`.

## Contract calls (interactions)

* `executor-dao` This call is used to verify whether a certain contract caller is designated as an extension.

## Features

1. `create-pool` This function establishes a liquidity pool for a specified token pair (token-x/token-y). It begins by verifying that the `tx-sender` is an ALEX admin operator (see the function `is-dao-or-extension`), as it is intended to be used by the main `amm-pool-v2-01.clar` contract in the current model.
The primary validation performed by this function ensures that the pool does not already exist; if it does, an error is thrown. This validation considers the factor and both token combinations (token-x/token/y or token-y/token-x) as unique identifiers. When a pool is created, an entry is added to the `pools-data-map` structure, using this unique identifier as the key to keep track of all pool information, including balances, fees, thresholds, and more. Additionally, the function generates an ID for the newly created pool (see `pool-nonce`).
All remaining values in the datamap are initialized to zero (`u0`), except for `oracle-enabled`, which is set to `false`. Additionally, `start-block` and `end-block` are initialized with the maximum uint value to ensure the pool remains in a non-operational status until properly initialized. For a complete list of fields, refer to the `pools-data-map`.\
**Input**:
```lisp
(token-x-trait <ft-trait>)
(token-y-trait <ft-trait>)
(factor uint)
(pool-owner principal)
```

2. `update-pool` This function updates a liquidity pool identified by the unique combination of `token-x`, `token-y`, and `factor`. It is a governed function that restricts the `tx-sender` to be an ALEX admin operator (see the function `is-dao-or-extension`).
Similar to the aforementioned `create-pool` function, `update-pool` is designed to be used by the main `amm-pool-v2-01.clar` contract. However, in this case, it is used indirectly in position and swap operations.\
**Input**:
```lisp
(token-x principal)
(token-y principal)
(factor uint)
(pool-data {
    pool-id: uint,
    total-supply: uint,
    balance-x: uint,
    balance-y: uint,
    pool-owner: principal,
    fee-rate-x: uint,
    fee-rate-y: uint,
    fee-rebate: uint,
    oracle-enabled: bool,
    oracle-average: uint,
    oracle-resilient: uint,
    start-block: uint,
    end-block: uint,
    threshold-x: uint,
    threshold-y: uint,
    max-in-ratio: uint,
    max-out-ratio: uint }
)
```

### Governance features

1. `is-dao-or-extension` This standard protocol function checks whether a caller (`tx-sender`) is the DAO executor or an authorized extension, delegating the extensions check to the `executor-dao` contract.\
**Input**:
None.

2. `is-blocklisted-or-default` A read-only feature that verifies if a given address is blacklisted using the `blocklist` map.\
**Input**:
```lisp
(sender principal)
```

3. `set-blocklist-many` A public function, governed by the `is-dao-or-extension` mechanism, that allows setting or updating the blocklisted status for a list of addresses (up to 1000 addresses).\
**Input**:
```lisp
(list 1000 {
    sender: principal,
    blocked: bool
})
```

### Getter and Setter functions

#### Pool administration setters

* `set-fee-rebate`
* `set-pool-owner`
* `set-max-ratio-limit`
* `set-switch-threshold`

#### Pool operation setters and getters
The following groups of functions support pool usage and configuration features consumed by the main `amm-pool-v2-01.clar` contract.

#### Setters

* `set-start-block`
* `set-end-block`
* `set-fee-rate-x`
* `set-fee-rate-y`
* `set-max-in-ratio`
* `set-max-out-ratio`
* `set-oracle-average`
* `set-oracle-enabled`
* `set-threshold-x`
* `set-threshold-y`

#### Getters

* `get-pool-details-by-id`
* `get-pool-details`
* `get-pool-exists`
* `get-max-ratio-limit`
* `get-switch-threshold`

#### Internal helper functions

* `set-blocklist` This is a private function designed to complement the aforementioned governance function `set-blocklist-many`.\
**Input**:
```lisp
(sender principal)
(blocked bool)
```

## Errors defined in the contract
* `ERR-EXCEEDS-MAX-SLIPPAGE`
* `ERR-INVALID-LIQUIDITY`
* `ERR-INVALID-POOL`
* `ERR-MAX-IN-RATIO`
* `ERR-MAX-OUT-RATIO`
* `ERR-NO-LIQUIDITY`
* `ERR-NOT-AUTHORIZED`
* `ERR-ORACLE-AVERAGE-BIGGER-THAN-ONE`
* `ERR-ORACLE-NOT-ENABLED`
* `ERR-PAUSED`
* `ERR-PERCENT-GREATER-THAN-ONE`
* `ERR-POOL-ALREADY-EXISTS`
* `ERR-SWITCH-THRESHOLD-BIGGER-THAN-ONE`

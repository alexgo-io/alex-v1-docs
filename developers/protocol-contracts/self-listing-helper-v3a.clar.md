# self-listing-helper-v3a.clar

- Location: `./contracts/extensions/self-listing-helper-v3a.clar`
- [Deployed contract](https://explorer.hiro.so/txid/SP1E0XBN9T4B10E9QMR7XMFJPMA19D77WY3KP2QKC.self-listing-helper-v3a?chain=mainnet)

The `self-listing-helper-v3a` contract enables the creation of trading pools on the ALEX DEX through two distinct mechanisms: a governance-controlled flow and a permissionless flow. In both cases, pools are formed by pairing a governance-approved anchor token (`token-x`) with a listed token (`token-y`), which is the asset being introduced for trading.

This dual approach gives projects the flexibility to choose between a guided listing process or a fully autonomous, on-chain setup — all while using the same core infrastructure.

## Permissioned

Pools can be created by following a guided process through the [ALEX Lab UI](https://app.alexlab.co/self-service-listing). The user supplies a `token-y` that must be pre-approved by governance before the pool can be initialized.

## Permissionless

Users can list a new `token-y` without prior approval by deploying a wrapper contract that matches a governance-approved template. The contract includes a verification system, based on the [`clarity-stacks`](https://github.com/MarvinJanssen/clarity-stacks) library, that reconstructs and validates the original deployment transaction using Merkle proofs and block data. Once verification succeeds, the token is dynamically approved and the pool is created.

## Additional Capabilities

The contract also supports:

- Liquidity locking or burning for pool integrity
- On-chain parameter configuration (fees, oracles, thresholds)
- Governance-controlled token approvals
- Rebates management for AMM incentives

## Features

### Public

#### `create`

Permissioned pool creation. The listed token (`token-y`) must be pre-approved to initiate the process.

The function first runs validation checks via [`pre-check`](#pre-check), which ensures the anchor token (`token-x`) is approved and that the caller provides sufficient liquidity. It also checks that no existing pool with the given token pair and `factor` already exists.

Next, it verifies that the listing token has a reserve in the `amm-vault-v2-01` contract, which serves as a proxy for its approval status.

If all validations pass, the function calls [`post-check`](#post-check), which:

- Creates the new pool on `amm-pool-v2-01`
- Configures its parameters (fees, thresholds, oracle)
- Applies LP token lock or burn rules via `liquidity-locker` if requested

The function emits a print log with the full pool configuration for transparency.

##### Parameters

| Name              | Type                                                                                                                                                                                                                                                                                                 |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request-details` | `{ token-x-trait: <ft-trait>, token-y-trait: <ft-trait>, factor: uint, bal-x: uint, bal-y: uint, fee-rate-x: uint, fee-rate-y: uint, max-in-ratio: uint, max-out-ratio: uint, threshold-x: uint, threshold-y: uint, oracle-enabled: bool, oracle-average: uint, start-block: uint, lock: (buff 1) }` |

#### `create2`

Permissionless pool creation. The pool is created in a single transaction using a wrapper contract that matches an approved template and includes on-chain verification data.

The function begins by validating the request with [`pre-check`](#pre-check). It then calls `verify-deploy`, which:

- Reconstructs the transaction ID based on the deployment parameters
- Validates that the contract was mined using a Merkle proof and block data
- Ensures the code matches the wrapper template stored in this contract

If verification succeeds, the wrapper is stored in `wrap-token-map`, and `token-y` is dynamically approved via `amm-vault-v2-01.set-approved-token`.

Finally, [`post-check`](#post-check) is executed to create the pool and configure its parameters. The function emits a print log with both the request and verification details.

##### Parameters

| Name              | Type                                                                                                                                                                                                                                                                                                 |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request-details` | `{ token-x-trait: <ft-trait>, token-y-trait: <ft-trait>, factor: uint, bal-x: uint, bal-y: uint, fee-rate-x: uint, fee-rate-y: uint, max-in-ratio: uint, max-out-ratio: uint, threshold-x: uint, threshold-y: uint, oracle-enabled: bool, oracle-average: uint, start-block: uint, lock: (buff 1) }` |
| `verify-params`   | `{ nonce: (buff 8), fee-rate: (buff 8), signature: (buff 65), contract: principal, token-y: principal, proof: { tx-index: uint, hashes: (list 14 (buff 32)), tree-depth: uint }, tx-block-height: uint, block-header-without-signer-signatures: (buff 712) }`                                        |

#### `lock-liquidity`

This function locks a specified amount of LP tokens for a given pool. It is typically used immediately after a pool is created, when the creator chooses to lock the initial liquidity to signal trust to other users.

Internally, it calls the `lock-liquidity` function of the `liquidity-locker` contract. That contract:

- Transfers the LP tokens from the caller to its own custody
- Records the locked amount and sets an unlock block height (default: ~6 months)

##### Parameters

| Name      | Type   |
| --------- | ------ |
| `amount`  | `uint` |
| `pool-id` | `uint` |

#### `burn-liquidity`

This function allows the caller to burn a specified amount of LP tokens from a given pool. It is one of the options available when configuring the pool after creation, typically used to make the initial liquidity permanently inaccessible.

Internally, it calls the `burn-liquidity` function of the `liquidity-locker` contract. That contract:

- Calls the `burn-fixed` function on the pool contract to destroy the LP tokens
- Updates the internal `burnt-liquidity` record for that pool

##### Parameters

| Name      | Type   |
| --------- | ------ |
| `amount`  | `uint` |
| `pool-id` | `uint` |

#### `claim-liquidity`

This function allows users to reclaim their previously locked LP tokens after the lock period has expired.

Internally, it calls the `claim-liquidity` function from the `liquidity-locker` contract. That contract:

- Checks that the caller has a non-zero locked amount
- Verifies that the current block is past the `end-burn-block` set during locking
- Transfers the LP tokens back to the user
- Deletes the corresponding lock record

##### Parameters

| Name      | Type   |
| --------- | ------ |
| `pool-id` | `uint` |

### Governance Features

#### `is-dao-or-extension`

This standard protocol function checks whether a caller (`tx-sender`) is the DAO executor or an authorized extension, delegating the extensions check to the `executor-dao` contract.

#### `approve-token-x`

A public function, governed through `is-dao-or-extension`, that sets the approval status and minimum required balance for a token to be used as the anchor token (`token-x`) in pool creation.

This function updates the `approved-token-x` map, enabling the protocol to define which tokens can serve as anchors and to enforce a minimum contribution threshold (`min-x`) for those tokens.

##### Parameters

| Name       | Type        |
| ---------- | ----------- |
| `token`    | `principal` |
| `approved` | `bool`      |
| `min-x`    | `uint`      |

#### `set-fee-rebate`

A public function, governed through `is-dao-or-extension`, that sets the default fee rebate value used during pool creation.

This value is stored locally in the contract and passed as an argument to the `set-fee-rebate` function of the `amm-registry-v2-01` contract when a new pool is initialized.

##### Parameters

| Name             | Type   |
| ---------------- | ------ |
| `new-fee-rebate` | `uint` |

#### `set-wrapped-token-template`

A public function, governed through `is-dao-or-extension`, that sets the reference template used to validate wrapper token contracts during permissionless pool creation.

The `wrapped-token-template` is a list of code segments (as ASCII strings) that represent the expected body of a compliant wrapper contract. When a user submits a deployment proof via `create2`, this template is used to reconstruct the expected code and verify that the deployed contract matches it exactly.

##### Parameters

| Name           | Type                            |
| -------------- | ------------------------------- |
| `new-template` | `(list 20 (string-ascii 5000))` |

### Getters

#### `get-approved-token-x-or-default`

##### Parameters

| Name      | Type        |
| --------- | ----------- |
| `token-x` | `principal` |

#### `get-fee-rebate`

#### `get-lock-period`

#### `get-locked-liquidity-or-default`

##### Parameters

| Name      | Type        |
| --------- | ----------- |
| `owner`   | `principal` |
| `pool-id` | `uint`      |

#### `get-locked-liquidity-for-pool-or-default`

##### Parameters

| Name      | Type   |
| --------- | ------ |
| `pool-id` | `uint` |

#### `get-burnt-liquidity-or-default`

##### Parameters

| Name      | Type   |
| --------- | ------ |
| `pool-id` | `uint` |

#### `get-wrapped-token-contract-code`

##### Parameters

| Name    | Type        |
| ------- | ----------- |
| `token` | `principal` |

### Relevant internal functions

#### `pre-check`

This private function performs validations prior to pool creation in both permissioned and permissionless flows.

It verifies that:

- The `token-x` is approved and has sufficient balance (`bal-x`).
- A pool with the given pair and factor does not already exist (in either order).
- The provided `lock` parameter is valid (`NONE`, `LOCK`, or `BURN`).

It retrieves the approval and minimum balance requirements from the internal `approved-token-x` map and interacts with the `amm-pool-v2-01` contract to check pool existence.

##### Parameters

| Name              | Type                                                                                                                                                                                                                                                                                                 |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request-details` | `{ token-x-trait: <ft-trait>, token-y-trait: <ft-trait>, factor: uint, bal-x: uint, bal-y: uint, fee-rate-x: uint, fee-rate-y: uint, max-in-ratio: uint, max-out-ratio: uint, threshold-x: uint, threshold-y: uint, oracle-enabled: bool, oracle-average: uint, start-block: uint, lock: (buff 1) }` |

#### `post-check`

This private function finalizes pool creation by initializing the AMM pool and applying additional configuration parameters.

It performs the following actions:

- Calls the `create-pool` function from the `.amm-pool-v2-01` contract to initialize the pool with the provided liquidity amounts.
- If the `lock` is set to `LOCK`, it calls `lock-liquidity` from the `liquidity-locker` contract to lock the initial LP tokens.
- If the `lock` is set to `BURN`, it calls `burn-liquidity` from the `liquidity-locker` contract instead.
- It applies pool parameters like fee rates, max ratios, thresholds, oracle settings, and the `start-block` by calling their respective setter functions in the `amm-pool-v2-01` contract.
- Finally, it applies the global fee rebate for the pool by calling `set-fee-rebate` from the `amm-registry-v2-01` contract.

This function is used after both `create` and `create2` to complete the pool setup.

##### Parameters

| Name              | Type                                                                                                                                                                                                                                                                                                 |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request-details` | `{ token-x-trait: <ft-trait>, token-y-trait: <ft-trait>, factor: uint, bal-x: uint, bal-y: uint, fee-rate-x: uint, fee-rate-y: uint, max-in-ratio: uint, max-out-ratio: uint, threshold-x: uint, threshold-y: uint, oracle-enabled: bool, oracle-average: uint, start-block: uint, lock: (buff 1) }` |

## Storage

### `wrapped-token-template`

| Data     | Type                          |
| -------- | ----------------------------- |
| Variable | `list 20 (string-ascii 5000)` |

A list of strings that define the expected code template for a valid wrapper token contract. Each entry represents a fragment of the full contract body, and the full code is reconstructed by concatenating these parts with the listed token's contract address.

This template is used during permissionless pool creation (`create2`) to verify that a wrapper contract deployed by a user matches the expected safe implementation.

### `approved-token-x`

| Data | Type                                           |
| ---- | ---------------------------------------------- |
| Map  | `principal => { approved: bool, min-x: uint }` |

A map that stores the approval status and minimum liquidity threshold for tokens used as `token-x` (anchor token) in pool creation. This validation is applied in both permissioned and permissionless listing flows.

### `fee-rebate`

| Data     | Type   |
| -------- | ------ |
| Variable | `uint` |

A global rebate value that is assigned to newly created pools. This value is passed to the `amm-registry-v2-01` contract during pool setup, where it determines the protocol fee discount applied to the pool.

### `wrap-token-map`

| Data | Type                     |
| ---- | ------------------------ |
| Map  | `principal => principal` |

A map that links a listed token (`token-y`) to its corresponding verified wrapper contract.  
It is only populated during the permissionless listing flow, once the deployment proof is validated.  
This allows the AMM system to recognize and route trades through the correct wrapper contract.

## Contract calls

- `executor-dao`: calls are made to verify whether a certain contract-caller is designated as an extension.
- `liquidity-locker`: this contract is used to manage post-creation liquidity settings. It allows the contract to lock, burn, or later claim LP tokens based on the pool creator’s configuration.
- `clarity-stacks`: this contract is used to verify that a wrapper contract was properly deployed on the Stacks blockchain. It checks that the contract was mined and included in a valid block. This system is built on top of Marvin Janssen’s [`clarity-stacks`](https://github.com/MarvinJanssen/clarity-stacks) proof framework.
- `clarity-stacks-helper`: this contract is called to convert a Clarity string into its consensus-encoded buffer format.
- `amm-vault-v2-01`: this contract is called to verify that `token-y` has reserves before pool creation and to approve it in the permissionless listing flow after successful wrapper verification.
- `amm-pool-v2-01`: this contract is used to validate pool existence, create new trading pools, and configure pool parameters such as fees, slippage thresholds, oracles, and start block.
- `amm-registry-v2-01`: this contract is called during pool creation to assign the fee rebate configuration for the newly created pool.

## Errors

| Error Name                      | Value         |
| ------------------------------- | ------------- |
| `err-not-authorised`            | `(err u1000)` |
| `err-token-not-approved`        | `(err u1002)` |
| `err-insufficient-balance`      | `(err u1003)` |
| `err-pool-exists`               | `(err u1004)` |
| `err-invalid-lock-parameter`    | `(err u1005)` |
| `err-invalid-length-nonce`      | `(err u2000)` |
| `err-invalid-length-fee`        | `(err u2001)` |
| `err-invalid-length-signature`  | `(err u2002)` |
| `err-invalid-principal-version` | `(err u2003)` |
| `err-principal-not-contract`    | `(err u2004)` |

<!-- Documentation Contract Template v0.1.1 -->
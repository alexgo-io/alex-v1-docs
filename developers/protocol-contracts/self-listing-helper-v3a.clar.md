# self-listing-helper-v3a.clar

- [Deployed contract](https://explorer.hiro.so/txid/SP1E0XBN9T4B10E9QMR7XMFJPMA19D77WY3KP2QKC.self-listing-helper-v3a?chain=mainnet)

The `self-listing-helper-v3a` contract enables the creation of trading pools on the ALEX DEX through two distinct mechanisms: a permissioned flow and a permissionless flow.

In the **permissioned flow**, pre-approved tokens can create pools by following a guided process through the ALEX UI. These tokens must be whitelisted and have sufficient anchor token liquidity.

In the **permissionless flow**, any user can list a new token by deploying a wrapper contract that matches an approved template. The contract includes a verification system to ensure the integrity of the deployment: it reconstructs and validates the original contract transaction using Merkle proofs and block data, confirming that the wrapper is trustworthy.

Once verification succeeds, the token is dynamically approved and a pool is created.

The contract also supports:
- Liquidity locking or burning for pool integrity
- On-chain parameter configuration (fees, oracles, thresholds)
- Governance-controlled token approvals
- Rebates management for AMM incentives

This dual approach gives projects the flexibility to choose between a guided listing process or a fully autonomous, on-chain setup — all while using the same core infrastructure.

## Features

### Public Features

#### `create` 

This function initiates the creation of a liquidity pool between two pre-approved tokens. It is used in the **permissioned listing** flow, where the listing token (`token-y`) has already been approved by governance or whitelisted in the system.

The function first runs validation checks via `pre-check`, which ensures the anchor token (`token-x`) is approved and that the caller provides sufficient liquidity. It also checks that no existing pool with the given token pair and `factor` already exists.

Next, it verifies that the listing token has a reserve in the `amm-vault-v2-01` contract, which serves as a proxy for its approval status.

If all validations pass, the function calls `post-check`, which:
- Creates the new pool on `amm-pool-v2-01`
- Configures its parameters (fees, thresholds, oracle)
- Applies LP token lock or burn rules via `liquidity-locker` if requested

The function emits a print log with the full pool configuration for transparency.

##### Parameters

| Name             | Type       |
|------------------|------------|
| `request-details`| `{ token-x-trait: <ft-trait>, token-y-trait: <ft-trait>, factor: uint, bal-x: uint, bal-y: uint, fee-rate-x: uint, fee-rate-y: uint, max-in-ratio: uint, max-out-ratio: uint, threshold-x: uint, threshold-y: uint, oracle-enabled: bool, oracle-average: uint, start-block: uint, lock: (buff 1) }` |

#### `create2`

This function allows the creation of a new liquidity pool using a token that has **not been pre-approved** by governance. It is the main entry point for the **permissionless listing** flow.

Before calling this function, the user must deploy a wrapper contract for the listing token (`token-y`) that matches an approved template. The contract must then be verified on-chain by submitting proof of deployment.

The function begins by validating the request with `pre-check`. It then calls `verify-deploy`, which:
- Reconstructs the transaction ID based on the deployment parameters
- Validates that the contract was mined using a Merkle proof and block data
- Ensures the code matches the wrapper template stored in this contract

If verification succeeds, the wrapper is stored in `wrap-token-map`, and `token-y` is dynamically approved via `amm-vault-v2-01.set-approved-token`.

Finally, `post-check` is executed to create the pool and configure its parameters. The function emits a print log with both the request and verification details.

##### Parameters

| Name             | Type       |
|------------------|------------|
| `request-details`| `{ token-x-trait: <ft-trait>, token-y-trait: <ft-trait>, factor: uint, bal-x: uint, bal-y: uint, fee-rate-x: uint, fee-rate-y: uint, max-in-ratio: uint, max-out-ratio: uint, threshold-x: uint, threshold-y: uint, oracle-enabled: bool, oracle-average: uint, start-block: uint, lock: (buff 1) }` |
| `verify-params`  | `{ nonce: (buff 8), fee-rate: (buff 8), signature: (buff 65), contract: principal, token-y: principal, proof: { tx-index: uint, hashes: (list 14 (buff 32)), tree-depth: uint }, tx-block-height: uint, block-header-without-signer-signatures: (buff 712) }` |

#### `lock-liquidity`

This function locks a specified amount of LP tokens for a given pool. It is typically used immediately after a pool is created, when the creator chooses to lock the initial liquidity as a trust signal for other users.

Internally, it calls the `lock-liquidity` function of the external `.liquidity-locker` contract. That contract:
- Transfers the LP tokens from the caller to its own custody
- Records the locked amount and sets an unlock block height (default: ~6 months)

##### Parameters

| Name       | Type   |
|------------|--------|
| `amount`   | `uint` |
| `pool-id`  | `uint` |

#### `burn-liquidity`

This function allows the caller to burn a specified amount of LP tokens from a given pool. It is one of the options available when configuring the pool after creation, typically used to make the initial liquidity permanently inaccessible.

Internally, it calls the `burn-liquidity` function of the external `.liquidity-locker` contract. That contract:
- Calls the `burn-fixed` function on the pool contract to destroy the LP tokens
- Updates the internal `burnt-liquidity` record for that pool

##### Parameters

| Name       | Type   |
|------------|--------|
| `amount`   | `uint` |
| `pool-id`  | `uint` |

#### `claim-liquidity`

This function allows users to reclaim their previously locked LP tokens after the lock period has expired.

Internally, it calls the `claim-liquidity` function from the external `liquidity-locker` contract. That contract:
- Checks that the caller has a non-zero locked amount
- Verifies that the current block is past the `end-burn-block` set during locking
- Transfers the LP tokens back to the user
- Deletes the corresponding lock record

##### Parameters

| Name       | Type   |
|------------|--------|
| `pool-id`  | `uint` |

### Governance Features

#### `is-dao-or-extension`
This standard protocol function checks whether a caller (`tx-sender`) is the DAO executor or an authorized extension, delegating the extensions check to the `executor-dao` contract.

#### `approve-token-x`

A public function, governed through the `is-dao-or-extension`, that sets the approval status and minimum required balance for a token to be used as the anchor token (`token-x`) in pool creation.

This function updates the `approved-token-x` map, enabling the protocol to define which tokens can serve as anchors and to enforce a minimum contribution threshold (`min-x`) for those tokens.

##### Parameters

| Name       | Type      |
|------------|-----------|
| `token`    | `principal` |
| `approved` | `bool`      |
| `min-x`    | `uint`      |

#### `set-fee-rebate`

A public function, governed through the `is-dao-or-extension`, that sets the default fee rebate value used during pool creation.

This value is stored locally in the contract and passed as an argument to the `set-fee-rebate` function of the external `amm-registry-v2-01` contract when a new pool is initialized.

##### Parameters

| Name             | Type   |
|------------------|--------|
| `new-fee-rebate` | `uint` |

#### `set-wrapped-token-template`

A public function, governed through the `is-dao-or-extension`, that sets the reference template used to validate wrapper token contracts during permissionless pool creation.

The `wrapped-token-template` is a list of code segments (as ASCII strings) that represent the expected body of a compliant wrapper contract. When a user submits a deployment proof via `create2`, this template is used to reconstruct the expected code and verify that the deployed contract matches it exactly.

##### Parameters

| Name           | Type                                 |
|----------------|--------------------------------------|
| `new-template` | `(list 20 (string-ascii 5000))`      |

### Getters

#### `get-approved-token-x-or-default`

##### Parameters

| Name      | Type       |
|-----------|------------|
| `token-x` | `principal`|

#### `get-fee-rebate`

#### `get-lock-period`

#### `get-locked-liquidity-or-default`

##### Parameters

| Name     | Type       |
|----------|------------|
| `owner`  | `principal`|
| `pool-id`| `uint`     |

#### `get-locked-liquidity-for-pool-or-default`

##### Parameters

| Name     | Type |
|----------|------|
| `pool-id`|`uint`|

#### `get-burnt-liquidity-or-default`

##### Parameters

| Name     | Type |
|----------|------|
| `pool-id`|`uint`|

#### `get-wrapped-token-contract-code`

##### Parameters

| Name    | Type       |
|---------|------------|
| `token` | `principal`|

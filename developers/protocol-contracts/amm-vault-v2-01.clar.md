# Vault

#### Location: [`alex-dao-2/contracts/aux/amm-vault-v2-01.clar`](https://github.com/alexgo-io/alex-dao-2/blob/main/contracts/aux/amm-vault-v2-01.clar)

This document provides comprehensive technical details for the vault contract within ALEX's Automated Market Maker (AMM) Trading Pool system. The vault contract supports the primary contract [amm-pool-v2-01.clar](amm-pool-v2-01.clar.md) in position and swap operations by keeping record of the reserves accumulated from fees and securing pool assets. It ensures asset security through transfer transactions where the vault contract is the recipient of token transfers, thereby holding and safeguarding the assets within the pool.
In addition to supporting Trading Pool operations, the vault contract also offers a flash-loan feature for registered tokens, available to approved users.

## Storage

### Variables: (data-var)

* `paused` (bool)
This data variable acts as a flag to determine and control the operational status of the contract within the system.

#### Token variables

* `approved-tokens` (datamap key: principal value: bool)
A datamap structure that lists all the approved tokens for transfer and loan operations.

* `reserve` (datamap key: principal value: uint)
A datamap structure that keeps record of all the reserves in the vault for a specific token.

#### Flash-loan variables

* `approved-flash-loan-users` (datamap key: principal value: bool)
A datamap structure that lists all the approved users for loan operations.

* `flash-loan-enabled` (bool)
A flag variable indicating the status of the contract's flash-loan feature. Currently, this is only an informative flag.

* `flash-loan-fee-rate` (uint)
This variable defines the fee rate charged for loan operations.

#### Mathematical constants

This symbolic constant is employed to define and restrict decimal precision to 8 decimal places.

* `ONE_8` It is declared as `u100000000`.

## Contract calls (interactions)

* `executor-dao` This call is used to verify whether a certain contract caller is designated as an extension.

* `token-trait` Calls are made to token contracts that comply with the <ft-trait> defined in each token-trait parameter to facilitate token transfers and retrieve balances during transfer and loan operations.

* `flash-loan-user-trait` Calls are made to token contracts that comply with the <flash-loan-trait> defined in the flash-loan-user-trait variable to execute loans for the approved flash-loan users and tokens.

## Features

1. `add-to-reserve` This function increases the existing reserves of a specific token by the given amount. It is governed by the `is-dao-or-extension` check to ensure that the `tx-sender` is an ALEX admin operator. Intended to be invoked by the main `amm-pool-v2-01.clar` contract during swap operations, this function is called following a transfer of the incoming token (token-x) to the vault contract. The `add-to-reserve` function is then executed with the token principal and the specified amount, representing the swap fees charged by the system for the transaction.\
**Input**:
```lisp
(token-trait principal)
(amount uint)
```

2. `remove-from-reserve` This function serves as the reverse operation of `add-to-reserve`, decreasing the specified amount for the given token. An additional check ensures that the amount is less than or equal to the token's current reserve in the vault.
Although it is not currently called by any existing module, the function also verifies that the `tx-sender` is an ALEX admin operator via the `is-dao-or-extension` control.\
**Input**:
```lisp
(token-trait principal)
(amount uint)
```

3. `transfer-ft` Like `add-to-reserve`, this function is intended to be called by the `amm-pool-v2-01.clar` contract during swap operations and is governed by the `is-dao-or-extension` check. Controls are in place to ensure that the contract is operational (not paused) and that the given token is approved in the contract's list.
If all these conditions are met, the function will execute a transfer of the specified amount from the vault to the designated recipient.\
**Input**:
```lisp
(token-trait <ft-trait>)
(amount uint)
(recipient principal)
```

4. `transfer-ft-two` This function serves as a helper for calling the `transfer-ft` function twice with two different tokens and amounts for the same recipient within a single transaction. In the current model, this feature is utilized by the `amm-pool-v2-01.clar` contract during position reduction operations to return assets to the user.\
**Input**:
```lisp
(token-x-trait <ft-trait>)
(dx uint)
(token-y-trait <ft-trait>)
(dy uint)
(recipient principal)
```

5. `transfer-sft` This function is similar to `transfer-ft`, but it is designed for semi-fungible tokens, which operate with a token ID. It includes the same controls as `transfer-ft` regarding the vault's operational status and approved tokens. Although this function is not called by the `amm-pool-v2-01.clar` contract in the current system design, it requires invocation by an ALEX admin operator, as governed by the `is-dao-or-extension` check.\
**Input**:
```lisp
(token-trait <sft-trait>)
(token-id uint)
(amount uint)
(recipient principal)
```

6. `flash-loan` This function is designed to lend the recipient (the `tx-sender`) a specified amount of tokens to execute an embedded function declared in the `flash-loan-trait`. The recipient then transfers back the same amount plus the corresponding fee (refer to `flash-loan-fee-rate`).
As with previous features, controls are in place to check the vault's operational status, approve tokens and flash-loan users, and ensure there is sufficient balance to make the transfer.\
**Input**:
```lisp
(flash-loan-user-trait <flash-loan-trait>)
(token-trait <ft-trait>)
(amount uint)
(memo (optional (buff 16)))
```

### Governance features

1. `is-dao-or-extension` This standard protocol function checks whether a caller (`tx-sender`) is the DAO executor or an authorized extension, delegating the extensions check to the `executor-dao` contract.\
**Input**:
None.

2. `is-paused` A read-only function that checks the operational status of the contract.\
**Input**:
None.

3. `pause` A public function, governed through the `is-dao-or-extension`, that can change the contract's operational status.\
**Input**:
```lisp
(new-paused bool)
```

### Getter and Setter functions

#### Vault administration getter
* `get-reserve`

#### Flash-loan operation setters and getters

**Setters**

* `set-approved-flash-loan-user`
* `set-approved-token`
* `set-flash-loan-enabled`
* `set-flash-loan-fee-rate`

**Getters**
* `get-flash-loan-enabled`
* `get-flash-loan-fee-rate`

#### Internal helper functions

* `check-is-approved-flash-loan-user` This is a private function designed to verify whether a flash-loan user is approved in the contract's persisted datamap, `approved-flash-loan-users`.\
**Input**:
```lisp
(flash-loan-user-trait principal)
```

* `check-is-approved-token` This is a private function designed to verify whether a token is approved in the contract's persisted datamap, `approved-tokens`.\
**Input**:
```lisp
(flash-loan-token principal)
```

**Mathematical helpers**

These helper functions aid in various calculations within the context of the contract.
* `mul-down`
* `mul-up`

## Errors defined in the contract

* `ERR-AMOUNT-EXCEED-RESERVE`
* `ERR-INVALID-BALANCE`
* `ERR-INVALID-TOKEN`
* `ERR-NOT-AUTHORIZED`
* `ERR-PAUSED`

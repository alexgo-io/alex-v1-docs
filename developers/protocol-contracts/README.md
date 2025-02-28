---
description: ALEX DAO - Comprehensive Technical Design Overview
---

This document provides a detailed overview of the smart contracts that enable ALEX DeFi operations. We categorize these contracts based on their functionalities, explain their interactions, and describe some common technical aspects.

## AMM Trading Pool

This section provides an overview of the ALEX on-chain Trading Pool and its Automated Market Making (AMM) protocol.

![](https://kroki.io/plantuml/svg/eNptjssKgzAQRfd-xeC2pK-1CP0AoQuX3YxxGgIxkZkoSOm_NyrFhW7vuefOMOmI3jiC_FFVUDO21ht4huBy-GS84aIpsetUn4ga7-p6O2uHXFya8uVDT4zRBi85oMDcOVCZjJXI005PtiRCXk-L_y9m34OVEQcXdxNvoqQJ8UgCJxDSA1ObxoQkrl8tYjb_BqrcbqxBSlb-A0UYXfM=)

### Pool: amm-pool-v2-01.clar

This is the primary contract in ALEX's AMM Trading Pool system. It encompasses several core operations, including pool creation, liquidity operations, LP token management, and token swapping. This contract is complemented by the two auxiliary contracts listed below.

[Complete technical documentation](./amm-pool-v2-01.clar.md)

### Registry: amm-registry-v2-01.clar

This contract functions as a persistence module for all pool-related information needed by the ALEX Automated Market Maker (AMM) Trading Pool system. It also manages a list of blocklisted operators.

[Complete technical documentation](./amm-registry-v2-01.clar.md)

## Vault

The Vault component of the ALEX platform is distinct from the Trading Pool components by design. This separation offers numerous advantages, such as reduced transaction costs for users and a faster learning curve for developers who are creating custom pools on ALEX.

### Vault: amm-vault-v2-01.clar

The Vault contract supports the primary contract `amm-pool-v2-01.clar` in position and swap operations by keeping records of the reserves accumulated from fees and securing pool assets. This contract also offers a flash-loan feature for registered tokens, available to approved users.

[Complete technical documentation](./amm-vault-v2-01.clar.md)

## Farming Campaign: farming-campaign-v2-02.clar

This contract manages the ALEX Surge liquidity incentive program, overseeing pool registration, voting, staking, and reward distribution. It determines how $ALEX rewards are allocated based on user votes and supports additional voter incentives.

[Complete technical documentation](./farming-campaign-v2-02.clar.md)

## Common features

### Governance

The smart contracts discussed in this section include features to control administrative privileges, access, and operational status. It is needed to duplicate some functions for them to be available on each contract.

#### Admin access control

Contracts in the ALEX platform may include a feature that checks administrative access through the function `is-dao-or-extension`. This function ensures that the caller (`tx-sender`) is either the DAO executor or an authorized extension.

#### Operational status

The `amm-pool-v2-01` and `amm-vault-v2-01` contracts have functionalities to set and query the operational status of the contract. Specifically, these contracts include a `paused` flag. When this flag is set to true, all operations within the contract are halted.

#### Blocklisted operators

The `amm-registry-v2-01` contract features the ability to update (on admin operator request) and query a list of blacklisted addresses that are prohibited from operating within the trading pool. The `amm-pool-v2-01` contract delegates the task of this verification to the Registry, which performs a check against the `tx-sender`.

### Traits

In Clarity language, a trait defines a public interface to which smart contracts can conform. All Trading Pool contracts in this documentation import traits to ensure interface conformity for various types (such as tokens and flash-loan users) and to conduct their transactions safely. These traits are customized versions of standard traits which are provided by the ALEX platform to serve specific purposes. Below is a list of all the traits utilized by the Trading Pool contracts.

#### sip-010-trait

This is a customized version of the Stacks' Standard Trait Definition for Fungible Tokens and is used by contracts in the ALEX platform. The changes in this version include support for 8-digit fixed notation and additional helper functions for `transfer`, `get-balance`, and `get-total-supply`. Mint and burn functions are also included along with their respective helpers.

[ALEX sip-010 customized implementation](https://github.com/alexgo-io/alex-dao-2/blob/main/contracts/traits/trait-sip-010.clar) | 
[Full sip-010 standard](https://github.com/stacksgov/sips/blob/main/sips/sip-010/sip-010-fungible-token-standard.md)

#### semi-fungible-trait

This is a customized version of the Stacks' Standard Trait Definition for Semi-Fungible Tokens and is used only by the Vault contract.
The customizations are similar to those in the customized `sip-010-trait`, with additional fixed notation helpers for the `transfer-memo`, `get-overall-balance`, and `get-overall-supply` functions. Additionally, the `get-token-uri` function is customized to redefine the `response` parameter type to the tuple: `(optional (string-utf8 256)) uint` instead of sip-013's `string-ascii`.

[ALEX sip-013 customized implementation](https://github.com/alexgo-io/alex-dao-2/blob/main/contracts/traits/trait-semi-fungible.clar) | 
[Full sip-013 standard](https://github.com/stacksgov/sips/blob/main/sips/sip-013/sip-013-semi-fungible-token-standard.md)

#### flash-loan-user-trait

This is a custom trait from ALEX designed to support flash-loan operations and is used only by the Vault contract. The trait defines an `execute` function, which is asserted in the Vault's `flash-loan` function signature, allowing the flash-loan user to execute their logic with the loaned amount.

[ALEX flash-loan customized implementation](https://github.com/alexgo-io/alex-dao-2/blob/main/contracts/traits/trait-flash-loan-user.clar)

### Mathematics helpers

The Pool and Vault contracts are equipped with helper functions that facilitate various mathematical operations (e.g., amounts, percentages) with the necessary precision. These helpers improve the expressiveness of the contracts' logic. Some of these helper functions utilize mathematical constants defined within each contract. All helper functions are declared as private and, like the governance functions, may be replicated across both contracts using the same equations.

|Topic|Functions|
|--|--|
|Precision multipliers and divisors|`mul-down`, `mul-up`, `div-down`, `div-up`|
|Rolling summation|`rolling_sum_div`, `rolling_div_sum`|
|Accumulate|`accumulate_division`, `accumulate_product`|
|Power and exponential|`pow-fixed`, `pow-priv`, `pow-down`, `pow-up`, `exp-fixed`, `exp-pos`|
|Logarithmic|`log-fixed`, `ln-fixed`, `ln-priv`|

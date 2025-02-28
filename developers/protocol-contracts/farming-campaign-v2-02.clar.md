# farming-campaign-v2-02.clar

<!--- Location: [`alex-dao-2/contracts/extensions/farming-campaign-v2-02.clar`](https://github.com/alexgo-io/alex-dao-2/blob/main/contracts/extensions/farming-campaign-v2-02.clar)-->
- [Deployed contract](https://explorer.hiro.so/txid/SP1E0XBN9T4B10E9QMR7XMFJPMA19D77WY3KP2QKC.farming-campaign-v2-02?chain=mainnet)

The `farming-campaign-v2-02` contract manages the ALEX Surge liquidity incentive program, where liquidity pools compete to receive $ALEX rewards. Participants influence the reward distribution by voting for pools, while liquidity providers can stake LP tokens to earn additional rewards.
Surge is structured in rounds. The process follows these steps:
1. **Pool Registration** – Projects must register liquidity pools before the deadline to participate in the Surge round.
2. **Voting** – Users vote using ALEX and LiALEX tokens to decide how rewards will be distributed among the registered pools.
3. **Staking** – Liquidity providers stake LP tokens, which represent their share of a trading pool’s assets, to earn additional rewards.
4. **Reward Distribution** – At the end of the round, $ALEX rewards are distributed among the pools based on the votes received. Within each pool, the rewards are allocated proportionally to liquidity providers based on the amount of LP tokens they staked.

Additionally, projects can donate voter rewards as extra incentives to attract votes for their pools.

## Features

### Public Features

#### `stake`

This function allows users to stake LP tokens in a registered liquidity pool within an active Surge campaign. Staking LP tokens makes users eligible to receive a share of the $ALEX rewards allocated to the pool based on the total votes it received.
The function verifies that the registration period has ended and the staking period is still active. It then interacts with the `token-amm-pool-v2-01` contract to transfer the specified amount of LP tokens from the sender’s wallet to the contract. After the transfer, it updates the total amount of LP tokens staked in the pool. Once the staking is confirmed, the contract updates the `campaign-stakers` map with the staker’s details and emits a notification event.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-id`|`uint`|
| `campaign-id`| `uint`|
|`amount`|`uint`|

#### `unstake`

This function allows users to withdraw their staked LP tokens from a liquidity pool in a Surge campaign while claiming any earned $ALEX. Before proceeding, it verifies that the staking period has ended and that the user has not already collected their allocation.
The contract determines the user's share based on the $ALEX rewards allocated to the pool, which were determined by the votes it received. The distribution within the pool is then proportional to the amount of LP tokens each user staked. Once determined, it mints and transfers the corresponding amount to the user via the `token-alex` contract. Additionally, it calls `token-amm-pool-v2-01` to return the LP tokens back to the user and updates `campaign-stakers` to mark the process as complete. A notification event logs the transaction.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-id`|`uint`|
| `campaign-id`| `uint`|

#### `register-for-campaign`

This function allows a liquidity pool to register to participate in a Surge campaign. Before registering, it ensures that the pool is whitelisted and that the registration deadline has not been reached. Additionally, it checks that the pool has not already been registered.
Once validated, the function creates an entry in `campaign-registrations`, initializing reward amounts and total staked values to `0`. It also updates `campaign-registered-pools` to add the new registered pool. A notification event logs the registration.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-id`|`uint`|
| `campaign-id`| `uint`|

#### `add-reward-for-campaign`

This function allows a registrant to add voter rewards to a liquidity pool already registered in a Surge round. These rewards incentivize users to vote for the pool.
The function first verifies that the pool is registered in the campaign. It ensures that the voting phase is still open and validates that the reward token matches either of the pool’s trading tokens by interacting with the `amm-pool-v2-01`. If these conditions are met, the function transfers the specified reward amount from the registrant to the contract using the provided token contract. It then updates the stored reward balances for the pool in `campaign-registrations` and records the registrant’s contribution in `campaign-registrants`.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-id`|`uint`|
| `campaign-id`| `uint`|
|`reward-token-trait`|`<ft-trait>`|
|`reward-amount`|`uint`|

#### `vote-campaign`

This function allows participants to vote for liquidity pools in a Surge round, determining how $ALEX rewards are distributed. Users allocate their voting power across one or multiple pools.
The function verifies that the voting phase is active and retrieves the voter’s available voting power using the `voting-power` function, which calculates the user's voting power based on their $ALEX and $LiALEX holdings. It ensures that the total votes cast does not exceed the voter's limit. Once validated, the function updates the vote counts for each selected pool in `campaign-voter-votes` map, records the total votes cast in `campaign-total-vote` map and applies the vote distribution logic through `update-pool-votes` function.

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
| `votes`| `list 1000 { pool-id: uint, votes: uint }`|
|`lp-pools`|`list 200 uint`|

#### `claim-vote-reward`

This function allows voters to claim rewards from a Surge round if they voted for a liquidity pool that distributed voter rewards. These rewards are additional incentives offered by pool registrants to encourage voting participation.
The function first checks that the voting phase has ended and ensures the voter has not previously claimed rewards for the specified pool. It then calculates the voter’s share based on the total votes cast for the pool, using data stored in `campaign-pool-votes-by-voter` and `campaign-pool-votes-for-project-reward` maps, which track individual and total votes for voter rewards distribution. The function verifies that the requested reward tokens match the liquidity pool’s assets by interacting with the `amm-pool-v2-01` contract. If all conditions are met, the function transfers the voter’s share of the rewards and updates `campaign-vote-rewards-claimed` to prevent duplicate claims.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-id`|`uint`|
| `campaign-id`| `uint`|
|`reward-token-x-trait`|`<ft-trait>`|
|`reward-token-y-trait`|`<ft-trait>`|

#### `claim-vote-reward-many`

This function enables batch claiming of voter rewards for multiple liquidity pools and campaigns in a single transaction. It iterates over the provided lists of pool IDs, campaign IDs and reward token contracts, calling the `claim-vote-reward` function for each entry.
Since it relies on [`claim-vote-reward`](#claim-vote-reward) function, the same eligibility checks apply.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-ids`|`list 100 uint`|
| `campaign-ids`| `list 100 uint`|
|`reward-token-x-traits`|`list 100 <ft-trait>`|
|`reward-token-y-traits`|`list 100 <ft-trait>`|

### Privileged Features

#### `revoke-registration`

This privileged function allows revoking a pool's registration in a Surge campaign before the registration cut-off date. The action can be performed by the DAO or an authorized extension, or by the pool's registrant if `revoke-enabled` is set to `true`. If revoked, the registrant recovers the voter rewards they contributed.
The function ensures that the specified reward tokens match the pool’s trading pair by interacting with `amm-pool-v2-01`. If the conditions are met, it updates the `campaign-registrations` and `campaign-registrants` maps to remove the pool and return the reward tokens to the registrant.

### Governance Features

#### `is-dao-or-extension`
This standard protocol function checks whether a caller (`tx-sender`) is the DAO executor or an authorized extension, delegating the extensions check to the `executor-dao` contract.

#### `set-campaign-nonce`

A public function, governed through the `is-dao-or-extension`, that updates the campaign nonce, which serves as a unique identifier for newly created Surge rounds. This ensures that each campaign has a sequential and distinct ID.

##### Parameters

| Name    | Type |
| --------| ---- |
|`new-nonce`|`uint`|

#### `set-revoke-enabled`

A public function, governed through the `is-dao-or-extension`, that enables or disables the ability to revoke a registered liquidity pool from a campaign before the registration cut-off date. When set to `true`, the registrant of a pool can cancel its participation and recover the reward tokens added during registration. By default, this is set to `false`, meaning that once a pool is registered, it cannot be removed from the campaign.

##### Parameters

| Name    | Type |
| --------| ---- |
|`enabled`|`bool`|

#### `whitelist-pools`

A public function, governed through the `is-dao-or-extension`, that updates the list of liquidity pools eligible to participate in Surge campaigns. Only pools that are whitelisted can be registered in a campaign.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pools`|`(list 1000 uint)`|

#### `create-campaign`

A public function, governed through the `is-dao-or-extension`, that initializes a new Surge campaign. This function sets key parameters such as registration, voting, and staking deadlines, as well as the total reward amount allocated for distribution. Each campaign is assigned a unique ID, incremented from the `campaign-nonce` variable.

##### Parameters

| Name    | Type |
| --------| ---- |
|`registration-cutoff`|`uint`|
|`voting-cutoff`|`uint`|
|`stake-cutoff`|`uint`|
|`stake-end`|`uint`|
|`reward-amount`|`uint`|
|`snapshot-block`|`uint`|

#### `transfer-token`

A public function, governed through the `is-dao-or-extension`, that allows the contract to transfer a specified amount of a fungible token to a recipient.

##### Parameters

| Name    | Type |
| --------| ---- |
|`token-trait`|`<ft-trait>`|
|`amount`|`uint`|
|`recipient`|`principal`|

#### `update-campaign`

A public function, governed through the `is-dao-or-extension`, that updates the parameters of an existing campaign. This function modifies attributes such as registration deadlines, voting periods, staking timelines, and reward allocations.

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`details`|`{ registration-cutoff: uint, voting-cutoff: uint, stake-cutoff: uint, stake-end: uint, reward-amount: uint, snapshot-block: uint }`|

#### `update-campaign-registrations`

A public function, governed through the `is-dao-or-extension`, that updates the registration details of a liquidity pool within a campaign. This function allows modifying the reward amounts allocated in token X and token Y, as well as the total staked amount in the pool. If the pool is not already registered in the campaign, it will be added to the list of registered pools.

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`pool-id`|`uint`|
|`reward-amount-x`|`uint`|
|`reward-amount-y`|`uint`|
|`total-staked`|`uint`|

#### `update-campaign-stakers`

A public function, governed through the `is-dao-or-extension`, that updates the staking details of a participant in a specific liquidity pool within a campaign. This function modifies the amount of LP tokens staked by a user and whether they have claimed their rewards.

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`pool-id`|`uint`|
|`staker`|`principal`|
|`amount`|`uint`|
|`claimed`|`bool`|

#### `update-campaign-registrants`

A public function, governed through the `is-dao-or-extension`, that updates the amount of token X and token Y recorded for a registrant when adding a liquidity pool to a campaign. These values represent voter rewards allocated to the pool, which can be modified or revoked before the registration cut-off date.

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`pool-id`|`uint`|
|`registrant`|`principal`|
|`token-x-amount`|`uint`|
|`token-y-amount`|`uint`|

#### `set-project-reward-ignore-list`

A public function, governed through the `is-dao-or-extension`, that updates the list of addresses excluded from receiving voter rewards.

##### Parameters

| Name    | Type |
| --------| ---- |
|`addresses`|`(list 1000 principal)`|

### Getters

#### `get-campaign-nonce`

#### `get-campaign-or-fail`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|

#### `get-campaigns-or-fail-many`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-ids`|`(list 200 uint)`|

#### `get-campaign-registration-by-id-or-fail`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`pool-id`|`uint`|

#### `get-campaign-registration-by-id-or-fail-many`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-ids`|`(list 200 uint)`|
|`pool-ids`|`(list 200 uint)`|

#### `get-campaign-staker-or-default`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`pool-id`|`uint`|
|`staker`|`principal`|

#### `get-campaign-staker-or-default-many`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-ids`|`(list 200 uint)`|
|`pool-ids`|`(list 200 uint)`|
|`stakers`|`(list 200 principal)`|

#### `get-pool-whitelisted`

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-id`|`uint`|

#### `get-whitelisted-pools`

#### `voting-power`

#### `get-campaign-registered-pools`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|

#### `get-campaign-summary`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|

#### `get-campaign-staker-history-many`

##### Parameters

| Name    | Type |
| --------| ---- |
|`address`|`principal`|
|`campaign-ids`|`(list 200 uint)`|

#### `get-registration-or-default`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`pool-id`|`uint`|
|`registrant`|`principal`|

#### `get-registration-or-default-many`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|
|`pool-ids`|`(list 1000 uint)`|
|`registrant`|`principal`|

#### `get-revoke-enabled`

##### Parameters

| Name    | Type |
| --------| ---- |
|`campaign-id`|`uint`|

### Relevant internal functions

#### `update-pool-votes`

This private function updates the voting records for a liquidity pool. It is invoked by the `vote-campaign` function when a user submits votes.
The function retrieves the current vote counts for the specified pool and updates the following vote-tracking maps: `campaign-pool-votes-for-project-reward`, `campaign-pool-votes-for-alex-reward` and `campaign-pool-votes-by-voter`.

##### Parameters

| Name    | Type |
| --------| ---- |
|`vote`|`{ pool-id: uint, votes: uint }`|
| `context`| `{ campaign-id: uint, voter: principal }`|

#### `calculate-lp-voting-power`

This private function is used to calculate the LP voting power of a user for a specific liquidity pool. It is called by the `voting-power` function to determine the contribution of LP tokens to a user's total voting power.
The function retrieves the pool's details from `amm-registry-v2-01` and `amm-pool-v2-01`. It then queries `alex-farming` contract to check if the user has staked LP tokens. Using this data, it calculates the user's LP voting power by determining how much ALEX-equivalent liquidity they provided, factoring in both directly held and staked LP tokens.

##### Parameters

| Name    | Type |
| --------| ---- |
|`pool-id`|`uint`|
| `acc`| `{ address: principal, total: uint }`|

## Storage

### `campaign-nonce`

| Data     | Type   |
| -------- | ------ |
| Variable | `uint` |

A counter that keeps track of the total number of campaigns created. Each new campaign increments this value by `1`, ensuring a unique identifier for every Surge round.

### `revoke-enabled`

| Data     | Type   |
| -------- | ------ |
| Variable | `bool` |

A flag that determines whether a registered liquidity pool can be revoked from a campaign before the registration cut-off date. When set to `true`, the registrant of a pool can cancel its participation and recover the reward tokens added during registration.
By default, this value is set to `false`, meaning that once a pool is registered, it cannot be removed from the campaign.

### `whitelisted-pools`

| Data     | Type   |
| -------- | ------ |
| Variable | `list (1000 uint)` |

A list of approved liquidity pools that are eligible to participate in ALEX Surge campaigns. Only pools included in this list can register for a campaign.
By default, this list is empty, meaning that no pools are eligible until explicitly added. The DAO or an authorized contract can update this list through governance functions.

### `project-reward-ignore-list`

| Data     | Type   |
| -------- | ------ |
| Variable | `list (1000 principal)` |

A list of addresses that are excluded from receiving voter rewards in ALEX Surge. If a voter's address is included in this list, they will not be eligible to claim any voter rewards, even if they participated in voting.
By default, this list is empty, meaning no addresses are excluded initially. The DAO or an authorized contract can update this list through governance functions.

### `campaigns`

| Data     | Type   |
| -------- | ------ |
| Map | `uint → { registration-cutoff: uint, voting-cutoff: uint, stake-cutoff: uint, stake-end: uint, reward-amount: uint, snapshot-block: uint }` |

This map stores campaign data, where each entry is identified by a campaign ID (`uint`). It tracks the key timeline phases and reward details for each ALEX Surge campaign.

- `registration-cutoff`: the deadline for registering pools in the campaign.
- `voting-cutoff`: the deadline for casting votes.
- `stake-cutoff`: indicates the moment from which LP tokens can no longer be staked.
- `stake-end`: the point at which the staking period ends and the reward emission phase begins.
- `reward-amount`: the total $ALEX allocated for distribution in the campaign.
- `snapshot-block`: the block height at which participant balances are recorded for voting power calculations.

### `campaign-registrations`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, pool-id: uint } → { reward-amount-x: uint, reward-amount-y: uint, total-staked: uint }` |

This map stores the registration data of each liquidity pool within a specific Surge round.

Fields:
- `campaign-id`: the unique identifier of the Surge round.
- `pool-id`: the unique identifier of the registered liquidity pool.
- `reward-amount-x`: the amount of rewards in token X allocated to the pool.
- `reward-amount-y`: the amount of rewards in token Y allocated to the pool.
- `total-staked`: the total amount of LP tokens staked in the pool.

This map is updated when a pool registers for a campaign and when LPs stake tokens in it.

### `campaign-stakers`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, pool-id: uint, staker: principal } → { amount: uint, claimed: bool }` |

This map stores staking data for individual participants in a specific liquidity pool within a Surge round.

Fields:
- `campaign-id`: The unique identifier of the Surge round.
- `pool-id`: The unique identifier of the liquidity pool where the staking occurs.
- `staker`: the principal identifier of the user who staked LP tokens.
- `amount`: The total amount of LP tokens staked by the user in the pool.
- `claimed`: A boolean value indicating whether the user has claimed their rewards.

This map is updated when users stake LP tokens and when they claim their rewards at the end of the campaign.

### `campaign-total-vote`

| Data     | Type   |
| -------- | ------ |
| Map | `campaign-id: uint → total-votes: uint` |

This map tracks the total number of votes cast in a specific Surge round. It is updated whenever users vote for liquidity pools during the voting phase.

### `campaign-registered-pools`

| Data     | Type   |
| -------- | ------ |
| Map | `campaign-id: uint → pool-ids: (list 1000 uint)` |

This map stores the list of liquidity pools that have been successfully registered in a specific Surge round.

### `campaign-registrants`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, pool-id: uint, registrant: principal } → { token-x-amount: uint, token-y-amount: uint }` |

This map records the amount of token X and token Y added by a registrant when adding a liquidity pool to a Surge round. These tokens are used as rewards for voters but can be revoked before the registration cut-off date.

### `campaign-voter-votes`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, voter: principal } → uint` |

This map tracks the total voting power spent by a voter across all liquidity pools in a given Surge round.

### `campaign-pool-votes-by-voter`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, pool-id: uint, voter: principal } → uint` |

This map records the number of votes a specific voter has allocated to a particular liquidity pool within a Surge round. It is primarily used to calculate project rewards.

### `campaign-pool-votes-for-project-reward`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, pool-id: uint } → uint` |

This map stores the total number of votes received by a liquidity pool in a Surge round, specifically for the calculation of project rewards.

### `campaign-pool-votes-for-alex-reward`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, pool-id: uint } → uint` |

This map tracks the total number of votes received by a liquidity pool in a Surge round, specifically for the $ALEX reward distribution.

### `campaign-vote-rewards-claimed`

| Data     | Type   |
| -------- | ------ |
| Map | `{ campaign-id: uint, pool-id: uint, voter: principal } → bool` |

This map tracks whether a voter has claimed their voter rewards for a specific pool in a Surge round.

## Contract calls
- `executor-dao`: calls are made to verify whether a certain contract-caller is designated as an extension.
- `token-alex`: this contract is called to mint and transfer $ALEX rewards to liquidity providers when they unstake.
- `token-wlialex`: this contract is called to retrieve the $LiALEX balance of a voter at the snapshot block to calculate their voting power.
- `auto-alex-v3-wrapped`: this contract is necessary for user's voting power calculations.
- `alex-staking-v2`: this contract is called to retrieve a user's manually staked $ALEX balance, which is included in the calculation of their total voting power.
- `token-amm-pool-v2-01`: this contract is called to transfer staked LP tokens during the staking process and to return them when users unstake. It is also used to validate reward token compatibility when adding voter rewards.
- `amm-pool-v2-01`: this contract is called to transfer staked LP tokens during the staking process and to return them when users unstake. It is also used to retrieve LP token balances and total supply when calculating a user's voting power.
- `reward-token-trait`: this trait represents the fungible tokens used for distributing staking and voter rewards. It follows the `SIP-010` standard.
- `reward-token-x-trait`: this trait refers to one of the two possible reward tokens in a liquidity pool.
- `reward-token-y-trait`: this trait refers to one of the two possible reward tokens in a liquidity pool.
- Tokens (`token-trait`): this trait is used to interact with fungible tokens when transferring staking deposits, rewards, and voter incentives. It is a customized version of Stacks' standard definition for Fungible Tokens (`sip-010`), with support for 8-digit fixed notation.
- `amm-registry-v2-01`: this contract is called to retrieve data about a liquidity pool. It is used in the calculation of LP voting power.
- `alex-farming`: this contract is called to retrieve staking information for users who have deposited LP tokens into the Alex Farming system. It is used to calculate LP voting power.

## Errors

| Error Name       | Value         |
| ---------------- | ------------- |
| `err-not-authorized` | `(err u1000)` |
| `err-get-block-info`    | `(err u1001)` |
| `err-invalid-campaign-registration`    | `(err u1002)` |
| `err-invalid-campaign-id`    | `(err u1003)` |
| `err-registration-cutoff-passed`    | `(err u1004)` |
| `err-stake-cutoff-passed`    | `(err u1005)` |
| `err-campaign-not-ended`    | `(err u1006)` |
| `err-token-mismatch`    | `(err u1007)` |
| `err-invalid-input`    | `(err u1008)` |
| `err-invalid-reward-token`    | `(err u1010)` |
| `err-already-claimed`    | `(err u1011)` |
| `err-stake-end-passed`    | `(err u1005)` |
| `err-not-registered`    | `(err u1013)` |
| `err-revoke-disabled`    | `(err u1014)` |
| `err-registration-cutoff-not-passed` | `(err u1015)` |
| `err-voting-cutoff-passed`    | `(err u1016)` |
| `err-pool-not-registered`    | `(err u1017)` |
| `err-pool-already-registered`    | `(err u1018)` |
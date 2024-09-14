---
hidden: true
---

# Using the Launchpad

## Deployment

[https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.alex-launchpad-v1-1?chain=mainnet](https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.alex-launchpad-v1-1?chain=mainnet)

## Listing

`contract-owner` calls `create-pool` with the following parameters.

* `launch-token-trait` : the trait reference of `project token` (e.g. ALEX)
* `payment-token-trait` : trait reference of `payment token` (e.g. sUSDT)
* `launch-owner` : address of the project that launches the `project token`
* `launch-tokens-per-ticket` : number of `project token` per `ticket`
* `price-per-ticket-in-fixed` : `payment token` deposit required per `ticket` (in 8-digit fixed notation)
* `activation-threshold` : number of tickets required to activate the launch
* `registration-start-height` : start (inclusive) block-height of registration period
* `registration-end-height` : end (inclusive) block-height of registration period
* `claim-end-height` : end (inclusive) block-height of claim (i.e. lottery) period
* `apower-per-ticket-in-fixed` : `apower` required for each `ticket` (can be a list, in 8-digit fixed notation)
* `registration-max-tickets` : maximum number of `ticket` an address can register
* `fee-per-ticket-in-fixed` : listing `fee` charged by the platform as percentage of `price-per-ticket-in-fixed` (in 8-digit fixed notation)

**Launch price** of `project token` in `payment token` = `launch-tokens-per-ticket` / `price-per-ticket-in-fixed`

## Project Token Transfer

Once the pool is created, and before the registration period starts, `launch-owner` must call `add-to-position` with the following parameters:

* `launch-id` : id of the launch
* `tickets` : number of `tickets` `launch-owner` wants to allocate to
* `launch-token-trait` : trait reference of `project token`

**Total raise** for `project token` = `tickets` x `launch-tokens-per-ticket` x Launch price of `token`.

Upon calling `add-to-position`, (`tickets` x `launch-tokens-per-ticket`) of `project token` will be transferred from `launch-owner` to the contract.

## Registration

Participants call `register` with the following parameters:

* `launch-id` : id of the launch
* `tickets` : number of `ticket` the participant wants to register
* `payment-token-trait` : trait reference of `payment token`

Assuming (1) registration period has started, (2) registration period has not ended and (3) the participant is not already registered, the contract will register the participant and:

* burn the required number of `apower` from the participant's wallet, and
* transfer `tickets` x `price-per-ticket-in-fixed` of `payment token` from the participant's wallet to the contract.

## Lottery

The lottery will be drawn one block after the registration ended. The claim/lottery period ends at `claim-end` block-height.

`contract-owner` or `launch-owner` will draw the lottery off-chain using the [prescribed rule](what-is-the-launchpad.md#e255) and call `claim` repeatedly with the following parameters

* `launch-id` : id of the launch
* `input` : list of participants who won the lottery (up to 200)
* `launch-token-trait` : trait reference of `project token`
* `payment-token-trait` : trait reference of `payment token`

Upon calling `claim`, assuming (1) registration period has ended, (2) not all tickets are already won, (3) the launch is activated and (4) the claim/lottery period has not ended yet, the contract will verify the validity of `input` and

* transfer the proceeds of `payment token`, net of `fee`, to `launch-owner`,
* transfer `fee` to the platform, and
* transfer the appropriate number of `project token` to the winners.

Refund will be processed by either `contract-owner` or `launch-owner` in a similar manner to the claim.

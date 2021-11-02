# Error Codes

## General Error

General error starts with 1000.

| Error                      | Code |                Description               |
| -------------------------- | :--: | :--------------------------------------: |
| not-authorized-err         | 1000 |            Unauthorised access           |
| internal-function-call-err | 1001 | function call error in the same contract |

## Pool Error

Pool errors starts with 2000.

| Error                         | Code |                Description               |
| ----------------------------- | :--: | :--------------------------------------: |
| pool-already-exists-err       | 2000 |           Pool Already Existing          |
| invalid-pool-err              | 2001 |           Accesing invalid Pool          |
| no-liquidity-err              | 2002 |          Liquidity insufficient          |
| invalid-liquidity-err         | 2003 |    Accesing Invalid Liquidity feature    |
| too-many-pools-err            | 2004 |      Exceeded maximum number of pool     |
| no-fee-x-err                  | 2005 |       Insufficient fee for Token-X       |
| no-fee-y-err                  | 2006 |       Insufficient fee for Token-Y       |
| invalid-token-err             | 2007 |          Accesing invalid Token          |
| invalid-balance-err           | 2008 |         Accesing invalid balance         |
| invalid-expiry-err            | 2009 |            expiry > max-expiry           |
| math-call-err                 | 2010 |          Math library call error         |
| already-expiry-err            | 2011 |       current block-height > expiry      |
| get-weight-fail-err           | 2012 |       get-weight fail on pool logic      |
| get-expiry-fail-err           | 2013 |       get-expiry fail on pool logic      |
| yield-token-equation-call-err | 2014 |    yield token equation calling error    |
| get-price-fail-err            | 2015 |              get-price error             |
| dy-bigger-than-available-err  | 2016 |      thrown if dy > balance-aytoken      |
| expiry-err                    | 2017 |         yield token expiry error         |
| stacking-in-progress-err      | 2018 |          stacking is in progress         |
| ltv-greater-than-one-err      | 2019 |         LTV > 100%, i.e. default         |
| ERR-EXCEED-MAX-SLIPPAGE       | 2020 |  Output exceeds maximum slippage target  |
| ERR-PRICE-LOWER-THAN-MIN      | 2021 |  Swap price is lower than specified min  |
| ERR-PRICE-GREATER-THAN-MAX    | 2022 | Swap price is greater than specified max |
| ERR-INVALID-POOL-TOKEN        | 2023 |  Provided pool token trait is incorrect  |
| ERR-AMOUNT-EXCEED-RESERVE     | 2024 |      Transfer amount exceeds reserve     |

## Vault Error

Vault errors starts with 3000.

| Error                               | Code |                    Description                   |
| ----------------------------------- | :--: | :----------------------------------------------: |
| transfer-failed-err                 | 3000 |              General transfer failed             |
| transfer-x-failed-err               | 3001 |            Transfer of Token-X failed            |
| transfer-y-failed-err               | 3002 |            Transfer of Token-Y failed            |
| insufficient-flash-loan-balance-err | 3003 |          Insufficient Flash Loan balance         |
| invalid-post-loan-balance-err       | 3004 |             Invalid Post loan balance            |
| user-execute-err                    | 3005 |         User execution error of Flashloan        |
| loan-transfer-failed-err            | 3006 |           Error on Transfer flash loan           |
| post-loan-transfer-failed-err       | 3007 |           Error on Transfer flash loan           |
| invalid-flash-loan-balance-err      | 3008 | Error on retrieving balance from flashloan token |

## Equation Error

Equation error starts with 4000.

| Error                    | Code |                       Description                      |
| ------------------------ | :--: | :----------------------------------------------------: |
| weight-sum-err           | 4000 |            Sum of weight should be always 1            |
| max-in-ratio-err         | 4001 |                     In ratio Error                     |
| max-out-ratio-err        | 4002 |                     Out ratio Error                    |
| math-call-err            | 4003 |      Error while calling math functions on library     |
| insufficient-balance-err | 4004 | input value is larger than current balance on equation |

## Math Error

Math error starts with 5000.

| Error                     | Code |              Description              |
| ------------------------- | :--: | :-----------------------------------: |
| percent-greater-than-one  | 5000 |        percent value exceeded 1       |
| SCALE\_UP\_OVERFLOW       | 5001 |        scale up overflow error        |
| SCALE\_DOWN\_OVERFLOW     | 5002 |       scale down overflow error       |
| ADD\_OVERFLOW             | 5003 |           addition overflow           |
| SUB\_OVERFLOW             | 5004 |          subtraction overflow         |
| MUL\_OVERFLOW             | 5005 |        multiplication overflow        |
| DIV\_OVERFLOW             | 5006 |           division overflow           |
| POW\_OVERFLOW             | 5007 |        power operation overflow       |
| MAX\_POW\_RELATIVE\_ERROR | 5008 |         max pow relative error        |
| X\_OUT\_OF\_BOUNDS        | 5009 |       parameter x out of bounds       |
| Y\_OUT\_OF\_BOUNDS        | 5010 |       parameter y out of bounds       |
| PRODUCT\_OUT\_OF\_BOUNDS  | 5011 |    product of x and y out of bounds   |
| INVALID\_EXPONENT         | 5012 |           exponential error           |
| OUT\_OF\_BOUNDS           | 5013 |      general out of bounds error      |
| fixed-point-err           | 5014 | catch-all for math-fixed-point errors |
|                           |      |                                       |

## Token Error

Token error starts with 6000.

| Error                   | Code |     Description     |
| ----------------------- | :--: | :-----------------: |
| get-symbol-fail-err     | 6000 |  get-symbol failed  |
| get-balance-fail-err    | 6001 |  get-balance failed |
| ERR-MINT-FAILED         | 6002 |     mint failed     |
| ERR-BURN-FAILED         | 6003 |     burn failed     |
| ERR-STX-TRANSFER-FAILED | 6004 | STX transfer failed |

## Oracle Error

Token error starts with 7000.

| Error                     | Code |                  Description                 |
| ------------------------- | :--: | :------------------------------------------: |
| get-oracle-price-fail-err | 7000 |               get-price failed               |
| err-token-not-in-oracle   | 7001 | Token cannot be found on given oracle source |

## Multisig Error <a href="oracle-error" id="oracle-error"></a>

‌

Multisig error starts with 8000.

| Error                        | Code | Description                                                              |
| ---------------------------- | ---- | ------------------------------------------------------------------------ |
| not-enough-balance-err       | 8000 | Proposer does not have enough balance to meet the condition of proposing |
| no-fee-change-err            | 8001 | Proposer attempts with unchanged value of fee                            |
| invalid-pool-token-err       | 8002 | Voters attempts to vote with wrong pool token                            |
| block-height-not-reached-err | 8003 | Current block height is not reached for starting / ending the proposal   |
|                              |      |                                                                          |

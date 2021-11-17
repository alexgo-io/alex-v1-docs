# Error Codes

## General Error

General error starts with 1000.

| Error              | Code |     Description     |
| ------------------ | :--: | :-----------------: |
| ERR-NOT-AUTHORIZED | 1000 | Unauthorised access |

## Pool Error

Pool errors starts with 2000.

| Error                        | Code |                Description               |
| ---------------------------- | :--: | :--------------------------------------: |
| ERR-POOL-ALREADY-EXISTS      | 2000 |           Pool Already Existing          |
| ERR-INVALID-POOL-ERR         | 2001 |           Accesing invalid Pool          |
| ERR-NO-LIQUIDITY             | 2002 |          Liquidity insufficient          |
| ERR-INVALID-LIQUIDITY        | 2003 |    Accesing Invalid Liquidity feature    |
| ERR-TOO-MANY-POOLS           | 2004 |      Exceeded maximum number of pool     |
| ERR-NO-FEE-X                 | 2005 |       Insufficient fee for Token-X       |
| ERR-NO-FEE-Y                 | 2006 |       Insufficient fee for Token-Y       |
| ERR-INVALID-TOKEN            | 2007 |          Accesing invalid Token          |
| ERR-INVALID-BALANCE          | 2008 |         Accesing invalid balance         |
| ERR-INVALID-EXPIRY           | 2009 |            expiry > max-expiry           |
| ERR-MATH-CALL                | 2010 |          Math library call error         |
| ERR-ALREADY-EXPIRED          | 2011 |       current block-height > expiry      |
| ERR-GET-WEIGHT-FAIL          | 2012 |       get-weight fail on pool logic      |
| ERR-GET-EXPIRY-FAIL-ERR      | 2013 |       get-expiry fail on pool logic      |
| ERR-GET-PRICE-FAIL           | 2015 |              get-price error             |
| ERR-DY-BIGGER-THAN-AVAILABLE | 2016 |      thrown if dy > balance-aytoken      |
| ERR-EXPIRY                   | 2017 |         yield token expiry error         |
| ERR-STACKING-IN-PROGRESS     | 2018 |          stacking is in progress         |
| ERR-LTV-GREATER-THAN-ONE     | 2019 |         LTV > 100%, i.e. default         |
| ERR-EXCEEDS-MAX-SLIPPAGE     | 2020 |  Output exceeds maximum slippage target  |
| ERR-PRICE-LOWER-THAN-MIN     | 2021 |  Swap price is lower than specified min  |
| ERR-PRICE-GREATER-THAN-MAX   | 2022 | Swap price is greater than specified max |
| ERR-INVALID-POOL-TOKEN       | 2023 |  Provided pool token trait is incorrect  |
| ERR-AMOUNT-EXCEED-RESERVE    | 2024 |      Transfer amount exceeds reserve     |
| ERR-TOO-MANY-TOKENS          | 2025 |        Too many stake-able tokens        |
| ERR-INVALID-TOKEN            | 2026 |      Token is not a stake-able token     |

## Vault Error

Vault errors starts with 3000.

| Error                               | Code |                    Description                   |
| ----------------------------------- | :--: | :----------------------------------------------: |
| ERR-TRANSFER-FAILED                 | 3000 |              General transfer failed             |
| ERR-TRANSFER-X-FAILED               | 3001 |            Transfer of Token-X failed            |
| ERR-TRANSFER-Y-FAILED               | 3002 |            Transfer of Token-Y failed            |
| ERR-INSUFFICIENT-FLASH-LOAN-BALANCE | 3003 |          Insufficient Flash Loan balance         |
| ERR-INVALID-POST-LOAN-BALANCE       | 3004 |             Invalid Post loan balance            |
| ERR-USER-EXECUTE                    | 3005 |         User execution error of Flashloan        |
| ERR-LOAN-TRANSFER-FAILED            | 3006 |           Error on Transfer flash loan           |
| ERR-POST-LOAN-TRANSFER-FAILED       | 3007 |           Error on Transfer flash loan           |
| ERR-INVALID-FLASH-LOAN              | 3008 | Error on retrieving balance from flashloan token |

## Equation Error

Equation error starts with 4000.

| Error                    | Code |                       Description                      |
| ------------------------ | :--: | :----------------------------------------------------: |
| ERR-WEIGHT-SUM           | 4000 |            Sum of weight should be always 1            |
| ERR-MAX-IN-RATIO         | 4001 |                     In ratio Error                     |
| ERR-MAX-OUT-RATIO        | 4002 |                     Out ratio Error                    |
| ERR-MATH-CALL            | 4003 |      Error while calling math functions on library     |
| ERR-INSUFFICIENT-BALANCE | 4004 | input value is larger than current balance on equation |

## Math Error

Math error starts with 5000.

| Error                        | Code |              Description              |
| ---------------------------- | :--: | :-----------------------------------: |
| ERR-PERCENT-GREATER-THAN-ONE | 5000 |        percent value exceeded 1       |
| SCALE\_UP\_OVERFLOW          | 5001 |        scale up overflow error        |
| SCALE\_DOWN\_OVERFLOW        | 5002 |       scale down overflow error       |
| ADD\_OVERFLOW                | 5003 |           addition overflow           |
| SUB\_OVERFLOW                | 5004 |          subtraction overflow         |
| MUL\_OVERFLOW                | 5005 |        multiplication overflow        |
| DIV\_OVERFLOW                | 5006 |           division overflow           |
| POW\_OVERFLOW                | 5007 |        power operation overflow       |
| MAX\_POW\_RELATIVE\_ERROR    | 5008 |         max pow relative error        |
| X\_OUT\_OF\_BOUNDS           | 5009 |       parameter x out of bounds       |
| Y\_OUT\_OF\_BOUNDS           | 5010 |       parameter y out of bounds       |
| PRODUCT\_OUT\_OF\_BOUNDS     | 5011 |    product of x and y out of bounds   |
| INVALID\_EXPONENT            | 5012 |           exponential error           |
| OUT\_OF\_BOUNDS              | 5013 |      general out of bounds error      |
| ERR-FIXED-POINT              | 5014 | catch-all for math-fixed-point errors |

## Token Error

Token error starts with 6000.

| Error                      | Code |     Description    |
| -------------------------- | :--: | :----------------: |
| ERR-GET-SYMBOL-FAIL        | 6000 |  get-symbol failed |
| ERR-GET-BALANCE-FIXED-FAIL | 6001 | get-balance failed |
| ERR-MINT-FAILED            | 6002 |     mint failed    |
| ERR-BURN-FAILED            | 6003 |     burn failed    |

## Oracle Error

Token error starts with 7000.

| Error                     | Code |                  Description                 |
| ------------------------- | :--: | :------------------------------------------: |
| ERR-GET-ORACLE-PRICE-FAIL | 7000 |               get-price failed               |
| ERR-TOKEN-NOT-IN-ORACLE   | 7001 | Token cannot be found on given oracle source |

## Multisig Error <a href="oracle-error" id="oracle-error"></a>

‌

Multisig error starts with 8000.

| Error                        | Code | Description                                                              |
| ---------------------------- | ---- | ------------------------------------------------------------------------ |
| ERR-NOT-ENOUGH-BALANCE       | 8000 | Proposer does not have enough balance to meet the condition of proposing |
| ERR-NO-FEE-CHANGE            | 8001 | Proposer attempts with unchanged value of fee                            |
| ERR-INVALID-POOL-TOKEN       | 8002 | Voters attempts to vote with wrong pool token                            |
| ERR-BLOCK-HEIGHT-NOT-REACHED | 8003 | Current block height is not reached for starting / ending the proposal   |

## Faucet Error

Faucet error starts with 9000.

| Error                    | Code | Description                              |
| ------------------------ | ---- | ---------------------------------------- |
| ERR-EXCEEDS-MAX-USE      | 9000 | User attempted more than max use allowed |
| ERR-USDA-TRANSFER-FAILED | 9001 | USDA transfer failed                     |
| ERR-WBTC-TRANSFER-FAILED | 9002 | WBTC transfer failed                     |
| ERR-STX-TRANSFER-FAILED  | 9003 | STX transfer failed                      |
| ERR-ALEX-TRANSFER-FAILED | 9004 | ALEX transfer failed                     |

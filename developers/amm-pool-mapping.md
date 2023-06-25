# AMM Pool Mapping

Below table maps the pool listed on [https://app.alexlab.co/pool](https://app.alexlab.co/pool) to smart contracts with the relevant parameters

## Pool Types

We have three smart contracts in production that provide AMM.

### Trading Pool

[Trading Pool](../trading-lending-and-borrowing/trading-pool.md) is the latest AMM smart contract that developers should use whenever possible.

Trading Pool implements Generalised Mean Equation and, with a suitable parameterisation, supports both risky pairs (i.e. $$x y=L$$), stable pairs (i.e. $$x +y=L$$) and any linear combination in-between (i.e. Curve).

Trading Pool is parameterised with a single parameter $$t$$. $$t$$ can be between 0 and 1, with $$t=1$$ being equivalent of constant product formula (i.e. Uniswap V2) and  $$t=0$$ being equivalent of constant sum formula (i.e. mStable). $$0<t <1$$ then gives a Curve-like formula.

#### Contract address

`SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.amm-swap-pool-v1-1`

### Fixed Weight Pool

Fixed Weight Pool is based on Balancer and allows to create AMM pools with custom (i.e. non equal-weight), fixed-weight, pools.

Fixed Weight Pool has been deprecated since the introduction of [Trading Pool](amm-pool-mapping.md#trading-pool).

#### Contract address

`SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.fixed-weight-pool-v1-01`

### Simple Weight Pool

Simple Weight Pool supports the constant product formula (i.e.  $$x y=L$$).

Simple Weight Pool has been deprecated since the introduction of [Trading Pool](amm-pool-mapping.md#trading-pool).

#### Contract address

`SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.simple-weight-pool-alex`

## AMM Pools

### STX-xBTC-1

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wbtc`
* `factor`: 1e8

### STX-sUSDT-1

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-susdt`
* `factor`: 1e8

### STX-ALEX-1

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.age000-governance-token`
* `factor`: 1e8

### ALEX-ATALEXV2-1

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.age000-governance-token`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.auto-alex-v2`
* `factor`: 1e8

### ALEX-DIKO-1

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.age000-governance-token`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wdiko`
* `factor`: 1e8

### STX-VIBES-1

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wvibes`
* `factor`: 1e8

### STX-ALEX-50-50

**Smart Contract**: [Fixed Weight Pool](amm-pool-mapping.md#fixed-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.age000-governance-token`
* `weight-x`: 0.5e8
* `weight-y`: 0.5e8

### STX-xBTC-50-50

**Smart Contract**: [Fixed Weight Pool](amm-pool-mapping.md#fixed-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wbtc`
* `weight-x`: 0.5e8
* `weight-y`: 0.5e8

### STX-xUSD-50-50

**Smart Contract**: [Fixed Weight Pool](amm-pool-mapping.md#fixed-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wxusd`
* `weight-x`: 0.5e8
* `weight-y`: 0.5e8

### xUSD-USDA-0.005

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wxusd`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wusda`
* `factor`: 0.005e8

### ALEX-USDA-50-50

**Smart Contract**: [Simple Weight Pool](amm-pool-mapping.md#simple-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.age000-governance-token`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wusda`

### STX-CORGI-1

**Smart Contract**: [Trading Pool](amm-pool-mapping.md#trading-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wcorgi`
* `factor`: 1e8

### STX-MIA-50-50

**Smart Contract**: [Fixed Weight Pool](amm-pool-mapping.md#fixed-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wmia`
* `weight-x`: 0.5e8
* `weight-y`: 0.5e8

### STX-NYCC-50-50

**Smart Contract**: [Fixed Weight Pool](amm-pool-mapping.md#fixed-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wstx`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wnycc`
* `weight-x`: 0.5e8
* `weight-y`: 0.5e8

### ALEX-BANABA-50-50

**Smart Contract**: [Simple Weight Pool](amm-pool-mapping.md#simple-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.age000-governance-token`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wban`

### ALEX-SLIME-50-50

**Smart Contract**: [Simple Weight Pool](amm-pool-mapping.md#simple-weight-pool)

#### Parameters

* `token-x`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.age000-governance-token`
* `token-y`: `SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.token-wslm`


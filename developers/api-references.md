# REST API

Front-end developers may use our REST API ([https://api.alexgo.io](https://api.alexgo.io)) to access the latest market data on ALEX.

## Pool

---

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/allswaps" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/pool_stats/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/pool_volume/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/volume_24h/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/volume_7d/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/pool_liquidity/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/liquidity/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/fee/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

## Stats

---

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/stats/tvl" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/stats/tvl/{token}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/stats/total_supply/{token}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

## Price

---

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/price/{token}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/pool_token_price/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/pool_token_stats" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/price_history/{token}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

## DEX

---

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/pairs" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/tickers" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/ticker/{ticker_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/orderbook/{ticker}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/historical_swaps/{pool_token_id}" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

## Coin-gecko

---

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v2/coin-gecko/pairs" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v2/coin-gecko/tickers" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

## Public

---

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/public/pairs" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

{% swagger src="https://api.alexgo.io/swagger-ui-yaml" path="/v1/public/amm-pool-stats" method="get" %}
[https://api.alexgo.io/swagger-ui-yaml](https://api.alexgo.io/swagger-ui-yaml)
{% endswagger %}

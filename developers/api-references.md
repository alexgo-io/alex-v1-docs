# REST API

Front-end developers may use our REST API ([https://api.alexgo.io](https://api.alexgo.io)) to access the latest market data on ALEX.

## Pool

---

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/allswaps" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/pool_stats/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/pool_volume/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/volume_24h/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/volume_7d/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/pool_liquidity/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/liquidity/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/fee/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

## Stats

---

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/stats/tvl" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/stats/tvl/{token}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/stats/total_supply/{token}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

## Price

---

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/price/{token}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/pool_token_price/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/pool_token_stats" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/price_history/{token}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

## Dex

---

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/pairs" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/tickers" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/ticker/{ticker_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/orderbook/{ticker}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/historical_swaps/{pool_token_id}" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

## Coin-gecko

---

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v2/coin-gecko/pairs" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v2/coin-gecko/tickers" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

## Public

---

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/public/pairs" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

{% swagger src="../.gitbook/assets/openapi.yaml" path="/v1/public/amm-pool-stats" method="get" %}
[../.gitbook/assets/openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

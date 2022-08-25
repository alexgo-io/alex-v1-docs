# API References

The following endpoints are available at [https://api.alexlab.co/](https://api.alexlab.co/).

Documentation is also available in json format at [https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json).

### General



{% swagger src="https://api.alexlab.co/swagger-api.json" path="undefined" method="undefined" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/pool_volume/{pool_token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/volume_24h/{token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/volume_7d/{token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/pool_liquidity/{pool_token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/liquidity/{token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/fee/{pool_token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/stats/tvl" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/stats/tvl/{token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/stats/total_supply/{token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/price/{token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/pool_token_price/{pool_token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/pool_token_stats" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

{% swagger src="https://api.alexlab.co/swagger-api.json" path="/v1/price_history/{token}" method="get" %}
[https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)
{% endswagger %}

### DEX

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/pairs" summary="" %}
{% swagger-description %}
Returns all existing pairs
{% endswagger-description %}

{% swagger-response status="200: OK" description="Succesfully returning all existing pairs" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/tickers" summary="" %}
{% swagger-description %}
Returns all markets statistics for the last 24 hours
{% endswagger-description %}

{% swagger-response status="200: OK" description="uccesfully returning all markets statistics for the last 24 hours" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/historical_swaps" summary="" %}
{% swagger-description %}
Returns all existing historical trades
{% endswagger-description %}

{% swagger-parameter in="path" name="ticker" required="true" %}
ticker with delimiter between different cryptoassets eg. ALEX_WSLM
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of recent block heights to be returned, default value is 1000
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Succesfully returning all historical trades of certain ticker" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/orderbook" summary="" %}
{% swagger-description %}
Returns orderbook information
{% endswagger-description %}

{% swagger-parameter in="path" name="ticker_id" required="true" %}
ticker with delimiter between different cryptoassets eg. ALEX_WSLM
{% endswagger-parameter %}

{% swagger-parameter in="query" name="depth" required="true" type="Int" %}
specifies value of depth on either side of bid/ask
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Return orderbook information of queried ticker" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

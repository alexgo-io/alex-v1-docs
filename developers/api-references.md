# API References

The following endpoints are available at [https://api.alexlab.co/](https://api.alexlab.co/).

Documentation is also available in json format at [https://api.alexlab.co/swagger-api.json](https://api.alexlab.co/swagger-api.json)



### General

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/allswaps" summary="/v1/allswaps" %}
{% swagger-description %}
Returns all existing swaps with status
{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/pool_stats" summary="/v1/pool_stats/{pool_token}" %}
{% swagger-description %}
Returns pool stats
{% endswagger-description %}

{% swagger-parameter in="path" name="pool_token" required="true" %}
pool token of swap pool eg. fwp-alex-wslm
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/pool_volume" summary="/v1/pool_volume/{pool_token}" %}
{% swagger-description %}
Returns all 24 hour pool volumes in time series
{% endswagger-description %}

{% swagger-parameter in="path" name="pool_token" required="true" %}
pool token of swap pool eg. fwp-alex-wslm
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/volume_24h" summary="/v1/volume_24h/{token}" %}
{% swagger-description %}
Returns daily volumes of token in time series
{% endswagger-description %}

{% swagger-parameter in="path" name="token" required="true" %}
name of token eg. age000-governance-token"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/volume_7d" summary="/v1/volume_7d/{token}" %}
{% swagger-description %}
Returns daily volumes of token in time series
{% endswagger-description %}

{% swagger-parameter in="path" name="token" required="true" %}
name of token eg. age000-governance-token"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/pool_liquidity" summary="/v1/pool_liquidity/{pool_token}" %}
{% swagger-description %}
Returns liquidity in time series
{% endswagger-description %}

{% swagger-parameter in="path" name="pool_token" required="true" %}
pool token of swap pool eg. fwp-alex-wslm
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/liquidity" summary="/v1/liquidity/{token}" %}
{% swagger-description %}
Returns liquidity of token in time series
{% endswagger-description %}

{% swagger-parameter in="path" name="token" required="true" %}
name of token eg. age000-governance-token"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/fee" summary="/v1/fee/{pool_token}" %}
{% swagger-description %}
Returns pool fee in time series
{% endswagger-description %}

{% swagger-parameter in="path" name="pool_token" required="true" %}
pool token of swap pool eg. fwp-alex-wslm
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/stats/tvl" summary="/v1/stats/tvl" %}
{% swagger-description %}
Returns total TVL(total value locked) value of ALEX platform
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/stats/tvl" summary="/v1/stats/tvl/{token}" %}
{% swagger-description %}
Returns token TVL(total value locked) in time series
{% endswagger-description %}

{% swagger-parameter in="path" name="token" required="true" %}
name of token eg. age000-governance-token"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/stats/total_supply" summary="/v1/stats/total_supply/{token}" %}
{% swagger-description %}
Returns total supply of queried token eg. age000-governance-token"
{% endswagger-description %}

{% swagger-parameter in="path" name="token" required="true" %}
name of token eg. age000-governance-token"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/price" summary="/v1/price/{token}" %}
{% swagger-description %}
Returns price of token
{% endswagger-description %}

{% swagger-parameter in="path" name="token" required="true" %}
name of token eg. age000-governance-token"
{% endswagger-parameter %}

{% swagger-parameter in="query" name="offset" type="Int" %}
Specifies the offset of data to be returned, default value is 0
{% endswagger-parameter %}

{% swagger-parameter in="query" name="limit" type="Int" %}
Specifies number of data to be returned, default value is 10
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/pool_token_stats" summary="/v1/pool_token_stats" %}
{% swagger-description %}
Returns pool token price value of ALEX platform
{% endswagger-description %}

{% swagger-response status="200: OK" description="" %}
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

### DEX

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/pairs" summary="/v1/pairs" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/tickers" summary="/v1/tickers" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/historical_swaps" summary="/v1/historical_swaps/{ticker}" %}
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

{% swagger method="get" path="" baseUrl="https://api.alexlab.co/v1/orderbook" summary="/v1/orderbook/{ticker_id}" %}
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

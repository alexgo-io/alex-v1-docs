# Using the Orderbook

ALEX Orderbook is available at [https://app.alexlab.co/orderbook](https://app.alexlab.co/orderbook).

## Register with the Orderbook

Users register with the Orderbook and deploy their own wallet to which they make deposits and withdrawals.

At the registration, users provide their public keys so the Orderbook can verify their signed orders.

## Place buy / sell orders gas-free

Users can place buy / sell orders of any tokens supported by the Orderbook without paying transaction fees. They can cancel orders any time. Orderbook ensures that the user wallet balance is sufficient.

All orders are signed by the originating users and the orders are validated with the users’ public keys before settlement.

## Orders are matched and confirmed

Order Matching Engine continuously matches buy and sell orders. Matched orders are sent to Exchange Contract.

## Orders are settled in aggregate

Exchange Contract validates the matched orders, aggregate them and settle in a single transaction. It updates the order fill, which is then used by Order Matching Engine to optimise the order matching.

## REST API

Once user registration is complete, you can use our REST API ([https://stxdx-api.alexlab.co/](https://stxdx-api.alexlab.co/)) to interact with the Orderbook.

For example, [b20-trading-bot](https://github.com/alexgo-io/b20-trading-bot) uses our API to automate grid trading on the Orderbook.

For more details, please contract your ALEX representatives.

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/login" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/accounts:getByPrincipal" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/accounts/{uid}" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/accounts/pnl/{uid}/today" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/assets" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/accounts/{uid}:updateSettings" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/accounts/{uid}:sendVerificationEmail" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/accounts/{uid}:verifyEmail" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/accounts/{uid}/fund-history" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orders" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orders" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orders:make" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orderbook/{market}" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orders/{order_hash}" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orders/{order_hash}:cancel" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orders:cancel_all" method="post" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orderbook:tickers" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/fills" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orderbook/{market}:fills" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orderbook/{market}:trades" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orderbook/{market}:trading-view" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orderbook:overview" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

{% swagger src="https://stxdx-api.alexlab.co/swagger-ui-json" path="/v1/orderbook/{market}:overview" method="get" %}
[https://stxdx-api.alexlab.co/swagger-ui-json](https://stxdx-api.alexlab.co/swagger-ui-json)
{% endswagger %}

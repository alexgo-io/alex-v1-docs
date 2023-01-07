# Using the Orderbook

ALEX Orderbook is available at [https://orderbook.alexlab.co](https://orderbook.alexlab.co).

## Register with the Orderbook

Users register with the Orderbook and deploy their own wallet to which they make deposits and withdrawals.

At the registration, users provide their public keys so the Orderbook can verify their signed orders.

## Place buy / sell orders gas-free

Users can place buy / sell orders of any tokens supported by the Orderbook without paying transaction fees. They can cancel orders any time. Orderbook ensures that the user wallet balance is sufficient.

All orders are signed by the originating users and the orders are validated with the users’ public keys before settlement.

## Orders are matched and confirmed

Order Matching Engine continuously matches buy and sell orders. Matched orders are sent to Exchange Contract.&#x20;

## Orders are settled in aggregate

Exchange Contract validates the matched orders, aggregate them and settle in a single transaction. It updates the order fill, which is then used by Order Matching Engine to optimise the order matching.&#x20;

\
\


\
\


\
\


\
\

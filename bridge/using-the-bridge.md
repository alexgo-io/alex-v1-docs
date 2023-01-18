# Using the Bridge

ALEX Bridge is available at [https://app.alexlab.co/bridge](https://app.alexlab.co/bridge).

## Fee

There are two layers of fees to use the Bridge.

10bps is deducted from the source asset, floored at $1 for xUSD => USDC, which goes towards covering the operational cost of the Bridge (including gas fee on both Ethereum and Stacks for the destination asset transfer). Any balances go to ALEX DAO Treasury.

Wrapped charges a variable fee based on the amount of the transfer. You can find their fee schedule [here](https://app.gitbook.com/s/wFwxyo05u2LDudAxiuAv/).

## From USDC to xUSD

You must have a valid Ethereum address, a valid Stacks address and connected to ALEX Bridge.

<figure><img src="../.gitbook/assets/Screenshot 2023-01-01 at 8.23.51 PM.png" alt=""><figcaption></figcaption></figure>

On the left, you can see the balance of USDC held at your wallet. Enter the amount of USDC you want to bridge over to Stacks, and, on the right, you will see the amount of xUSD you will receive (net of [fees](using-the-bridge.md#fee)).

If you click on Details, you will see the details of the [fee](using-the-bridge.md#fee) and the time expected to take to bridge over USDC to xUSD.

Clicking on Wrap, you can confirm the transfer on the familiar MetaMask pop-up window. Please note that once you confirm the transfer and the transaction confirmed, it is not reversible.

After that, you will see another pop-up from MetaMask that asks you to sign the transaction, togehter with your Stacks address ("ToAddress"). This is important because this is the cryptographic proof that you want to send the resulting xUSD to that Stacks address of yours.

Once you complete these two steps, you can monitor the bridge transfer on the [Order History](using-the-bridge.md#order-history).

## From xUSD to USDC

Just like from USDC to xUSD, you must have a valid Ethereum address, a valid Stacks address and connected to ALEX Bridge.

<figure><img src="../.gitbook/assets/Screenshot 2023-01-01 at 8.37.04 PM.png" alt=""><figcaption></figcaption></figure>

On the left, you can see the balance of xUSD held at your wallet. Enter the amount of xUSD you want to bridge over to Ethereum, and, on the right, you will see the amount of USDC you will receive (net of [fees](using-the-bridge.md#fee)).

If you click on Details, you will see the details of the [fee](using-the-bridge.md#fee) and the time expected to take to bridge over xUSD to USDC.

Clicking on Unwrap, you can confirm the details of the transfer (including your Ethereum address), following which you will confirm the transaction on the familiar Hiro (or Xverse) pop-up window. Please note that once you confirm the transfer and the transaction confirmed, it is not reversible.

Once you complete these two steps, you can monitor the bridge transfer on the [Order History](using-the-bridge.md#order-history).

## Order History

You can use Order History to monitor the current transfers as well as reviewing historical transfers. You will also find order hashes, which would be useful if you have issues with any of the orders and need to speak to the community managers for help.

<figure><img src="../.gitbook/assets/Screenshot 2023-01-01 at 8.41.30 PM.png" alt=""><figcaption></figcaption></figure>


---
description: Bitcoin Bridge is not a typical cross-chain bridge
---

# What is the Bitcoin Bridge

<figure><img src="../.gitbook/assets/Screenshot 2023-11-13 at 12.55.07 PM (1).png" alt=""><figcaption></figcaption></figure>

Bitcoin Bridge is a key component of ALEX Layer that abstracts the difference between L1 and L2 from the user experience, i.e. providing the "native-like” Bitcoin DeFi experience on L1, whereby users can use native BTC or L1 assets issued on Bitcoin to interact with L2 smart contracts.

Bitcoin Bridge is fundamentally more secure and faster than a typical cross-chain bridge.&#x20;

Bitcoin Bridge does not require validators to validate to the destination chain that an action was executed on the source chain, because Stacks as the smart contract layer of Bitcoin reads the Bitcoin state natively, i.e. Bitcoin Bridge can independently validate Bitcoin events.

And Stacks forks when Bitcoin forks, meaning no re-org risk typical in cross-chain bridge, and no extra waiting time for bridge users.\
\
This fundamental difference allows Bitcoin Bridge to integrate L2 smart contracts with L1 assets, not merely transferring assets between L1 and L2.

#### BRC20 Bridge latest deployment

[https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.brc20-bridge-endpoint-v1-03?chain=mainnet](https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.brc20-bridge-endpoint-v1-03?chain=mainnet)

#### BTC Bridge latest deployment (developer preview)

[https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.btc-bridge-endpoint-dev-preview-1?chain=mainnet](https://explorer.hiro.so/txid/SP3K8BC0PPEVCV7NZ6QSRWPQ2JE9E5B6N3PA0KBR9.btc-bridge-endpoint-dev-preview-1?chain=mainnet)\

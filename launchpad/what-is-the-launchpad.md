# What is the Launchpad

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Every project needs a platform to launch their project tokens and our Launchpad is a lottery-based token launch platform that uses a hybrid off-chain / on-chain model.

All user funds related transactions are on chain. These include the submission of the Launch ticket together with the required payment tokens, the distribution or claim of the project tokens won, and the distribution or refund of the payment tokens in the case of not winning the lottery.

What is computationally expensive, i.e. the lottery drawing itself, is done off chain, but submitted to our on-chain contract for verification. The lottery drawing is also transparent and verifiable as it is based on the random seed available from the latest block height, i.e. anyone can independently verify the winners' table.

## Off-chain lottery system <a href="#e255" id="e255"></a>

Lottery system is based on a random number generator called “linear congruential generator”, which is [one of the oldest and best-known pseudorandom number generator algorithms](https://en.wikipedia.org/wiki/Linear\_congruential\_generator).

It starts by firstly drawing a verifiable random function seed (“seed”) from the Stacks block following the registration end. The seed is then

1. multiplied by a constant (_134775813_),
2. added to another constant (_1_), and, finally,
3. the modulus of its outcome by another constant (_4294967296_) is calculated to serve as the next random number.

This random number is then scaled by `maximum step size`, the ratio of the total number of registered tickets over the total number of tickets to be won (i.e. oversubscription ratio), multiplied by 1.5 times `walk resolution`.

This `maximum step size` helps ensure the fair-ness of the lottery system.

If a random number drawn falls between a particular `walk resolution` within a block that corresponds to a ticket held by a user, the ticket is determined to have won the lottery.

To generate the next random number, we move to the end of the `walk resolution` of the current random number, and draw another random number using the above linear congruential generator with the current random number as its seed.

The process is repeated until we reach the end of the chain, with the tickets upon whose corresponding `walk resolution` random numbers fell having won the lottery.

These “winners” are first determined off-chain, without the need for any on-chain events, thereby greatly increasing the overall efficiency of the lottery system.

## On-chain verification <a href="#a897" id="a897"></a>

In order to ensure these “winners” are not tempered with or generated in error, upon submission of the list to process claim (whereby the “winners” receive the project tokens) and refund, the contract verifies independently that the list is correct using the same process outlined above, before triggering on-chain events (i.e. processing claims and refunds).

This hybrid on-chain/off-chain lottery system therefore allows ALEX to create a Launchpad that combines the efficiency/speed of off-chain processing, while maintaining verifiability/immutability of on-chain processing.

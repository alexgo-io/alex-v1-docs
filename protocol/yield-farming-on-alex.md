# 流动性挖矿

ALEX 可以在不同期限的 ayToken \(收益代币\) 之间 \(即使是使用静态抵押品的方式\) 提供高 LTV。比如，以 ayToken-1y \(1年期收益代币\) 作为抵押品，可以提供高达90%的LTV来铸造 ayToken-O/N \(隔夜收益代币\)，这就类似于使用隔夜借贷购买长期资产。这使得流动性挖矿更具吸引力，矿工可以重复购买 ayToken-1y \(1年期收益代币\)，并将其用作抵押品来铸造和出售 ayToken-O/N \(隔夜收益代币\)，再购买 ayToken-1y \(1年期收益代币\) 这一过程，以实现高 APY \(这里假设 ayToken-1y APY &gt; ayToken-O/N APY\)。

![Yield Farming Use Case](https://raw.githubusercontent.com/alexgo-io/alex-v1/main/diagrams/use-case-yield-farming.svg)

例如，Rachel 花费100 BTC 并购买了ayBTC-1y \(一年期比特币收益代币\)。她用它作为抵押品以 90% LTV 借入 ayBTC-O/N \(隔夜比特币收益代币\)。然后她卖掉借来的 ayBTC-O/N 并购买更多的 ayBTC-1y。她再用它作为抵押品以 90% LTV 借入ayBTC-O/N。然后重复这个过程。

如果 1y APY 为 10%，O/N APY 为 5%，上述流动挖矿可以获得接近 55% 的APY。

在数学上，

$$r=\frac{1+r{1y}}{1-LTV}-\left(1+\frac{r{1d}}{365}\right)^{365}\left(\frac{LTV}{1-LTV}\right)-1$$

更详细的说明，请参阅[此处](https://docs.google.com/spreadsheets/d/1L-52KHFl7O_h22Fg4gpZKczdPEXuAt5yAh2gX3BQP58/edit?usp=sharing)。


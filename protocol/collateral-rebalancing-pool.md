# 抵押品动态平衡池

抵押品动态平衡池 \(CRP\) 使用[加权方程](https://docs.alexgo.io/v/cn/protocol/platform-architecture-that-supports-ecosystem-development)并在 Token \(基础代币\) 和抵押品之间动态重新平衡。

CRP 动态地重新平衡抵押品以确保铸造的 ayToken \(收益代币，也即贷款\) 保持偿付能力，尤其是在不利的市场环境中 \(即贷款价值不超过抵押品价值\)。这种动态重新平衡机制，加上对关键参数 \(包括 LTV 和波动率假设\) 的谨慎选择，使 ALEX 消除了对平台使用者的清算需求。任何剩余的缺口风险 \(当 CRP 无法完全解决时\) 都可以通过维持强大的储备基金来解决。 

例如，ayBTC / USD 池 \(即以美元为抵押品借入 BTC\)，将会对 BTC 价格的上行风险实施动态对冲策略 \(即随着 BTC 现货上涨而卖出美元并买入 BTC，反之亦然\)。由此产生的美元和 BTC 之间的重新平衡将由套利者通过套利交易来实现。详细的数值模拟请参阅[此处](https://docs.google.com/spreadsheets/d/1nSg6L30iedpk_rLhq3E7Zv8ct3Myb_D8oWmgJzwtwtw/edit?usp=sharing)。借款人实际上也是流动性提供者 \(LP\)，接收 ayBTC 作为 PoolToken \(池代币\)，他们提供的美元抵押品将自动转换为一篮子美元和 BTC。

抵押品池动态平衡的主要好处在于我们可以更好的优化 LTV 并完全去掉清算风险 \(使用适当的 LTV 和储备基金\)。这也允许我们将所有 ayToken \(收益代币\) 聚合到一个池中 \(而不是每个借款人一个单独池\)。

抵押品池动态平衡的主要缺点是在还款时收回的抵押品的美元价值将与原始价值不同 \(由于重新平衡造成的无偿损失\)。

当借款人通过提供适当的抵押品铸造 ayToken \(收益代币\) 时，抵押品被转换为一篮子抵押品和 Token \(基础代币\)，权重由 CRP 确定。 CRP 根据现行 LTV 确定权重，并使用以下公式：

$$
\begin{split}
&w_{Token}=N\left(d_{1}\right)\\
&w_{Collateral}=\left(1-w_{Token}\right)\\
&d_{1}= \frac{1}{\sigma\sqrt{t}}\left[\ln\left(\frac{LTV_{t}}{LTV_{0}}\right) + t\times\left(APY_{Token}-APY_{Collateral} + \frac{\sigma^2}{2}\right)\right]
\end{split}
$$

一些读者可能会注意到上述公式与 [Black & Scholes delta](https://en.wikipedia.org/wiki/Black–Scholes_model) 的相似性，因为它确实如此。 CRP 本质上实现了基础代币/抵押品 \(Token / Collat​​eral\) 看涨期权的 delta 复制策略，当 LTV 走高时购买更多 Token \(基础代币\)，反之亦然。


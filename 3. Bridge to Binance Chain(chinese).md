## 跨链进行时—如何实现everiToken与币安链资产转账

毫无疑问，去中心化交易所一直是业界备受关注的话题，无论是从监管还是资产交易角度，都是行业发展的必经之路。

币安作为全球最大的交易所在全球运营方面投入的资金不少，尤其对于技术安全投入的资金更是不容小觑。币安成立去中心化交易所DEX (Decentralized Exchange)，除了在安全性上的考虑之外，也是趋势所致。

## everiToken与Binance Chain的代币协议

那么，全球有这么多公有链，目前的去中心化解决方案仍然是以某一条链为主，比如币安的DEX采用COSMOS，与everiToken资产的协议完全不同，所以不同链上的资产跨链转账成为问题。

EVT作为everiToken公有链上原生代币，可以作为网络中交易的费用或者作为奖励给那些打包交易的节点等等。Binance Chain目前支持BEP2兼容的通证，EVT在Binance Chain上的原生代币为[**EVT-49B**](https://explorer.binance.org/asset/EVT-49B)。为了实现EVT在Binance Chain的交易，我们设计了[Bridge](https://github.com/everitoken/evt-bnb-dex-bridge)。
通过这个Bridge，两条链的原生代币可以简单且安全的实现转换。

## Bridge的工作原理

everiToken与Binance Chain之间的Bridge是由everiToken官方运营。它主要功能是监听两条链上已经确认的交易，如果任何交易符合跨链交易的特征，Bridge会对其进行相应的操作。

Bridge定义了两个地址，分别在everiToken和Binance Chain上，并作为交换地址。Bridge将监视这两个地址上的确认交易、检查其有效性并完成交换过程。

跨连交易地址分别为：

everiToken公有链：`EVT5NYzcUjJEpGunhJJZmw8r2qph2uDhGBkhCWSJTp3Nv3VQ7aLT3`

Binance Chain：`bnb1v3fl4kuwuhzf3g7ghscsq7uzmu5dw50waseptd`

> 通过Bridge交换EVT和EVT-49B将会产生1个EVT的手续费。

## 如何将EVT交换成**EVT-49B**

如果你拥有EVT，想要在Binance Chain上交易，你必须要将EVT转换为**EVT-49B**，流程如下：

> 你必须要有Binance Chain的账户，如果没有的花，可以通过此[链接申请](https://www.binance.org/en/create)

1. 在任何可以兼容EVT的钱包里点击“转账”选项
2. 在**To（接收地址）**字段，输入`EVT5NYzcUjJEpGunhJJZmw8r2qph2uDhGBkhCWSJTp3Nv3VQ7aLT3`
3. 在**Memo（备忘录）**字段，输入自己在Binance Chain的地址（注意是以BNB开头）
4. 在**Amount（数量）**字段，输入交易的数量

如果这笔交易在everiToken公有链上确认，Bridge会将相应金额转移到您的Binance chain帐户（在步骤3. Memo中填入的地址）。

转换率为1：1，意味着你从everiToken上转出的数额将与Binance chain上收到的**EVT-49B**的数额相等。

## 如何将**EVT-49B**提至任何兼容EVT的钱包？

如果你在Binance chain上拥有**EVT-49B**，希望转换为EVT。流程如下：

1. 在任何兼容BEP-2协议的钱包里点击“转账“功能
2. 在**To（接收地址）**字段，输入`bnb1v3fl4kuwuhzf3g7ghscsq7uzmu5dw50waseptd`
3. 在**Memo（备忘录）**字段，输入自己EVT的地址（注意是以EVT开头）
4. 在**Amount（数量）**字段，输入交易的数量

如果在Binance chain此笔交易确认，Bridge会将相应的金额转移到您的everiToken帐户（在步骤3. Memo中填入的地址），转换率仍为1：1。

## 总结

通过Bridge，everiToken公共链和Binance Chain之间实现了无摩擦以及透明的代币交换。将代币转移至上文提到的相应的跨链交易地址，并在Memo（备忘录中）填入目标地址。Bridge会完成代币交换交易。

## 相关链接

* [Bridge的开源代码](https://github.com/everitoken/evt-bnb-dex-bridge)
* [EVT-49B在Binance Chain详细信息](https://explorer.binance.org/asset/EVT-49B)

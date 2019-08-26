## Staking in everiToken

everiToken will support staking start with 4.x series. This will allow the adoption of a new consensus consisting in **DPOS**(Delegated Proof-Of-Stake) and **POS**(Proof-Of-Stake). Token holders will be able to stake their EVTs in order to vote for the validators. The top voted 21 validators will be then selected as the **Block Producer** nodes. Other **non-Block Producers** validators will be able to receive and verify the blocks and take part in an on-chain random number generation.

Operating a validator node in everiToken is similar to running an investment fund.  Every validator's initial net value will always be equal to 1 EVT per share. The the operation of staking will be similar to purchasing these shares at net value. For example, if you stake 1000 EVTs into one validator whose net value is 1 EVT per share, then you will get 1000 shares worth of that validator.

There's no limitation for register a new validator and every stake holder could do it. But only if it have staked __100'000__ shares it can start receiving staking bonus.

All the staked EVTs will go into a unique pool called **Stake Pool**, ROI of the whole pool will be depend on two factors: 1) the total amount of stake pool and 2) the time since the Stake Pool was created. As the amount of the Stake Pool growing, and time of its creation passes, the ROI will decrease.

ROI table of the stake pool:

![ROI](/imgs/developers/ROIs.png)

And ROI formula is: ![formula-1](/imgs/developers/staking-formula1.svg)

> Currently **r** is 50000, **t** is -0.67, **q** is 10 and **w** is -0.001; **A** is the total amount of staked EVTs and **D** is total days since the Stake Pool was created.

As for the validators, if the node is kept running efficiently, then he will receive the ROI of stake pool to his net value every several hours. This means that the **yield curve** of one validator representing his node operation stability and the stake holders have the  intention to select for excellent validators that meet these criteria which will help him earn more EVTs.

Unlike the stake operation which purchases shares in a proper amount, unstaking is similar to withdrawing shares. The difference of the net value between your purchase and withdraw is the profit for each share. For example, if you withdraw 1000 shares at 1.5 EVT per share net value and your purchased net value is 1 EVT per share, your final profit is equal to 500 EVTs.

### Validators

Everyone can use `newvalidator` action to register a new validator and will need to stake at least 100,000 EVTs into the new validator before taking part into the network and get profit form the Stake Pool. Every stake holders can stake EVTs into the new validator to purchase its shares. There will be some additional ROI for validators whose staked amount is smaller.

### Stake Holders

Every EVT holders can stake their tokens into any valid validators by pushing `staketkns` action. And there're two types of options when staking: active and fixed. Active stake shares can be withdrawn at any time while fixed shares are promised to be locked for a period of time. According to the length of period, the ROI will be higher the longer the shares are locked.

Additional ROI formula is: ![formula-2](/imgs/developers/staking-formula2.svg)

> Currently **r** is 150 and **t** is 5; **D** is the total days promised to lock

`unstaketkns` action is used to withdraw the active shares. In case of fixed shares, the action required is `toactivetkns` action to convert into active shares. 
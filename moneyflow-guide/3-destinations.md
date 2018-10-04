---
description: (this section is still under construction)
---

# 4 - Destinations

Destination – an WeiReceiver element, that collect money. There are three types of destinations: funds, expenses, table \(table will be discussed in the next section\).

Destination, as weiReceiver, have a **getMinWeiNeeded\(\)** and **getTotalWeiNeed\(\)** functions. In normal case, destinations needs money, if it do not receive it yet. Function **getMinWeiNeeded\(\)** will differ from **getTotalWeiNeed\(\)** in two situations:  expense is a fund or relative, so min need is always 0, but total can be more then 0. There is another one method – **isNeedsMoney\(\)**, which response true or false.

Each destination have **flush\(\)** and **flushTo\(\)** functions, that will flush collected funds to owner or any address respectively.

## 1. Funds

Fund – destination, that can accept any amount, until cap if reached \(if cap exist\). So, unlike absolute expense, it will not revert, if you send less money, then it totally need, and, unlike relative expense, total amount do not relative from a flow amount. You can use it like a normal fund, but there are some special cases: fund can be used as buffer between customers payments and the main money flow of an organization, or you can collect additional funds, that comes to a moneyflow.

#### Parameters

1. **Capped** or **Uncapped** Uncapped collects any amount of funds Capped collects funds until some amount is reached.
2. **Pull model** or **Push model** _\(not implemented\)_ Currently all Thetta funds have “pull model”, i.e. when you need to call some method to get money out of it.
3. **Untokenized** or **Tokenized** _\(not implemented\)_ Currently all Thetta funds are “untokenized”. Tokenized fund means that you need tokens in order to pull money out of it \(rewards subsystem\).

TODO - add pic

TODO - add code \(same as on pic\)

## 2. Expenses

Expense – destination, that accept only **getTotalWeiNeeded\(currentFlow\)** amount.  Current flow make sense only for relative expense – an expense, that calculate how much it need from a current flow \(e.g., relative expense needs 2%, so if current flow is 100 ETH, it will need a 2 ETH\). Relative expense **getMinWeiNeeded\(\)** is always 0. Absolute expense **getMinWeiNeeded\(\)==getTotalWeiNeeded\(\)** and do not depends on currentFlow**.** 

There is a WeiExpense basic class with a following parameters:

1. **Absolute** or **Relative** Absolute amount is set in ETH \(e.g., 1.2 ETH\), relative is set in parts per million \(e.g., 10000 is 1%\).
2. **One-time only** or **Periodic** One-time receives funds only once, periodic can receive funds many times but only once during the specified interval.
3. **Accumulate debt** or not For example, we have periodic absolute expense with 1 ETH every day. How much element need, if there is no payment in the last three days?  If debt is accumulated, the answer is 3, else is 1. So, it make sense only for periodic expenses.
4. **Period** _\(in hours\)_ ****Parameter for periodic expenses. Expense will not need amount, if in current period amount is received, and will need, if not. If period set to 0, this expense will need money always. For example, it can be useful to aggregate fee from every transaction \(absolute or relative periodic expense with 0\).
5. **NeededWei** Parameter for absolute expense. 
6. **PartsPerMillion** Parameter for relative expense.

But there are some inherited from WeiExpense classes, such as:

1. WeiAbsoluteExpense
2. WeiAbsoluteExpenseWithPeriod
3. WeiRelativeExpense
4. WeiRelativeExpenseWithPeriod

Each of them have a predefined values, such as WeiAbsoluteExpense have PartsPerMillion==0, Period==0,  isPeriodic==false, isAccumulateDebt==false. So, you should use one of this classes instead of WeiExpense.

TODO - add pic

TODO - add code \(same as on pic\)


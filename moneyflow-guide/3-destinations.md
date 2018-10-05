---
description: (this section is still under construction)
---

# 3 - Destinations

Destination – is a terminal **WeiReceiver** element, i.e. does not send money further. There are three types of destinations: 

* **WeiFund**
* **WeiExpense**
* **MoneyflowTable**

Each destination has _flush\(\)_ and _flushTo\(\)_ ****functions, that will send collected funds to the owner or any address respectively.

## 1. WeiFund

**WeiFund** – is a destination, that can accept any amount, until cap if reached \(if cap is set\). 

{% hint style="info" %}
Fund can not be relative.
{% endhint %}

\(TODO\) You can use it as regular fund, but there are some special cases: fund can be used as buffer between customers payments and the main money flow of an organization, or you can collect additional funds, that flows into a moneyflow.

{% hint style="info" %}
Unlike absolute expense, fund will not throw an exception, if you send less money than is needed
{% endhint %}

### Fund types

1. **OneTimeFund** have a cap and will receive money until fulfilled. You can get money from there with _flush\(\)_ or _flushTo\(\)_ functions.
2. **PeriodicFund** have a cap too, but after fulfilled it will needs money again in a next period. There are two kinds of  **PeriodicFund**: **PeriodicFundDoNotAccumulateDebt** and **PeriodicFundAccumulateDebt**. Дуеы consider the difference between them in the following example.  Every month a **PeriodicFund** collects 10 eth. In this month it was collected only 7 eth. So, in the next month, if you request _getTotalWeiNeeded\(\)_, **PeriodicFundAccumulateDebt** will response with a 17 \(10 + 3\) eth, while **PeriodicFundDoNotAccumulateDebt** will response 3 eth.

TODO - add pic

{% code-tabs %}
{% code-tabs-item title="WeiFund example.sol" %}
```javascript
// contract WeiFund is IWeiReceiver, IDestination, Ownable {}
// constructor(uint _neededWei, bool _isPeriodic, bool _isAccumulateDebt, uint _periodHours) public {
WeiFund fund = new WeiFund(10*eth, false, false, 0);

uint totalNeed = fund.getTotalWeiNeeded(10*eth); // 10*eth
uint minNeed = fund.getMinWeiNeeded(); // 0
uint isNeed = fund.isNeedsMoney(); // true

fund.processFunds.value(3*eth)(3*eth);
fund.processFunds.value(3*eth)(3*eth);

uint totalNeed = fund.getTotalWeiNeeded(4*eth); // 4*eth
uint minNeed = fund.getMinWeiNeeded(); // 0
uint isNeed = fund.isNeedsMoney(); // true
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 2. WeiExpense

**WeiExpense** – destination, that accept only _getTotalWeiNeeded\(currentFlow\)_ ****amount.    
Current flow make sense only for relative expense – an expense, that calculate how much it need from a current flow \(e.g., relative expense needs 2%, so if current flow is 100 ETH, it will need a 2 ETH\). Relative expense _getMinWeiNeeded\(\)_ ****is always 0. Absolute expense _getMinWeiNeeded\(\)==getTotalWeiNeeded\(\)_ and do not depends on currentFlow**.** 

### WeiExpense types

As a result, moneyflow subsystem has different built-in expense types that you can use:

1. **WeiAbsoluteExpense**
   1. neededWei – an amount in wei, that this element will accept. If amount will be more or less, call will be reverted.
2. **WeiAbsoluteExpenseWithPeriod**
   1. neededWei – same as for **WeiAbsoluteExpense**;
   2. period – time interval in hours, to collect money. If time has passed, element will continue to collect money until it receives. After that a new period will began. If period set to 0, then element will need money always, and debt accumulating is restricted;
   3. isAccumulateDebt – boolean parameter. If true and if neededWei not collected in current period, the not received amount will be transferred to the next period. 
3. **WeiRelativeExpense**
   1. partsPerMillion – the number of millionths parts of the money flow that an element will aggregate.
4. **WeiRelativeExpenseWithPeriod**
   1. partsPerMillion – same as for **WeiRelativeExpense**;
   2. period – same as for **WeiAbsoluteExpenseWithPeriod**;
   3. isAccumulateDebt – same as for **WeiAbsoluteExpenseWithPeriod**;

TODO - add pic

TODO: code

{% code-tabs %}
{% code-tabs-item title="WeiExpense example.sol" %}
```javascript
WeiAbsoluteExpense absExpense = new WeiAbsoluteExpense(2*eth); // 2 eth
WeiRelativeExpense relExpense = new WeiRelativeExpense(10000); // 1%
WeiRelativeExpenseWithPeriod relExpenseWithPeriod = new WeiRelativeExpenseWithPeriod(10000, 0, false); // 1%, needs money on each transaction, debt is not accumulating
WeiAbsoluteExpenseWithPeriod absExpenseWithPeriod = new WeiAbsoluteExpenseWithPeriod(10*eth, 24, true); // 10 eth, every 24 hours, debt is accumulating

uint totalNeed = absExpense.getTotalWeiNeeded(2*eth); // 2 eth
uint minNeed = absExpense.getMinWeiNeeded(); // 2 eth
uint isNeed = absExpense.isNeedsMoney(); // true

absExpense.processFunds.value(2*eth)(2*eth);
```
{% endcode-tabs-item %}
{% endcode-tabs %}






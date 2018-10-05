---
description: (this section is still under construction)
---

# 3 - Destinations

Destination – is a terminal **WeiReceiver** element, i.e. does not send money further. There are three types of destinations: 

* WeiFund
* WeiExpense
* MoneyflowTable

Each destination has **flush\(\)** and **flushTo\(\)** functions, that will send collected funds to the owner or any address respectively.

## 1. Fund

Fund – is a destination, that can accept any amount, until cap if reached \(if cap is set\). 

{% hint style="info" %}
Fund can not be relative.
{% endhint %}

\(TODO\) You can use it like a normal fund, but there are some special cases: fund can be used as buffer between customers payments and the main money flow of an organization, or you can collect additional funds, that flows into a moneyflow.

{% hint style="info" %}
Unlike absolute expense, fund will not throw an exception, if you send less money than is needed
{% endhint %}

### Fund types

1. OneTimeFund -  collects 0 to cap;  if collected amount is equals to the cap -&gt; stop collecting more.   
2. Periodic
   1. PeriodicFundDoNotAccumulateDebt  
   2. PeriodicFundAccumulateDebt 

TODO - add pic  
  
TODO:

{% code-tabs %}
{% code-tabs-item title="WeiFund example.sol" %}
```javascript
// contract WeiFund is IWeiReceiver, IDestination, Ownable {}
// constructor(uint _neededWei, bool _isPeriodic, bool _isAccumulateDebt, uint _periodHours) public {


```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 2. Expense

Expense – destination, that accept only **getTotalWeiNeeded\(currentFlow\)** amount.    
Current flow make sense only for relative expense – an expense, that calculate how much it need from a current flow \(e.g., relative expense needs 2%, so if current flow is 100 ETH, it will need a 2 ETH\). Relative expense **getMinWeiNeeded\(\)** is always 0. Absolute expense **getMinWeiNeeded\(\)==getTotalWeiNeeded\(\)** and do not depends on currentFlow**.** 

There is a WeiExpense basic class with a following parameters:

### Expenses Type

As a result, moneyflow subsystem has different built-in expense types that you can use:

1. **WeiAbsoluteExpense**
   1. neededWei - TODO
2. **WeiAbsoluteExpenseWithPeriod**
   1. neededWei - TODO
   2. period - TODO
   3. isAccumulateDebt - TODO
3. **WeiRelativeExpense**
   1. partsPerMillion - TODO
4. **WeiRelativeExpenseWithPeriod**
   1. partsPerMillion - TODO
   2. period - TODO
   3. isAccumulateDebt - TODO

TODO - add pic

TODO: code

{% code-tabs %}
{% code-tabs-item title="WeiExpense example.sol" %}
```javascript
WeiAbsoluteExpense office = new WeiAbsoluteExpense(5**17);
WeiRelativeExpense bonus1 = new WeiRelativeExpense(10000);

```
{% endcode-tabs-item %}
{% endcode-tabs %}






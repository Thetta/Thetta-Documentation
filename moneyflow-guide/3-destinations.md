# 3 - Destinations

Destination – is a terminal **WeiReceiver** element, i.e. does not send money further.   
There are three basic types of destinations: 

* **WeiFund**s:
  * **WeiFund**
  * **WeiFundWithPeriod**
  * **WeiFundWithPeriodSliding**
* **WeiExpense**s:
  * **WeiAbsoluteExpense**
  * **WeiAbsoluteExpenseWithPeriod**
  * **WeiAbsoluteExpenseWithPeriodSliding**
  * **WeiRelativeExpense**
  * **WeiRelativeExpenseWithPeriod**
  * **WeiRelativeExpenseWithPeriodSliding**
* **MoneyflowTable**

Each destination has _flush\(\)_ and _flushTo\(\)_ ****functions, that will send collected funds to the owner or any address respectively.

## 1. WeiFund

**WeiFund** – is a destination that can accept any amount, until the cap is reached \(if cap is set\). 

Funds can be used as a buffer between customer payments and the main money flow of an organization, or just to store money \(e.g.: reserve fund of a corporation\).

{% hint style="info" %}
Fund can not be relative.
{% endhint %}

{% hint style="info" %}
Unlike absolute expense, fund can accept less ETH than it needs.
{% endhint %}

TODO - add pic of a generic fund

### WeiOneTimeFund

**WeiOneTimeFund** has a cap only.

{% code-tabs %}
{% code-tabs-item title="" %}
```javascript
WeiOneTimeFund fund = new WeiOneTimeFund(10*eth);

// 100 ETH is a maximum amount that we can send to the element in the current case
// Still fund needs only 10 ETH
uint totalNeed = fund.getTotalWeiNeeded(100*eth); // 10*eth
uint minNeeded = fund.getMinWeiNeeded(); // 0
uint isNeeded = fund.isNeedsMoney(); // true

// send 3 ETH
fund.processFunds.value(3*eth)(100*eth);
// send 3 ETH again
fund.processFunds.value(3*eth)(100*eth);

// now fund needs 4 more ETH
totalNeeded = fund.getTotalWeiNeeded(100*eth); // 4*eth
minNeeded = fund.getMinWeiNeeded(); // 0
isNeeded = fund.isNeedsMoney(); // true
```
{% endcode-tabs-item %}

{% code-tabs-item title=undefined %}
```

```
{% endcode-tabs-item %}
{% endcode-tabs %}

### WeiPeriodicFund / WeiPeriodicFundSliding 

Periodic funds ****have a cap and a period.  
Imagine, we set cap to 10 ETH and period to 1 week:

```javascript
uint periodHours = 24 * 7;        // 1 week
WeiPeriodicFund fund = new WeiPeriodicFund(10*eth, periodHours);
WeiPeriodicFundSliding fund2 = new WeiPeriodicFundSliding(10*eth, periodHours);

// 100 ETH is a maximum amount that we can send to the element in the current case
// Still fund needs only 10 ETH
uint totalNeed = fund.getTotalWeiNeeded(100*eth); // 10*eth
uint minNeeded = fund.getMinWeiNeeded(); // 0
uint isNeeded = fund.isNeedsMoney(); // true

// send 3 ETH to both funds
fund.processFunds.value(3*eth)(100*eth);
fund2.processFunds.value(3*eth)(100*eth);

// now fund needs 7 more ETH
totalNeeded = fund.getTotalWeiNeeded(100*eth); // 7*eth
minNeeded = fund.getMinWeiNeeded(); // 0
isNeeded = fund.isNeedsMoney(); // true

// 1 week is passed here
// fund needs 10 ETH only
totalNeeded = fund.getTotalWeiNeeded(100*eth); // 10*eth
// but fund2 needs 17 ETH
totalNeeded = fund2.getTotalWeiNeeded(100*eth); // 17*eth

minNeeded = fund.getMinWeiNeeded(); // 0
isNeeded = fund.isNeedsMoney(); // true
```

## 2. WeiExpense

**WeiExpense** – is a destination that accepts only _getTotalWeiNeeded_\(\) ****amount of ETH.  
    
TODO - add pic

{% code-tabs %}
{% code-tabs-item title="WeiExpense example.sol" %}
```javascript
// TODO - show what is different between (as in the fund example)
WeiAbsoluteExpense
WeiAbsoluteExpenseWithPeriod
WeiAbsoluteExpenseWithPeriodSliding
WeiRelativeExpense
WeiRelativeExpenseWithPeriod
WeiRelativeExpenseWithPeriodSliding


```
{% endcode-tabs-item %}
{% endcode-tabs %}






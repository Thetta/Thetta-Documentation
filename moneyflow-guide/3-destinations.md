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

**WeiOneTimeFund** has a cap only. You can flush from a **WeiOneTimeFund** multiple times while sum is collecting, and it will not affect to cap, because total received value stores in a _totalWeiReceived_  variable, and when you flush, this variable not changes.

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

fund.flush();

// Flush was not affected to getTotalWeiNeeded
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
uint periodHours = 24 * 7; // 1 week
WeiPeriodicFund fund = new WeiPeriodicFund(10*eth, periodHours);
WeiPeriodicFundSliding fundWithSliding = new WeiPeriodicFundSliding(10*eth, periodHours);

// 100 ETH is a maximum amount that we can send to the element in the current case
// Still fund needs only 10 ETH
uint totalNeed = fund.getTotalWeiNeeded(100*eth); // 10*eth
uint minNeeded = fund.getMinWeiNeeded(); // 0
uint isNeeded = fund.isNeedsMoney(); // true

// send 3 ETH to both funds
fund.processFunds.value(3*eth)(100*eth);
fundWithSliding.processFunds.value(3*eth)(100*eth);

// now fund needs 7 more ETH
totalNeeded = fund.getTotalWeiNeeded(100*eth); // 7*eth
minNeeded = fund.getMinWeiNeeded(); // 0
isNeeded = fund.isNeedsMoney(); // true

// 1 week is passed here

// fund needs 10 ETH only
totalNeeded = fund.getTotalWeiNeeded(100*eth); // 10*eth
// but fundWithSliding needs 17 ETH
totalNeeded = fundWithSliding.getTotalWeiNeeded(100*eth); // 17*eth

```

## 2. WeiExpense

**WeiExpense** – is a destination that accepts only _getTotalWeiNeeded_\(\) ****amount of ETH.  
    
TODO - add pic

{% code-tabs %}
{% code-tabs-item title="WeiExpense example.sol" %}
```javascript
uint neededWei = 5*eth;
uint periodInHours = 24;

WeiAbsoluteExpense absoluteExpense = new WeiAbsoluteExpense(neededWei);
WeiAbsoluteExpenseWithPeriod absoluteExpenseWithPeriod = new WeiAbsoluteExpenseWithPeriod(neededWei, periodInHours);
WeiAbsoluteExpenseWithPeriodSliding absoluteExpenseWithPeriodSliding = new WeiAbsoluteExpenseWithPeriodSliding(neededWei, periodInHours);

absoluteExpense.getTotalWeiNeeded(100*eth); // 5*eth
absoluteExpense.getMinWeiNeeded(); // 5*eth
absoluteExpense.isNeedsMoney(); // true

absoluteExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 5*eth
absoluteExpenseWithPeriod.getMinWeiNeeded(); // 5*eth
absoluteExpenseWithPeriod.isNeedsMoney(); // true

absoluteExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 5*eth
absoluteExpenseWithPeriodSliding.getMinWeiNeeded(); // 5*eth
absoluteExpenseWithPeriodSliding.isNeedsMoney(); // true

// SEND MONEY
absoluteExpense.processFunds.value(5*eth)(5*eth);
absoluteExpenseWithPeriod.processFunds.value(5*eth)(5*eth);
absoluteExpenseWithPeriodSliding.processFunds.value(5*eth)(5*eth);

// AND THEN
absoluteExpense.getTotalWeiNeeded(100*eth); // 0
absoluteExpense.getMinWeiNeeded(); // 0
absoluteExpense.isNeedsMoney(); // false

absoluteExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 0
absoluteExpenseWithPeriod.getMinWeiNeeded(); // 0
absoluteExpenseWithPeriod.isNeedsMoney(); // false

absoluteExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 0
absoluteExpenseWithPeriodSliding.getMinWeiNeeded(); // 0
absoluteExpenseWithPeriodSliding.isNeedsMoney(); // false

// TWO PERIODS PASSED
absoluteExpense.getTotalWeiNeeded(100*eth); // 0
absoluteExpense.getMinWeiNeeded(); // 0
absoluteExpense.isNeedsMoney(); // false

absoluteExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 5*eth
absoluteExpenseWithPeriod.getMinWeiNeeded(); // 5*eth
absoluteExpenseWithPeriod.isNeedsMoney(); // true

absoluteExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 10*eth
absoluteExpenseWithPeriodSliding.getMinWeiNeeded(); // 10*eth
absoluteExpenseWithPeriodSliding.isNeedsMoney(); // true
```
{% endcode-tabs-item %}
{% endcode-tabs %}

```javascript
uint partsPerMillion = 50000;
uint periodInHours = 24;

WeiRelativeExpense relativeExpense = new WeiRelativeExpense(partsPerMillion);
WeiRelativeExpenseWithPeriod relativeExpenseWithPeriod = new WeiRelativeExpenseWithPeriod(partsPerMillion, periodInHours);
WeiRelativeExpenseWithPeriodSliding relativeExpenseWithPeriodSliding = new WeiRelativeExpenseWithPeriodSliding(partsPerMillion, periodInHours);

relativeExpense.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpense.getMinWeiNeeded(); // 0
relativeExpense.isNeedsMoney(); // true

relativeExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpenseWithPeriod.getMinWeiNeeded(); // 0
relativeExpenseWithPeriod.isNeedsMoney(); // true

relativeExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpenseWithPeriodSliding.getMinWeiNeeded(); // 0
relativeExpenseWithPeriodSliding.isNeedsMoney(); // true

// SEND MONEY
relativeExpense.processFunds.value(5*eth)(100*eth);
relativeExpenseWithPeriod.processFunds.value(5*eth)(100*eth);
relativeExpenseWithPeriodSliding.processFunds.value(5*eth)(100*eth);

// AND THEN
relativeExpense.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpense.getMinWeiNeeded(); // 0
relativeExpense.isNeedsMoney(); // true

relativeExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 0
relativeExpenseWithPeriod.getMinWeiNeeded(); // 0
relativeExpenseWithPeriod.isNeedsMoney(); // false

relativeExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 0
relativeExpenseWithPeriodSliding.getMinWeiNeeded(); // 0
relativeExpenseWithPeriodSliding.isNeedsMoney(); // false

// TWO PERIODS PASSED
relativeExpense.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpense.getMinWeiNeeded(); // 0
relativeExpense.isNeedsMoney(); // true

relativeExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpenseWithPeriod.getMinWeiNeeded(); // 0
relativeExpenseWithPeriod.isNeedsMoney(); // true

relativeExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 10*eth
relativeExpenseWithPeriodSliding.getMinWeiNeeded(); // 0
relativeExpenseWithPeriodSliding.isNeedsMoney(); // true
```




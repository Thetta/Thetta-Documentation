# 3 - Destinations

Destination – is a terminal **WeiReceiver** element, i.e. does not send money further.   
There are following basic types of destinations: 

* **WeiExpense**'s:
  * **WeiAbsoluteExpense**
  * **WeiAbsoluteExpenseWithPeriod**
  * **WeiAbsoluteExpenseWithPeriodSliding**
  * **WeiRelativeExpense**
  * **WeiRelativeExpenseWithPeriod**
  * **WeiRelativeExpenseWithPeriodSliding**
* **MoneyflowTable**

Each destination has _flush\(\)_ and _flushTo\(\)_ ****functions, that will send collected funds to the owner or any address respectively.

## 1. WeiAbsoluteExpense
**WeiAbsoluteExpense** have 2 parameters: totalWeiNeeded, minAmount, period

### 1.1. WeiAbsoluteExpense with minAmount==0

**WeiAbsoluteExpense** with minAmount==0 – is a destination that can accept any amount, until the cap is reached. 

Funds can be used as a buffer between customer payments and the main money flow of an organization, or just to store money \(e.g.: reserve fund of a corporation\).

{% hint style="info" %}
Unlike absolute expense with minAmount==totalWeiNeeded, this one can accept less ETH than it needs.
{% endhint %}

```javascript
WeiAbsoluteExpense expense10 = new WeiAbsoluteExpense(10*eth, 0);
WeiAbsoluteExpense expenseInfinite = new WeiAbsoluteExpense(1000000000*eth, 0);

// 10 ETH is a maximum amount that we can send to the element in the current case
// Still expense10 needs only 10 ETH
expense10.getTotalWeiNeeded(100*eth); // 10*eth
expense10.getMinWeiNeeded(100*eth); // 0
expense10.isNeedsMoney(); // true

expenseInfinite.getTotalWeiNeeded(100*eth); // 100*eth
expenseInfinite.getMinWeiNeeded(100*eth); // 0
expenseInfinite.isNeedsMoney(); // true

// send 3 ETH
expense10.processFunds.value(3*eth)(100*eth);
// send 3 ETH again
expense10.processFunds.value(3*eth)(100*eth);

// now expense10 needs 4 more ETH
expense10.getTotalWeiNeeded(100*eth); // 4*eth
expense10.getMinWeiNeeded(100*eth); // 0
expense10.isNeedsMoney(); // true

// send 3 ETH to expenseInfinite
expenseInfinite.processFunds.value(3*eth)(100*eth);

expenseInfinite.getTotalWeiNeeded(100*eth); // 100*eth
expenseInfinite.getMinWeiNeeded(100*eth); // 0
expenseInfinite.isNeedsMoney(); // true

// flash
expense10.flush();

// Flush was not affected to getTotalWeiNeeded
expense10.getTotalWeiNeeded(100*eth); // 4*eth
expense10.getMinWeiNeeded(100*eth); // 0
expense10.isNeedsMoney(); // true

// flash
expenseInfinite.flush();

// Flush was not affected to getTotalWeiNeeded
expenseInfinite.getTotalWeiNeeded(100*eth); // 100*eth
```

TODO - add pic 

## 1.2. WeiAbsoluteExpense, WeiAbsoluteExpenseWithPeriod, WeiAbsoluteExpenseWithPeriodSliding with minAmount==totalWeiNeeded

**WeiExpense** with minAmount==totalWeiNeeded – is a destination that accepts only _getTotalWeiNeeded_\(\) amount of ETH.  
    
TODO - add pic

{% code-tabs %}
{% code-tabs-item title="WeiExpense example.sol" %}
```javascript
uint neededWei = 5*eth;
uint periodInHours = 24;

WeiAbsoluteExpense absoluteExpense = new WeiAbsoluteExpense(neededWei, neededWei);
WeiAbsoluteExpenseWithPeriod absoluteExpenseWithPeriod = new WeiAbsoluteExpenseWithPeriod(neededWei, neededWei, periodInHours);
WeiAbsoluteExpenseWithPeriodSliding absoluteExpenseWithPeriodSliding = new WeiAbsoluteExpenseWithPeriodSliding(neededWei, neededWei, periodInHours);

absoluteExpense.getTotalWeiNeeded(100*eth); // 5*eth
absoluteExpense.getMinWeiNeeded(100*eth); // 5*eth
absoluteExpense.isNeedsMoney(); // true

absoluteExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 5*eth
absoluteExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 5*eth

// SEND MONEY
absoluteExpense.processFunds.value(5*eth)(5*eth);
absoluteExpenseWithPeriod.processFunds.value(5*eth)(5*eth);
absoluteExpenseWithPeriodSliding.processFunds.value(5*eth)(5*eth);

// AND THEN
absoluteExpense.getTotalWeiNeeded(100*eth); // 0
absoluteExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 0
absoluteExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 0

// TWO PERIODS PASSED
absoluteExpense.getTotalWeiNeeded(100*eth); // 0
absoluteExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 5*eth
absoluteExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 10*eth
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## 1.3. WeiAbsoluteExpense with minAmount > 0 and minAmount < totalWeiNeeded
There is one additional requirement: totalWeiNeeded must be a multiple of minAmount.

```javascript
WeiSplitter splitter = new WeiSplitter();

WeiAbsoluteExpense expense1 = new WeiAbsoluteExpense(500*eth, 1000*eth);
WeiAbsoluteExpense expense2 = new WeiAbsoluteExpense(200*eth, 800*eth);
WeiAbsoluteExpense expense3 = new WeiAbsoluteExpense(500*eth, 1500*eth);

splitter.addChild(expense1);
splitter.addChild(expense2);
splitter.addChild(expense3);

expense1.getTotalWeiNeeded(3300*eth); // 1000*eth
expense2.getTotalWeiNeeded(3300*eth); // 800*eth
expense3.getTotalWeiNeeded(3300*eth); // 1500*eth
splitter.getTotalWeiNeeded(3300*eth); // 3300*eth

splitter.getTotalWeiNeeded(100*eth) // 0*eth
splitter.getTotalWeiNeeded(200*eth) // 200*eth
splitter.getTotalWeiNeeded(300*eth) // 200*eth
splitter.getTotalWeiNeeded(500*eth) // 500*eth
splitter.getTotalWeiNeeded(400*eth) // 400*eth
splitter.getTotalWeiNeeded(800*eth) // 700*eth
splitter.getTotalWeiNeeded(900*eth) // 900*eth
splitter.getTotalWeiNeeded(2800*eth) // 2800*eth
splitter.getTotalWeiNeeded(3300*eth) // 3300*eth
splitter.getTotalWeiNeeded(3500*eth) // 3300*eth

splitter.processFunds(700*eth);

splitter.getTotalWeiNeeded(100*eth) // 0*eth
splitter.getTotalWeiNeeded(200*eth) // 200*eth
splitter.getTotalWeiNeeded(300*eth) // 200*eth

splitter.getTotalWeiNeeded(2500*eth) // 2100*eth
splitter.getTotalWeiNeeded(2600*eth) // 2600*eth
splitter.getTotalWeiNeeded(2700*eth) // 2600*eth
splitter.getTotalWeiNeeded(3300*eth) // 2600*eth
splitter.getTotalWeiNeeded(3500*eth) // 2600*eth

splitter.processFunds(900*eth);

splitter.getTotalWeiNeeded(100*eth) // 0*eth
splitter.getTotalWeiNeeded(200*eth) // 200*eth
splitter.getTotalWeiNeeded(500*eth) // 200*eth
splitter.getTotalWeiNeeded(600*eth) // 200*eth
splitter.getTotalWeiNeeded(700*eth) // 700*eth
splitter.getTotalWeiNeeded(900*eth) // 700*eth
splitter.getTotalWeiNeeded(1100*eth) // 700*eth
splitter.getTotalWeiNeeded(1200*eth) // 1200*eth
splitter.getTotalWeiNeeded(1300*eth) // 1200*eth
splitter.getTotalWeiNeeded(1700*eth) // 1700*eth
splitter.getTotalWeiNeeded(1800*eth) // 1700*eth
splitter.getTotalWeiNeeded(3500*eth) // 1700*eth

splitter.processFunds.value(200*eth)(200*eth);
splitter.processFunds.value(1500*eth)(1500*eth);
```

## 2. WeiRelativeExpense, WeiRelativeExpenseWithPeriod, WeiRelativeExpenseWithPeriodSliding

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
relativeExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 5*eth

// SEND MONEY
relativeExpense.processFunds.value(5*eth)(100*eth);
relativeExpenseWithPeriod.processFunds.value(5*eth)(100*eth);
relativeExpenseWithPeriodSliding.processFunds.value(5*eth)(100*eth);

// AND THEN
relativeExpense.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 0
relativeExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 0

// TWO PERIODS PASSED
relativeExpense.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpenseWithPeriod.getTotalWeiNeeded(100*eth); // 5*eth
relativeExpenseWithPeriodSliding.getTotalWeiNeeded(100*eth); // 10*eth
```

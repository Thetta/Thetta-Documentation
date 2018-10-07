# 2 - Receivers

**WeiReceiver** is an abstract smart contract. Every moneyflow node is a **WeiReceiver**.   
It can send ETH further or be a terminal node. In the last case it is called "a destination". 

Notable functions:

* _processFunds_\(\) – use this payable function to send ETH to the node;
* _isNeedsMoney_\(\) – returns true if node can receive ETH;
* _getMinWeiNeeded_\(\) – returns a minimum amount of ETH, which node will accept;
* _getTotalWeiNeeded_\(\) – returns a maximum amount of ETH, which node will accept.

The basic algorithm for moneyflow is to first request _getTotalWeiNeed_\(\) ****from moneyflow entry point \(root node\), and then send ETH by calling _processFunds_\(\) ****function with the correct amount.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send money to it directly. Instead you should use  _processFunds_\(\) function.
{% endhint %}

### Absoulte vs Relative receivers

There are 2 types of WeiReceivers currently:

* Absolute - consumes absolute amounts of ETH, e.g.: 1 ETH;
* Relative - consumes relative amounts of ETH, e.g.: 20%.

The difference between _getMinWeiNeeded_\(\) or _getTotalWeiNeed_\(\) ****exists only in cases when the current scheme has at least one relative receiver. If the scheme consists of absolute receivers only, the behavior of this two functions will be the same.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send more than _getTotalWeiNeeded_\(\) __or less than _getMinWeiNeeded_\(\)_._
{% endhint %}

### Code example

```javascript
// Consumes 10%
WeiRelativeExpense relativeExpense = new WeiRelativeExpense(100000);
bool isNeeds = relativeExpense.isNeedsMoney(); // true
uint minNeeded = relativeExpense.getMinWeiNeeded(); // 0 
uint totalNeeded = relativeExpense.getTotalWeiNeeded(100*eth); // 10*eth is 10% of 100 eth

// will consume 10 ETH
relativeExpense.processFunds.value(totalNeeded)(100*eth);

WeiAbsoluteExpense absoluteExpense = new WeiAbsoluteExpense(10*eth);
isNeeds = absoluteExpense.isNeedsMoney(); // true
minNeeded = absoluteExpense.getMinWeiNeeded(); // 10*eth 
totalNeeded = absoluteExpense.getTotalWeiNeeded(10*eth); // 10*eth

// will consume 10 ETH one time only (but not less/more)
absoluteExpense.processFunds.value(totalNeeded)(10*eth);
```


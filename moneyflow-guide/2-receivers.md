# 2 - Receivers

**WeiReceiver** is an abstract smart contract. Every moneyflow element is a **WeiReceiver**.   
It can send ETH further or be a terminal element. In the last case it is called "a destination". 

Notable functions:

* _processFunds_\(\) – payable function, that takes a current flow value and an amount;
* _isNeedsMoney_\(\) – function, that returns true if element need money and false if not.
* _getMinWeiNeeded_\(\) – function, that response with a minimal amount, what element can accept.
* _getTotalWeiNeeded_\(\) – function, that takes a current flow value and response with a maximal amount, what element can accept.

The basic algorithm for moneyflow is to first request _getMinWeiNeeded_\(\) or _getTotalWeiNeed_\(\) ****from moneyflow entry point \(root node\), and then send ETH by calling _processFunds_\(\) ****function with the correct amount.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send money to it directly. Instead you should use  _processFunds_\(\) function.
{% endhint %}

The difference between _getMinWeiNeeded_\(\) or _getTotalWeiNeed_\(\) ****exist in cases, where we have relative receiver. If moneyflow consist of absolute receivers only, the behavior of this two functions will be the same.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send more than _getTotalWeiNeeded\(\)_ or less than _getMinWeiNeeded\(\)._
{% endhint %}

### Code example

```javascript
WeiRelativeExpense relativeExpense = new WeiRelativeExpense(100000);
bool isNeed = relativeExpense.isNeedsMoney(); // true
uint minNeed = relativeExpense.getMinWeiNeeded(); // 0 
uint totalNeed = relativeExpense.getTotalWeiNeeded(100*eth); // 10*eth
relativeExpense.processFunds.value(totalNeed)(100*eth);

WeiAbsoluteExpense absoluteExpense = new WeiAbsoluteExpense(10*eth);
bool isNeed = absoluteExpense.isNeedsMoney(); // true
uint minNeed = absoluteExpense.getMinWeiNeeded(); // 10*eth 
uint totalNeed = absoluteExpense.getTotalWeiNeeded(10*eth); // 10*eth
absoluteExpense.processFunds.value(totalNeed)(10*eth);
```


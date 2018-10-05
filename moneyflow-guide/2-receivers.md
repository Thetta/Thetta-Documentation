# 2 - Receivers

**WeiReceiver** is an abstract smart contract. Every moneyflow element is a **WeiReceiver**.   
It can send ETH further or be a terminal element. In the last case it is called "a destination". 

Notable functions:

* _processFunds_\(\) ****- TODO
* _isNeedsMoney_\(\) - TODO
* _getMinWeiNeeded_\(\) - TODO
* _getTotalWeiNeeded_\(\) - TODO

The basic algorithm for moneyflow is to first request _getMinWeiNeeded_\(\) or _getTotalWeiNeed_\(\) ****from moneyflow entry point \(root node\), and then send ETH by calling _processFunds_\(\) ****function with the correct amount.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send money to it directly. Instead you should use  _processFunds_\(\) function.
{% endhint %}

The difference between _getMinWeiNeeded_\(\) or _getTotalWeiNeed_\(\) ****exist in cases, where we have relative receiver. If moneyflow consist of absolute receivers only, the behavior of this two functions will be the same.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send more than _getTotalWeiNeeded\(\)_ or less than _getMinWeiNeeded\(\)._
{% endhint %}

### Code example

TODO


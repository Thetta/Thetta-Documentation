# 2 - Receivers

**WeiReceiver** is an abstract smart contract. Every moneyflow element is a WeiReceiver.   
It can send ETH further or be a terminal element. In the last case it is called "**a destination**". 

Notable functions:

* _processFunds_\(\) ****- TODO
* _isNeedsMoney_\(\) - TODO
* _getPartsPerMillion_\(\) - TODO
* _getMinWeiNeeded_\(\) - TODO
* _getTotalWeiNeeded_\(\) - TODO

The basic algorithm for moneyflow is to first request **getMinWeiNeeded\(\)** or **getTotalWeiNeed\(\)** from moneyflow entry point \(root node\), and then send ETH by calling **processFunds\(\)** function with the correct amount.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send money to it directly. Instead you should use  processFunds\(\) function.
{% endhint %}

The difference between **getMinWeiNeeded\(\)** or **getTotalWeiNeed\(\)** exist in cases, where we have relative receiver. If moneyflow consist of absolute receivers only, the behavior of this two functions will be the same.

{% hint style="info" %}
**WeiReceiver** will throw an exception, if you send more ETH than is needed. 
{% endhint %}

The first two elements are **Splitters**,  the next two are **Destinations**, and the last one is a composition of splitters and expenses \(subclass of destinations\) in a single contract.




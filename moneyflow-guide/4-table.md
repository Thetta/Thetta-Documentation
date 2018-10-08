# 5 - Table

**MoneflowTable** is a **WeiReceiver**,  that implements the composition of Destination and Splitter elements within a single contract. Instead of deploying many smart contracts you can use a single **MoneyflowTable** to dramatically reduce gas usage.

**MoneflowTable,** as any moneyflow node, features a _processFunds_\(\) function.

Notable functions:

* _getElementBalance_\(\) – TODO;
* _isElementNeedsMoney_\(\) – returns true if node can receive ETH;
* _getMinWeiNeededForElement_\(\) – returns a minimum amount of ETH, which node will accept;
* _getTotalWeiNeededForElement_\(\) - TODO.
* _addAbsoluteExpense_\(\), 
* _addRelativeExpense_\(\), 
* _addTopdownSplitter_\(\), 
* _addUnsortedSplitter_\(\) 
* _addChild_\(\)
* _openElement_\(\)
* _closeElement_\(\).

{% hint style="info" %}
**MoneyflowTable** is distinguished from the **Destination** by the fact that it have _withdrawFundsFromElement_\(\) instead of flush\(\)/flushTo\(\).
{% endhint %}

### Code example

{% code-tabs %}
{% code-tabs-item title="Moneyflow table example.sol" %}
```javascript
MoneyflowTable moneyflowTable = new MoneyflowTable();

uint spendsId = moneyflowTable.addUnsortedSplitter();
uint salariesId = moneyflowTable.addUnsortedSplitter();
uint employee1Id = moneyflowTable.addAbsoluteExpense(10*eth, false, false, 0, outputForEmployee1);
uint employee2Id = moneyflowTable.addAbsoluteExpense(15*eth, false, false, 0, outputForEmployee2);
uint employee3Id = moneyflowTable.addAbsoluteExpense(8*eth, false, false, 0, outputForEmployee3);

moneyflowTable.addChild(spendsId, salariesId);
moneyflowTable.addChild(salariesId, employee1Id);
moneyflowTable.addChild(salariesId, employee2Id);
moneyflowTable.addChild(salariesId, employee3Id);

```
{% endcode-tabs-item %}
{% endcode-tabs %}


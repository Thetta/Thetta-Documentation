---
description: (still under construction)
---

# 5 - Table

**WeiTable** is a **WeiReceiver**,  that implements the composition of Destination and Splitter elements within a single contract. Instead of deploying many smart contracts you can use a single **WeiTable** to dramatically reduce gas usage.

**WeiTable,** as any moneyflow node, features a _processFunds_\(\) function.

{% hint style="info" %}
**WeiTable** has functions for working both with individual nodes and with all **WeiTable** entirely. **IWeiReceiver** functions are interpreted as referring to a node with id equal to zero. For example, weiTable._getMinWeiNeeded\(\)_ is equal to weiTable._getMinWeiNeededAt\(0\)_.
{% endhint %}

Notable functions:

* _balanceAt_\(\) – get balance, that associated with node.
* _isNeedsMoneyAt_\(\) – returns true if node can receive ETH;
* _getMinWeiNeededAt_\(\) – returns a minimum amount of ETH, which node will accept;
* _getTotalWeiNeededAt_\(\) – returns a maximum amount of ETH, which node will accept;
* _addAbsoluteExpense_\(\) – create absolute expense node; it should be connected to a splitter in the weiTable
* _addRelativeExpense_\(\) – create relative expense node; it should be connected to a splitter in the weiTable
* _addTopdownSplitter_\(\) – create topdown splitter node; it should be connected to a splitter in the weiTable
* _addUnsortedSplitter_\(\) – create unsorted splitter node; it should be connected to a splitter in the weiTable
* _addChildAt_\(\) – connect a node to a splitter or 0-node
* _openAt_\(\) – make node acceptable for transfer amounts. By default, all nodes are open
* _closeAt_\(\) – make node non-acceptable for transfer amounts. If node is closed, needed amount is 0, and processFunds will revert.
* flushToAt\(\) – flush funds, that associated with node, to the specified address.

{% hint style="info" %}
**WeiTable** is distinguished from the **Destination** by the fact that it have f_lushToAt\(\)_ instead of _flush_\(\)/_flushTo_\(\).
{% endhint %}

### Code example

{% code-tabs %}
{% code-tabs-item title="Moneyflow table example.sol" %}
```javascript
WeiTable table = new MoneyflowTable();

uint spendsId = table.addUnsortedSplitter();
uint salariesId = table.addUnsortedSplitter();
uint employee1Id = table.addAbsoluteExpense(10*eth, false, false, 0, outputForEmployee1);
uint employee2Id = table.addAbsoluteExpense(15*eth, false, false, 0, outputForEmployee2);
uint employee3Id = table.addAbsoluteExpense(8*eth, false, false, 0, outputForEmployee3);

table.addChild(spendsId, salariesId);
table.addChild(salariesId, employee1Id);
table.addChild(salariesId, employee2Id);
table.addChild(salariesId, employee3Id);

// TODO - send money

// TODO - check balances

// TODO - withdraw salary

```
{% endcode-tabs-item %}
{% endcode-tabs %}


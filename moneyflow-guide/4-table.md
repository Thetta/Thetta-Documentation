---
description: (still under construction)
---

# 5 - Table

**WeiTable** is a **WeiReceiver**,  that implements the composition of Destination and Splitter elements within a single contract. Instead of deploying many smart contracts you can use a single **WeiTable** to dramatically reduce gas usage.

**WeiTable,** as any moneyflow node, features a _processFunds_\(\) function.

{% hint style="info" %}
**WeiTable** has functions for working both with individual nodes and with all **WeiTable** entirely. **IWeiReceiver** functions are interpreted as referring to a node with id equal to zero. For example, weiTable._getMinWeiNeeded\(\)_ is equal to weiTable.node\__getMinWeiNeeded\(0\)_.
{% endhint %}

Notable functions:

* _node\_balance_\(\) – get balance, that associated with node.
* _node\_isNeedsMoney_\(\) – returns true if node can receive ETH;
* _node\_getMinWeiNeeded_\(\) – returns a minimum amount of ETH, which node will accept;
* _node\_getTotalWeiNeeded_\(\) – returns a maximum amount of ETH, which node will accept;
* _addAbsoluteExpense_\(\) – create absolute expense node; it should be connected to a splitter in the weiTable
* _addRelativeExpense_\(\) – create relative expense node; it should be connected to a splitter in the weiTable
* _addTopdownSplitter_\(\) – create topdown splitter node; it should be connected to a splitter in the weiTable
* _addUnsortedSplitter_\(\) – create unsorted splitter node; it should be connected to a splitter in the weiTable
* _node\_addChild_\(\) – connect a node to a splitter or 0-node
* _node\_open_\(\) – make node acceptable for transfer amounts. By default, all nodes are open
* _node\_close_\(\) – make node non-acceptable for transfer amounts. If node is closed, needed amount is 0, and processFunds will revert.
* node\_flushTo\(\) – flush funds, that associated with node, to the specified address.

{% hint style="info" %}
**WeiTable** is distinguished from the **Destination** by the fact that it have _node\_flushTo\(\)_ instead of _flush_\(\)/_flushTo_\(\).
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

// TODO - send money

// TODO - check balances

// TODO - withdraw salary

```
{% endcode-tabs-item %}
{% endcode-tabs %}


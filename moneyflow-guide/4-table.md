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
* _addSplitter_\(\) – create splitter node; it should be connected to a splitter in the weiTable
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
WeiTable weiTable = new WeiTable();
weiTable.addAbsoluteExpense();
uint salaries = weiTable.getLastNodeId();

weiTable.addAbsoluteExpense(10*eth, 10*eth, false, false, 0);
uint employee1 = weiTable.getLastNodeId();

weiTable.addAbsoluteExpense(30*eth, 30*eth, false, false, 0);
uint employee2 = weiTable.getLastNodeId();

weiTable.addAbsoluteExpense(50*eth, 50*eth, false, false, 0);
uint employee3 = weiTable.getLastNodeId();

weiTable.addChildAt(salaries, employee1);
weiTable.addChildAt(salaries, employee2);
weiTable.addChildAt(salaries, employee3);

// check needs
weiTable.getTotalWeiNeeded(100*eth); // 90*eth
weiTable.getMinWeiNeeded(100*eth); // 90*eth

// send money
weiTable.processFunds.value(90*eth)(90*eth);

// check balances
weiTable.balanceAt(employee1); // 10*eth
weiTable.balanceAt(employee2); // 30*eth
weiTable.balanceAt(employee3); // 50*eth

// withdraw salary
weiTable.flushToAt(employee1, employee1_address);
weiTable.flushToAt(employee2, employee2_address);
weiTable.flushToAt(employee3, employee3_address);
```
{% endcode-tabs-item %}
{% endcode-tabs %}


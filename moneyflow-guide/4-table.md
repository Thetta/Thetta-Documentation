---
description: (this section is still under construction)
---

# 5 - Table

**MonefrowTable** is a WeiReceiver,  that implements the composition of Destination and Splitter elements within a single contract. Receivers in it are represented as data structures. Table is convenient to use when you deal with a lot of elements, it dramatically reduces gas usage \(about 1:10 for data structures to separate contract\).

MoneflowTable incudes a single entry point –an element with id equals to 0. So, if you request **getTotalWeiNeeded\(\)**, it would be equal to **getTotalWeiNeededForElement\(0\)**, similar for other functions.

Besides WeiReceiver functions, table includes WeiReceiver-like functions for each element, such as **getTotalWeiNeededForElement\(\)**, **getElementBalance\(\)**, **getMinWeiNeededForElement\(\)**, **isElementNeedsMoney\(\)**. 

MoneflowTable also includes structural functions, such as **addAbsoluteExpense\(\)**, **addRelativeExpense\(\)**, **addTopdownSplitter\(\)**, **addUnsortedSplitter\(\)**, **addChild\(\)**. 

As for Splitter, you can open/close MoneyflowTable elements with **openElement\(\)** and **closeElement\(\)**.

MoneyflowTable is distinguished from Destination by the fact that it have **withdrawFundsFromElement\(\)** instead of flush\(\)/flushTo\(\).

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


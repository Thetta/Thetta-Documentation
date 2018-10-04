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




---
description: (this section is still under construction)
---

# 4 - Splitters

Splitter takes is an entity that receives ETHer and splits it into multiple destinations. There are two types of splitter: top-down, which send money to the outputs consistently, and unsorted, which send money in parallel. The difference between them appears when we deal with a relative expenses.

Splitters do not need anything, but if you request them how much they need, they will request their children, which request their children, etc. Eventually, we will have a list of needs of destinations \(because only destination can be in the end of each brach\). Splitter will summarize it and get you an answer. So, only destinations have a needed amount.

Splitter can be **open\(\)** or **close\(\)**. So, in opened state splitter works as it should, but in closed state it ignore children need and do not accept money.

You can **addChild\(\)** to splitter, **getChild\(\)** info or **getChildrenCount\(\)**.

{% hint style="info" %}
You SHOULD NOT send more ETH than needed to the splitter! It will throw exception.
{% endhint %}



## 1. **Top-down splitter**

In a top-down splitter money are flowing from the top to bottom. That is why the order is very important, but if you have only absolute expenses, connected to splitter, there is no difference.   
****

![](https://lh3.googleusercontent.com/hQoFzWjyGofSjlBVOBXE6rI6-ak8yZEVJ9JFGyU9oIVPDUl8XENlD3qzjCmG4l0Pu-UJisEiPoBvbxgk2d2EiblKbVZrEgOJFNUWwiD5c0_kO4b-k8KIWiGn024eqt7TJZFKx3qn)

{% code-tabs %}
{% code-tabs-item title="WeiTopDownSplitter example.sol" %}
```javascript
WeiTopDownSplitter allOutpults = new WeiTopDownSplitter('AllOutpults');
WeiUnsortedSplitter spends = new WeiUnsortedSplitter('Spends');
WeiUnsortedSplitter salaries = new WeiUnsortedSplitter('Salaries');
WeiAbsoluteExpense employee1 = new WeiAbsoluteExpense(10*eth);
WeiAbsoluteExpense employee2 = new WeiAbsoluteExpense(15*eth);
WeiAbsoluteExpense employee3 = new WeiAbsoluteExpense(8*eth);

// CONNECTIONS
allOutpults.addChild(spends);
spends.addChild(salaries);
salaries.addChild(employee1);
salaries.addChild(employee2);
salaries.addChild(employee3);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## **2. Unsorted splitter**

In unsorted splitter there is no difference how children are ordered, i.e. you can swap any elements and the result will still stay the same.  
****![](https://lh5.googleusercontent.com/QeenERRhJwgH-zDVtHUZiOLhL0R9qa4jd4xtu8USx9LmGI7-O0w86rxPaX2Igphnm0VbX1FsKhtkBzud1odoKqgD4pGb8nDO2bEfUj-Kh1EpgtsGVe7xuKa-6CDeuMzn6ryGyx5u)

{% code-tabs %}
{% code-tabs-item title="WeiUnsortedSplitter example 1.sol" %}
```javascript
WeiUnsortedSplitter salaries = new WeiUnsortedSplitter('Salaries');
WeiAbsoluteExpense employee1 = new WeiAbsoluteExpense(1*eth);
WeiAbsoluteExpense employee2 = new WeiAbsoluteExpense(2*eth);
WeiAbsoluteExpense employee3 = new WeiAbsoluteExpense(3*eth);
​
salaries.addChild(employee1);
salaries.addChild(employee2);
salaries.addChild(employee3);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="WeiUnsortedSplitter example 2.sol" %}
```javascript
WeiUnsortedSplitter rest = new WeiUnsortedSplitter('Rest');
WeiRelativeExpenseWithPeriod reserveFund = new WeiRelativeExpenseWithPeriod(750000, 0, false);
WeiRelativeExpenseWithPeriod dividendsFund = new WeiRelativeExpenseWithPeriod(250000, 0, false);

// CONNECTIONS
allOutpults.addChild(rest);
rest.addChild(reserveFund);
rest.addChild(dividendsFund);
```
{% endcode-tabs-item %}
{% endcode-tabs %}




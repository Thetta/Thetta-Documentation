# 4 - Splitters

**WeiSplitter** is a node that has single ETH input and multiple ETH outputs.   
**WeiSplitter** consumes no ETH, so it polls its children in order to return the amount of ETH needed:

```javascript
// TODO
splitter = ...
```

There are two types of splitters: 

* **WeiTopDownSplitter** - the order of children nodes matters;
* **WeiUnsortedSplitter** - the order of children nodes does not matter.

Splitter can be **open\(\)** or **close\(\)**. In opened state splitter can receive ETH, but in closed state it ignores childrens' needs and does not accept money.

You can **addChild\(\)** to splitter, **getChild\(\)** or or **getChildrenCount\(\)**.

{% hint style="info" %}
You SHOULD NOT send more ETH than needed to the splitter! It will throw an exception.
{% endhint %}

## 1. **WeiTopDownSplitter**

In a top-down splitter ETH is flowing from the top to bottom. That is why the order of children nodes is very important. 

However, if splitter contains only absolute expenses, order does not matter.   
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

// TODO - ask needed amounts, etc
// TODO - send money
// TODO - show employee1, employee2, etc balances
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## **2. WeiUnsortedSplitter**

In unsorted splitter there is no difference how children are ordered, i.e. you can swap any elements and the result will still stay the same.  
****![](https://lh5.googleusercontent.com/QeenERRhJwgH-zDVtHUZiOLhL0R9qa4jd4xtu8USx9LmGI7-O0w86rxPaX2Igphnm0VbX1FsKhtkBzud1odoKqgD4pGb8nDO2bEfUj-Kh1EpgtsGVe7xuKa-6CDeuMzn6ryGyx5u)

{% code-tabs %}
{% code-tabs-item title="WeiUnsortedSplitter example 1.sol" %}
```javascript
WeiUnsortedSplitter salaries = new WeiUnsortedSplitter('Salaries');
WeiAbsoluteExpense employee1 = new WeiAbsoluteExpense(1*eth);
WeiAbsoluteExpense employee2 = new WeiAbsoluteExpense(2*eth);

salaries.addChild(employee1);
salaries.addChild(employee2);

salaries.getMinWeiNeeded(); // 3 eth
salaries.getTotalWeiNeeded(); // 3 eth
salaries.isNeedsMoney(); // true

salaries.processFunds.value(3*eth)(3*eth);

salaries.getMinWeiNeeded(); // 0
salaries.getTotalWeiNeeded(); // 0
salaries.isNeedsMoney(); // false

address(employee1).balance() // 1 eth
address(employee2).balance() // 2 eth
```
{% endcode-tabs-item %}
{% endcode-tabs %}




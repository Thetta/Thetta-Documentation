# 4 - Splitters

**WeiSplitter** is a node that has single ETH input and multiple ETH outputs.   
**WeiSplitter** consumes no ETH, so it polls its children in order to return the amount of ETH needed:

```javascript
WeiSplitter splitter = new WeiSplitter();
```

Splitter can be _open\(\)_ or _close\(\)_. In opened state splitter can receive ETH, but in closed state it ignores childrens' needs and does not accept money.

You can _addChild\(\)_ to splitter, _getChild\(\)_ or or _getChildrenCount\(\)_.

{% hint style="info" %}
You SHOULD NOT send more ETH than needed to the splitter! It will throw an exception.
{% endhint %}

There are 3 possible types of a splitter. 
* _Type.Splitter_: initial one. If no children, or if all children are splitters. Can accept absolute/relative, after that splitter will change it's own type to _Type.Absolute/Type.Relative_ respectievly.
* _Type.Absolute_: if at least one child is absolute expense. Can't accept relative expense – only absolute expense or splitter.
* _Type.Relative_: if at least one child is relative expense. Can't accept absolute expense – only relative expense or splitter.

In a splitter ETH is flowing from the first child to the last one. That is why the order of children nodes is very important. 

There are some special cases for a splitter.

### 1. Relative expenses follow each other.

```javascript
WeiSplitter splitterRelative = new WeiSplitter();
WeiRelativeExpense expense1 = new WeiRelativeExpense(250000);
WeiRelativeExpense expense2 = new WeiRelativeExpense(250000);

WeiSplitter splitterExpense = new WeiSplitter();
WeiAbsoluteExpense expense3 = new WeiAbsoluteExpense(8*eth);

splitterExpense.addChild(expense3);

splitterRelative.addChild(expense1);
splitterRelative.addChild(expense2);
splitterRelative.addChild(splitterExpense);

splitterRelative.getTotalWeiNeeded(1*eth); // 16*eth;
splitterRelative.getTotalWeiNeeded(16*eth); // 16*eth;
splitterRelative.getTotalWeiNeeded(100*eth); // 16*eth;
```

In this case the percentages are taken from the initial amount received at the entrance.
25% of 16*eth (4*eth) goes to expense1,  25% of 16*eth (4*eth) goes to expense2, 8*eth goes to expense3. 

You can not rearrange expenses in this example, bacause relative expense will be at the end, and while it takes only 25% of the flow, some part of the flow will remain on the splitter balance => revert. We can add additional relative expense to the end, that takes 100% of the flow.

With connections from the previous example, it will be like this:
```javascript
WeiSplitter splitterRelative = new WeiSplitter();
WeiRelativeExpense expense1 = new WeiRelativeExpense(250000);
WeiRelativeExpense expense2 = new WeiRelativeExpense(250000);
WeiRelativeExpense expense4 = new WeiRelativeExpense(1000000);

WeiSplitter splitterExpense = new WeiSplitter();
WeiAbsoluteExpense expense3 = new WeiAbsoluteExpense(8*eth);

splitterExpense.addChild(expense3);

splitterRelative.addChild(expense1);
splitterRelative.addChild(expense2);
splitterRelative.addChild(splitterExpense);
splitterRelative.addChild(expense4);

splitterRelative.getTotalWeiNeeded(1*eth); // 16*eth;
splitterRelative.getTotalWeiNeeded(16*eth); // 16*eth;
splitterRelative.getTotalWeiNeeded(100*eth); // 100*eth;
```
Lets swap splitterExpense and expense2:

```javascript
WeiSplitter splitterRelative = new WeiSplitter();
WeiRelativeExpense expense1 = new WeiRelativeExpense(250000);
WeiRelativeExpense expense2 = new WeiRelativeExpense(250000);
WeiRelativeExpense expense4 = new WeiRelativeExpense(1000000);

WeiSplitter splitterExpense = new WeiSplitter();
WeiAbsoluteExpense expense3 = new WeiAbsoluteExpense(8*eth);

splitterExpense.addChild(expense3);

splitterRelative.addChild(expense1);
splitterRelative.addChild(splitterExpense);
splitterRelative.addChild(expense2);
splitterRelative.addChild(expense4);

splitterRelative.getTotalWeiNeeded(1*eth); // (32/3)*eth;
splitterRelative.getTotalWeiNeeded((32/3)*eth); // (32/3)*eth;
splitterRelative.getTotalWeiNeeded(16*eth); // 16*eth;
splitterRelative.getTotalWeiNeeded(100*eth); // 100*eth;
```
In this case 25% of 16*eth (4*eth) goes to expense1, 8*eth goes to expense3, 25% of (16 - 4 - 8 = 4)  4*eth*25% = 1*eth goes to expense2, 3*eth goes to expense4. 

![](https://lh3.googleusercontent.com/hQoFzWjyGofSjlBVOBXE6rI6-ak8yZEVJ9JFGyU9oIVPDUl8XENlD3qzjCmG4l0Pu-UJisEiPoBvbxgk2d2EiblKbVZrEgOJFNUWwiD5c0_kO4b-k8KIWiGn024eqt7TJZFKx3qn)

### 2. Absolute expenses with minAmount > 0.

```javascript
WeiSplitter splitter = new WeiSplitter();
WeiAbsoluteExpense expense1 = new WeiAbsoluteExpense(500*eth, 1000*eth);
WeiAbsoluteExpense expense2 = new WeiAbsoluteExpense(200*eth, 800*eth);
WeiAbsoluteExpense expense3 = new WeiAbsoluteExpense(500*eth, 1500*eth);

await splitter.addChild(expense1);
await splitter.addChild(expense2);
await splitter.addChild(expense3);

splitter.getTotalWeiNeeded(100*eth) //0*eth
splitter.getTotalWeiNeeded(200*eth) //200*eth: expense2 get 200
splitter.getTotalWeiNeeded(300*eth) //200*eth: expense2 get 200
splitter.getTotalWeiNeeded(500*eth) //500*eth: expense1 get 500
splitter.getTotalWeiNeeded(600*eth) //500*eth: expense1 get 500
splitter.getTotalWeiNeeded(700*eth) //700*eth: expense1 get 500 expense2 get 200
splitter.getTotalWeiNeeded(800*eth) //700*eth: expense1 get 500 expense2 get 200
splitter.getTotalWeiNeeded(900*eth) //900*eth: expense1 get 500 expense2 get 400
splitter.getTotalWeiNeeded(1000*eth) //1000*eth: expense1 get 1000
```
In this case, if flow amount is not enough for a minimal minAmount of expenses, _getTotalWeiNeeded()_ will return 0.
If flow amount is enough for a minimal minAmount of expenses, _getTotalWeiNeeded()_ will return that minimal amount, even if expense with this minAmount is not a first child in a splitter.
If flow amount is enough for a minAmount of a first child, not enough for a 2\*minAmount of a first child, but enough for a minAmount of a first child plus n\*minAmount of another child, then first child will get minAmount and another child will get n\*minAmount.

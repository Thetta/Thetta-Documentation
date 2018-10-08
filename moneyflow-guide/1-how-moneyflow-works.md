# 1 - How moneyflow works

## What Moneyflow is?

Moneyflow – is a financial subsystem that can redistribute ETH according to the set of rules which is called "a scheme". This subsystem can be used by any smart contract or a DAO.

Moneyflow can be used:

* to receive money from third-party sources \(like customers and investors\)
* to pay salaries, bonuses and regular expenses \(like office rent\)
* to send dividends to token holders
* to collect and process funds \(roadmap, milestones, etc\)
* ...

### Scheme

Following diagram represents a typical moneyflow scheme of some organization:

![](https://lh4.googleusercontent.com/MnPsHXge9Q5PzDhg6rg0YHrgMsFIsLO5ynmuI2g4WYTholpQaS5riPgzvLbqic8Ymg_Q_tNE3mA0gV_Dwd-Pr0X_hBj7pdSOpsc0zV25toUovNCn6qBgYEopY5D1PPS7kO2wTOVf)

You can think of a moneyflow scheme as a tree, where each node is an instance of **WeiReceiver** smart contract.

An account or a smart contract that is a source of ETH is called "a source". A source is sending \(pushing\) ETH to the root node and is not a part of the scheme.

Tree leaves are called "destinations". Destination **is** where ETH ultimately stored, that's why it has no children.

ETH is flowing from the source to destinations and is processed by internal elements that are called "splitters". A splitter never stores ETH, instead it redistributes ETH to its outputs.

{% hint style="info" %}
Usually each moneyflow element is a separate smart contract, but you can use **MoneyflowTable** which can implement any moneyflow scheme, but in a single contract. It effectively reduces gas consumption.
{% endhint %}

### Absolute vs Relative elements

Expenses can be absolute or relative. Relative expense node will consume a fixed part of the total money flow, for example – 2%. Absolute expense node will consume a strictly fixed amount, for example – 5 ETH.

### How to group elements?

1. Usually money flows from the top to the bottom.
2. You can group different elements any way you like. However, it is highly recommended to group items logically. For example: ![](https://lh4.googleusercontent.com/hD_9pIqErOeNxawaK-K4EOyxh_8y38aMAkJE6CK9K2u9mbzyHLwigt8RVMKBzwCTMjKd2UaLk0Fctqe5N52Vl4CNwZ_Or1wtgcBTgtu2oquLWnYluCNUBck-02OkwTzgAwoGF2Ic) But But not like this: ![](https://lh5.googleusercontent.com/BBSgdtZNhidI84YZB1BIdfiFJ8RJrllfHL7mnUJclt_vUrLbX_a8DI6KjK3YuY_VyvM05D149gcBStF0dZecGlAwjTw2xHDeEc3imndumG8oinC9qCeqOHchJrpKX7NS0yaUINQo)

Although this is absolutely legitimate, it is highly recommended to avoid latter approach.

### Code example

```javascript
WeiTopDownSplitter root = new WeiTopDownSplitter('Root');
WeiUnsortedSplitter spends = new WeiUnsortedSplitter('Spends');
WeiUnsortedSplitter salaries = new WeiUnsortedSplitter('Salaries');
WeiAbsoluteExpense employee1 = new WeiAbsoluteExpense(1*eth);
WeiAbsoluteExpense employee2 = new WeiAbsoluteExpense(2*eth);
WeiUnsortedSplitter other = new WeiUnsortedSplitter('Other');
WeiAbsoluteExpense office = new WeiAbsoluteExpense(1*eth);
WeiUnsortedSplitter rest = new WeiUnsortedSplitter('Rest');

bool isPeriodic = false;
bool isAccumulateDebt = false;
uint periodInHours = 0;
uint neededWeiReserve = 100*eth;
uint neededWeiDividends = 0;

WeiFund reserveFund = new WeiFund(neededWeiReserve, isPeriodic, isAccumulateDebt, periodInHours);
WeiFund dividendsFund = new WeiFund(neededWeiDividends, isPeriodic, isAccumulateDebt, periodInHours); // Infinite fund

// Connect nodes
other.addChild(office);

salaries.addChild(employee1);
salaries.addChild(employee2);

spends.addChild(other);
spends.addChild(salaries);

rest.addChild(reserveFund);
rest.addChild(dividendsFund);

root.addChild(spends);
root.addChild(rest);
```


# 1 - How moneyflow works

## What is a Moneyflow?

Moneyflow – is a financial subsystem that can redistribute ETH according to the set of rules that is called  "a scheme". This subsystem can be utilized by any smart contract or a DAO.  
  
Moneyflow can be used:

* to receive money from third-party sources \(like customers and investors\)
* to pay salaries, bonuses and regular expenses \(like payment for office or internet\)
* to send dividends to token holders
* to collect and process funds \(roadmap, milestones, etc\)
* ...

### Scheme

Following diagram represents a typical moneyflow scheme of some organization:

![](https://lh4.googleusercontent.com/MnPsHXge9Q5PzDhg6rg0YHrgMsFIsLO5ynmuI2g4WYTholpQaS5riPgzvLbqic8Ymg_Q_tNE3mA0gV_Dwd-Pr0X_hBj7pdSOpsc0zV25toUovNCn6qBgYEopY5D1PPS7kO2wTOVf)

You can think of moneyflow scheme as a tree, where each node is an instance of **WeiReceiver** smart contract. 

An account or a smart contract that is a source of money is called "a source". Source is sending \(pushing\) ETH to the root node and is not a part of the scheme.  
  
The tree leaves are called "destinations". Destination ****is ****where ETH finally arrives, that's why it has no children.   
  
ETH are flowing from the source to destinations and are processed by internal elements that are called "splitters". Splitter never stores ETH, instead it redistributes ETH to its outputs. 

{% hint style="info" %}
Usually, each moneyflow element is a separate smart contract, but you can use **MoneyflowTable** which can implement any moneyflow scheme, but in single contract. It effectively reduces gas consumption.
{% endhint %}

### Absolute vs Relative elements

TODO

### How to group elements?

1. Usually money flow from the Top to the Down.
2. You can group different elements in any way you like. However it is highly recommended to logically group items together.  Like this: ![](https://lh4.googleusercontent.com/hD_9pIqErOeNxawaK-K4EOyxh_8y38aMAkJE6CK9K2u9mbzyHLwigt8RVMKBzwCTMjKd2UaLk0Fctqe5N52Vl4CNwZ_Or1wtgcBTgtu2oquLWnYluCNUBck-02OkwTzgAwoGF2Ic) But not like this: ![](https://lh5.googleusercontent.com/BBSgdtZNhidI84YZB1BIdfiFJ8RJrllfHL7mnUJclt_vUrLbX_a8DI6KjK3YuY_VyvM05D149gcBStF0dZecGlAwjTw2xHDeEc3imndumG8oinC9qCeqOHchJrpKX7NS0yaUINQo)

Also, this is absolutely legitimate, it is highly recommended to avoid later approach.


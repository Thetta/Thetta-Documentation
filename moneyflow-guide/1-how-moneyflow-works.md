---
description: (this section is still under construction)
---

# 1 - How moneyflow works

## What is a Moneyflow?

 **Moneyflow** – system for redistributing money from sources to destinations. Moneyflow can be represented as a tree, in which one root node - source, internal nodes - always splitters, leaves - always destinations.

This subsystem can be utilized by any DAO. But it is completely optional.   
Moneyflow can be used as the main financial flow of a DAO, which will include receiving money from third-party sources \(customers, investors, etc\), redistribution within the organization and output to the outside \(paying salaries to employees, paying regular expenses, paying other organizations, saving money in funds\) .

In addition to creating a budget system for a DAO, other uses can be found for moneyflow. For example, to create a roadmap, or some other thing.

There are 2 types of MoneyFlow elements that you can use – splitter and destination. Source is just an ordinary Ethereum account or a smart contract that is the source of the money.

**Splitter** takes money and redirect it to outputs. There are two types of splitter: **top-down**, which send money to the outputs consistently, and unsorted, which send money in parallel. The difference between them appears when we deal with a relative expenses.

**Destination** is ****where money finally arrive. Destination has no children, and it have many params. 

Each moneyflow element represents by a separate contract, but there is a **table**, which is a destination element, and which can implement any moneyflow scheme, but in one contract. It effectively reduce gas consumption.

Following diagram represent a moneyflow scheme of some DAO:

![Example of how moneyflow works](https://lh4.googleusercontent.com/MnPsHXge9Q5PzDhg6rg0YHrgMsFIsLO5ynmuI2g4WYTholpQaS5riPgzvLbqic8Ymg_Q_tNE3mA0gV_Dwd-Pr0X_hBj7pdSOpsc0zV25toUovNCn6qBgYEopY5D1PPS7kO2wTOVf)

Also, each moneyflow element have a **getMinWeiNeeded\(\)** and **getTotalWeiNeed\(\)** functions. Splitters do not need anything, but if you ask them, they will ask their children, which ask their children, etc. Splitter will summarize it and get you an answer. So, only destinations have a needed amount.

### How does MoneyFlow work?

1. Usually money flow from the Top to the Down.
2. You can group different elements in any way you like. However it is highly recommended to logically group Absolute and Relative items together.  Like this: ![](https://lh4.googleusercontent.com/hD_9pIqErOeNxawaK-K4EOyxh_8y38aMAkJE6CK9K2u9mbzyHLwigt8RVMKBzwCTMjKd2UaLk0Fctqe5N52Vl4CNwZ_Or1wtgcBTgtu2oquLWnYluCNUBck-02OkwTzgAwoGF2Ic) But not like this: ![](https://lh5.googleusercontent.com/BBSgdtZNhidI84YZB1BIdfiFJ8RJrllfHL7mnUJclt_vUrLbX_a8DI6KjK3YuY_VyvM05D149gcBStF0dZecGlAwjTw2xHDeEc3imndumG8oinC9qCeqOHchJrpKX7NS0yaUINQo)

Also, this is absolutely legitimate, it is highly recommended to avoid later approach.


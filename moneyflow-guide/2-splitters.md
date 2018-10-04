---
description: (this section is still under construction)
---

# 3 - Splitters

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

TODO - add code \(same as on pic\)

## **2. Unsorted splitter**

In unsorted splitter there is no difference how children are ordered, i.e. you can swap any elements and the result will still stay the same.  
****![](https://lh5.googleusercontent.com/QeenERRhJwgH-zDVtHUZiOLhL0R9qa4jd4xtu8USx9LmGI7-O0w86rxPaX2Igphnm0VbX1FsKhtkBzud1odoKqgD4pGb8nDO2bEfUj-Kh1EpgtsGVe7xuKa-6CDeuMzn6ryGyx5u)



TODO - add code \(same as on pic\)


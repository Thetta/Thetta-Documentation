# 2 - Implementing business logic

Imagine a smart contract that orders cakes from an external bakery:​

```javascript
pragma solidity ^0.4.24;
​​
contract Bakery {
    uint public cakesOrdered = 0;
    mapping (address=>bool) public isCakeProducedForAddress;
​
    function buySomeCake(address _cakeEater) public {
        cakesOrdered = cakesOrdered + 1; // increase cakesOrdered var
        isCakeProducedForAddress[_cakeEater] = true;
    }
}

contract CakeBuyer {
    function buySomeCakeInternal(Bakery _bakery) internal { 
        // just an example of some business logic
        // (external call)
        _bakery.buySomeCake(msg.sender);
    }
}
```

What if you want **CakeBuyer** to be controlled not only exclusively by you, but also by some of your friends? So that is where Thetta comes in.

Let's start by converting **CakeBuyer** to **CakeOrderingOrganization**:

```javascript
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

// definitions of Bakery and CakeBuyer are removed for clarity (see above)
contract Bakery { ... }
contract CakeBuyer { ... }

contract CakeOrderingOrganizaion is CakeBuyer, DaoClient {
    bytes32 public constant BUY_SOME_CAKE = keccak256("buySomeCake");
    Bakery public bakery;

    constructor(Bakery _bakery, DaoBase _daoBase) public DaoClient(_daoBase){
        bakery = _bakery;
    }

    function buySomeCake() public isCanDo(BUY_SOME_CAKE) {
        buySomeCakeInternal(bakery);
    }
}
```

{% hint style="info" %}
Please notice that **buySomeCakeInternal** method is marked with an _internal_ modifier and can be called only by **buySomeCake.**
{% endhint %}

The contract layout will now look as follows:

![](../.gitbook/assets/graph%20%281%29.png)

OK, what has been done?

1. All business logic is left in the **CakeByer** contract;
2. The **CakeOrderingOrganization** contract is implemented and will be controlled by you and your friends;
3. A new action - **buySomeCake** - has been added and connected with the **BUY\_SOME\_CAKE** permission.

Permissions will be granted to all actors \(i.e., users or other contracts\) in the next chapter.


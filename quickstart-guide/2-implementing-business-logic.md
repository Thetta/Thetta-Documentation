# 2 - Implementing business logic

Imagine a smart contract that orders cakes from the external bakery:​

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

contract CakeByer {
	function buySomeCakeInternal(Bakery _bakery) internal { 
		// just an example of some business logic
		// (external call)
		_bakery.buySomeCake(msg.sender);
	}
}
```

What if you want **CakeByer** to be controlled not by anyone, even not by yourself \(the owner\), but by you AND some of your friends? So that is where Thetta comes in. 

Let's start by converting **CakeByer** to **CakeOrderingOrganization**: 

```javascript
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

// definitions of Bakery and CakeByer are removed for clarity (see above)
contract Bakery { ... }
contract CakeByer { ... }

contract CakeOrderingOrganizaion is CakeByer, DaoClient {
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

Ok, what have we done?

1. We left all business logic in the **CakeByer** contract;
2. We implemented the **CakeOrderingOrganization** contract that will be controlled by you and your friends;
3. We added new action - **buySomeCake** - and connected it with the **BUY\_SOME\_CAKE** permission.

We are going to grant permissions to all actors \(i.e., users or other contracts\) in the next chapter.


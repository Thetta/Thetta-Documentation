# 2 - Implementing business logic

Imagine a smart contract that orders cakes from the external bakery:​

```text
//-------------------------------- ./Bakery.sol
pragma solidity ^0.4.24;
​
​
contract Bakery {
	event cakeProduced();
	uint public cakesOrdered = 0;
	mapping (address=>bool) public isCakeProducedForAddress;
​
	function buySomeCake(address _cakeEater) public{
		emit cakeProduced();
		cakesOrdered = cakesOrdered + 1; // increase cakesOrdered var
		isCakeProducedForAddress[_cakeEater] = true;
	}
}

//-------------------------------- ./CakeByer.sol
pragma solidity ^0.4.24;
import "./Bakery.sol";
​
​
contract CakeByer {
	function buySomeCakeInternal(Bakery _bakery) internal { 
		_bakery.buySomeCake(msg.sender);
	}
}
```

What if you want **CakeByer** to be controlled not by anyone, even not by yourself \(the owner\), but by you AND some of your friends? So that is where Thetta comes in. 

Let's start by converting **CakeByer** to **CakeOrderingOrganization**: 

```text
//-------------------------------- ./CakeOrderingOrganizaion.sol
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

import "./CakeByer.sol";
import "./Bakery.sol";


contract CakeOrderingOrganizaion is CakeByer, DaoClient {
	bytes32 public constant BUY_SOME_CAKE = keccak256("buySomeCake");
	DaoBase public daoBase;
	Bakery public bakery;

	constructor(Bakery _bakery, DaoBase _daoBase) public DaoClient(_daoBase){
		bakery = _bakery;
		daoBase = _daoBase;
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


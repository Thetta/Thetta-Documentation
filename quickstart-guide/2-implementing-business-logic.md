# 2 - Implementing business logic

Imagine you have this business logic in your DApp or Organization:

```text
pragma solidity ^0.4.22;
import "zeppelin-solidity/contracts/ownership/Ownable.sol";

contract CakeOrderingDapp is Ownable {
   function buySomeCake() public onlyOwner { ... }
}
```

Let **buySomeCake** method transfer Ether to some online bakery and order a cake. 

{% hint style="info" %}
Currently, only owner \(initially, this is who deployed it\) of this contract can call **buySomeCake** method 
{% endhint %}

Imagine you want **CakeOrderingOrganization** to be controlled not only by yourself, but by ALL your friends. So  that is where Thetta comes in:

```text
pragma solidity ^0.4.24;

import "zeppelin-solidity/contracts/ownership/Ownable.sol";
import "zeppelin-solidity/contracts/token/ERC20/MintableToken.sol";

import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/IDaoBase.sol";

contract CakeOrderingDapp {
	uint public x = 0;

	constructor() {
	}

	function buySomeCakeInternal(IDaoBase _daoBase, address _tokenAddress) internal { 
		x = 1; // write 1 to x
		_daoBase.issueTokens(_tokenAddress, msg.sender, 100); // issue 100 ERC20 tokens for caller
	}
}

contract CakeOrderingOrganizaion is CakeOrderingDapp, DaoClient {
	address public tokenAddress;
	bytes32 public constant BUY_SOME_CAKE = keccak256("buySomeCake");

	constructor(IDaoBase _daoBase, address _tokenAddress) public DaoClient(_daoBase){
		tokenAddress = _tokenAddress;
	}

	function buySomeCake() public isCanDo(BUY_SOME_CAKE) { 
		buySomeCakeInternal(daoBase, tokenAddress);
	}
}

```

{% hint style="info" %}
Please notice that **buySomeCakeInternal** method is marked with an _internal_ modifier and can be called only by **buySomeCake.**
{% endhint %}

### The contracts diagram will now look like this:

![](../.gitbook/assets/scheme.png)

Ok, what have we done?

1. We left all busines logic in **CakeOrderingCore** contract;
2. We added **CakeOrderingOrganization** contract that will controlled by your friends!
3. We added new action **buySomeCake** and attached to it custom permission **BUY\_SOME\_CAKE;**

We are going to grant all actors \(i.e., users or other contracts\) permissions later \(see next chapter\).


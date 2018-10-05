# 5 - Wrapping up everything

Ok, last time our code looked like this:

```javascript
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

contract Bakery {
	uint public cakesOrdered = 0;
	mapping (address=>bool) public isCakeProducedForAddress;

	function buySomeCake(address _cakeEater) public{
		cakesOrdered = cakesOrdered + 1; // increase cakesOrdered var
		isCakeProducedForAddress[_cakeEater] = true;
	}
}

contract CakeByer {
	function buySomeCakeInternal(Bakery _bakery) internal { 
		_bakery.buySomeCake(msg.sender);
	}
}

contract CakeOrderingOrganizaion is CakeByer, DaoClient {
	bytes32 public constant BUY_SOME_CAKE = keccak256("buySomeCake");
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

Let's grant needed permissions:

```javascript
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoBase.sol";

// we are importing Bakery, CakeByer, CakeOrderingOrganizaion definitions
// (see above)
import "./CakeOrderingOrganizaion.sol";

contract Deployer {
	DaoBase public daoBase;
	Bakery public bakery;
	CakeOrderingOrganizaion public cakeOrderingOrganizaion;

	function deploy(DaoBase _daoBase, address _friend1, address _friend2, address _friend3) public {
		daoBase = _daoBase;
		
		bakery = new Bakery();
		cakeOrderingOrganizaion = new CakeOrderingOrganizaion(bakery, daoBase);
		
		setPermissions(_friend1, _friend2, _friend3);
	}

	function setPermissions(address _friend1, address _friend2, address _friend3) public {
		daoBase.addGroupMember("Friends", _friend1);
		daoBase.addGroupMember("Friends", _friend2);

		// This will allow any address that is a member of "Friends" group 
		// to execute "buySomeCake" method:
		daoBase.allowActionByAnyMemberOfGroup(cakeOrderingOrganizaion.BUY_SOME_CAKE(), "Friends");

		// To allow specific address to execute action without any voting:
		daoBase.allowActionByAddress(cakeOrderingOrganizaion.BUY_SOME_CAKE(), _friend3);
	}
}
```

Deployer is a helper that allows you to deploy **CakeOrderingOrganization.**

### Conclusion

You have converted the **CakeByer** smart contract to the **CakeOrderingOrganization** DAO that can be now controlled collectively by you and your friends!

The complete project \(example, tests and migrations\) is available here - [https://github.com/Thetta/Quickstart\_Guide](https://github.com/Thetta/Quickstart_Guide)


# 5 - Wrapping up everything

Ok, last time our code looked like this:

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

//-------------------------------- ./CakeByer.sol
pragma solidity ^0.4.24;
import "./Bakery.sol";


contract CakeByer {
	function buySomeCakeInternal(Bakery _bakery) internal { 
		_bakery.buySomeCake(msg.sender);
	}
}

//-------------------------------- ./Bakery.sol
pragma solidity ^0.4.24;


contract Bakery {
	event cakeProduced();
	uint public cakesOrdered = 0;
	mapping (address=>bool) public isCakeProducedForAddress;

	function buySomeCake(address _cakeEater) public{
		emit cakeProduced();
		cakesOrdered = cakesOrdered + 1; // increase cakesOrdered var
		isCakeProducedForAddress[_cakeEater] = true;
	}
}

```

Let's grant needed permissions:

```text
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoBase.sol";

import "./CakeOrderingOrganizaion.sol";
import "./CakeByer.sol";
import "./Bakery.sol";


contract Deployer {
	DaoBase public daoBase;
	Bakery public bakery;
	CakeOrderingOrganizaion public cakeOrderingOrganizaion;

	function deploy(DaoBase _daoBase, address _friend1, address _friend2, address _friend3) public{
		daoBase = _daoBase;
		bakery = new Bakery();
		cakeOrderingOrganizaion = new CakeOrderingOrganizaion(bakery, daoBase);
		setPermissions(_friend1, _friend2, _friend3);
	}

	function setPermissions(address _friend1, address _friend2, address _friend3) public {
		// Add some address (user or contract) to Friends group
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

You have deployed **CakeOrderingOrganization.**  
Now you will be able to call **buySomeCake** method.


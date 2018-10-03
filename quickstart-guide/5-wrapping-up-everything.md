# 5 - Wrapping up everything

Ok, last time our code looked like this:

```text
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

// some external smart contract that does something
contract Bakery {
	event cakeProduced();
	
	uint public cakesOrdered = 0;
	mapping (address=>bool) public isCakeProducedForAddress;
​
	function buySomeCake(address _cakeEater) public {
		cakesOrdered = cakesOrdered + 1;
		isCakeProducedForAddress[_cakeEater] = true;
		
		emit cakeProduced();
	}
}
​
contract CakeByer {
    function buySomeCakeInternal(Bakery _bakery) internal { 
    	// some business logic here
		_bakery.buySomeCake(msg.sender);
	}
}

contract CakeOrderingOrganizaion is CakeByer, DaoClient {
    Bakery bakery;
	bytes32 public constant BUY_SOME_CAKE = keccak256("buySomeCake");
​
	constructor(Bakery _bakery, DaoBase _daoBase) public DaoClient(_daoBase){
		bakery = _bakery;
	}
​
	function buySomeCake() public isCanDo(BUY_SOME_CAKE) { 
		buySomeCakeInternal(bakery);
	}
}
```

Let's grant needed permissions:

```text
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoStorage.sol";
import "@thetta/core/contracts/DaoBase.sol";
import "@thetta/core/contracts/tokens/StdDaoToken.sol";

// 1 - Create all needed Thetta/core contracts
StdDaoToken token = new StdDaoToken("CookCoin","COOK", 18, true, true, 0);
address tokens;
tokens.push(token);

DaoStorage daoStorage = new DaoStorage(tokens);
DaoBase daoBase = new DaoBase(daoStorage);
token.transferOwnership(daoBase);

// TODO: 
Bakery bakery = ...;

// 2 - Create our org contract (a client of the DaoBase contract)
CakeOrderingOrganizaion org = new CakeOrderingOrganizaion(bakery, daoBase);

// 3 - Set all permissions
daoBase.addGroupMember("Friends", _friend1);
daoBase.addGroupMember("Friends", _friend2);
		 
// This will allow any address that is a member of "Friends" group 
// to execute buySomeCake action:
daoBase.allowActionByAnyMemberOfGroup(BUY_SOME_CAKE, "Friends");
		        
// This will allow specific address to execute action directly
// (can be another smart contract)
daoBase.allowActionByAddress(BUY_SOME_CAKE, msg.sender);

```

You have deployed **CakeOrderingOrganization.**  
Now you will be able to call **buySomeCake** method.


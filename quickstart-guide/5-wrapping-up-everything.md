# 5 - Wrapping up everything

Ok, last time our code looked like this:

```text
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

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

contract CakeOrderingDapp {
	function buySomeCakeInternal(Bakery _bakery) internal { 
		_bakery.buySomeCake(msg.sender);
	}
}

contract CakeOrderingOrganizaion is CakeOrderingDapp, DaoClient {
	bytes32 public constant BUY_SOME_CAKE = keccak256("buySomeCake");
	address public tokenAddress;
	DaoBase public daoBase;
	Bakery public bakery;

	constructor(Bakery _bakery, DaoBase _daoBase, address _tokenAddress) public DaoClient(_daoBase){
		bakery = _bakery;
		tokenAddress = _tokenAddress;
		daoBase = _daoBase;
	}

	function buySomeCake() public isCanDo(BUY_SOME_CAKE) {
		daoBase.issueTokens(tokenAddress, msg.sender, 100);
		buySomeCakeInternal(bakery);
	}

	function setPermissions(address _boss, address _user) public {
		// Add some address (user or contract) to Employee group
		daoBase.addGroupMember("Managers", _boss); 
		daoBase.allowActionByAddress(daoBase.ISSUE_TOKENS(), "Managers");

		// This will allow any address that is a member of "Managers" group 
		// to execute "issueTokens" method:
		daoBase.allowActionByAnyMemberOfGroup(BUY_SOME_CAKE, "Managers");
		        
		// To allow specific address to execute action without any voting:
		daoBase.allowActionByAddress(BUY_SOME_CAKE, _user);
	}
}
```

Let's grant needed permissions:

```text
pragma solidity ^0.4.22;
import "@thetta/core/contracts/DaoStorage.sol";
import "@thetta/core/contracts/DaoBase.sol";
import "@thetta/core/contracts/tokens/StdDaoToken.sol";

// 1 - Create all needed Thetta/core contracts
StdDaoToken token = new StdDaoToken("CookCoin","COOK", 18, true, true, 0);
StdDaoToken repToken = new StdDaoToken("CookRepCoin","CREP", 18, true, true, 0);
address tokens;
tokens.push(token);
tokens.push(repToken);

DaoStorage daoStorage = new DaoStorage(token);
DaoBase daoBase = new DaoBase(daoStorage);
token.transferOwnership(daoBase);
repToken.transferOwnership(daoBase);

// 2 - Create our org contract (a client of DaoBase contract)
CakeOrderingOrganizaion org = new CakeOrderingOrganizaion(daoBase);

// 3 - Set all permissions
daoBase.addActionByAddress(daoBase.ISSUE_TOKENS(), org);
daoBase.addActionByAddress(daoBase.ADD_PROPOSAL(), org);
daoBase.allowActionByVoting(org.BUY_SOME_CAKE(), token.address);

// 4 - (this is optional) Add me to the SuperUsers group
daoBase.addGroupMember("SuperUsers", msg.sender);
daoBase.allowActionByAnyMemberOfGroup(org.BUY_SOME_CAKE(), ”SuperUsers”);

```

You have deployed **CakeOrderingOrganization.**  
Now you will be able to call **buySomeCake** method.


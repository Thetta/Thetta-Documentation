# 5 - Wrapping up everything

Ok, last time our code looked like this:

```text
pragma solidity ^0.4.22;
import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

contract CakeOrderingCore is DaoClient {
   function buySomeCakeInternal() internal { 
      // 1 - some cake ordering business logic goes here
      ...
      
      // 2 - imagine we need to issue some tokens to the baker
      // 'dao' variable is provided by DaoClient 
      address baker = ...;  
      dao.issueTokens(repToken.address, baker, 100);
   }
}

contract CakeOrderingOrganizaion is CakeOrderingCore, DaoClient {
   bytes32 public constant BUY_SOME_CAKE = keccak256("buySomeCake");
   
   constructor(DaoBase _dao) DaoClient(_dao) {
   
   }
   
   function buySomeCake() public isCanDo(BUY_SOME_CAKE){
      buySomeCakeInternal();
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


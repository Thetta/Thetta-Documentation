# 3 - Actions + permission

There are many built-in actions provided by **Thetta core**:

1. addNewProposal
2. manageGroups
3. issueTokens
4. burnTokens
5. upgradeDaoContract
6. addNewTask
7. startTask
8. startBounty
9. ...

For example, the DaoBase smart contract has **issueTokens** action that is defined like this:

```javascript
// this is the main Thetta DAO Framework contract
// defined in "thetta/core/contracts/DaoBase.sol" file 
contract DaoBase {
  // bytes32 public constant ISSUE_TOKENS = keccak256("issueTokens");
  bytes32 public constant ISSUE_TOKENS = 0xe003bf3bc29ae37598e0a6b52d6c5d94b0a53e4e52ae40c01a29cdd0e7816b71;

  function issueTokens(address _tokenAddress, address _to, uint _amount) 
     public isCanDo(ISSUE_TOKENS) 
  { 
     ...   
  }   
}
```

In this case **issueTokens** is called an action, and **ISSUE\_TOKENS** - a permission.

### Setting permissions

Thetta permissions work like Access Control List (ACL) in your operating system.

To be able to call **buySomeCakes** action, one needs **BUY\_SOME\_CAKE** permission to be granted to their account. There are different options how one can set permissions:

```javascript
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

```


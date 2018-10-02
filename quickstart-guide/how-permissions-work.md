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

For example, DaoBase has **issueTokens** action that is defined like this:

```text
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

In this case we call **issueTokens** an action, and **ISSUE\_TOKENS** a permission.

### Setting permissions

Thetta Permissions work like AccessControlLists in your OperatingSystems.

In order to be able to call **issueTokens** action, one needs **ISSUE\_TOKENS** permission to be granted to his account. There are different options how you can set the permissions:

```
	function setPermissions(address _boss, address _user) public {
		// Add some address (user or contract) to Employee group
		daoBase.addGroupMember("Managers", _boss); 
		daoBase.allowActionByAddress(daoBase.ISSUE_TOKENS(), _boss);

		// This will allow any address that is a member of "Managers" group 
		// to execute "issueTokens" method:
		daoBase.allowActionByAnyMemberOfGroup(BUY_SOME_CAKE, "Managers");
		        
		// To allow specific address to execute action without any voting:
		daoBase.allowActionByAddress(BUY_SOME_CAKE, _user);
	}
```




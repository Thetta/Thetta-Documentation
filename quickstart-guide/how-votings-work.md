# 4 - How votings work

### Votings

Usually if you have a permission to call an action \(imagine, **issueTokens\)** directly, you can just call issueTokens method and it will immediately mint some tokens:

```text
// 1 - Grant myself ISSUE_TOKENS permission
// i should be the owner of daoBase OR i should have MANAGE_GROUPS permission
// to call addGroupMember and allowActionByAnyMemberOfGroup
daoBase.addGroupMember("Managers", msg.sender);
daoBase.allowActionByAnyMemberOfGroup(daoBase.ISSUE_TOKENS(), "Managers");

// 2 - Check permission 
// this should be 'true'
bool yesICan = daoBase.isCanDoAction(msg.sender, daoBase.ISSUE_TOKENS());
 
// 3 - Mint 100 tokens to employee1
// daoBase is an owner of repToken and controlls it
// to call this action you should have ISSUE_TOKENS permission granted
// to your account (current msg.sender)
daoBase.issueTokens(repToken.address, employee1, 100);
```

But sometimes, instead of directly minting tokens, you will want to start voting.

```text
// 1 - ISSUE_TOKENS can be called with voting
daoBase.allowActionByVoting(dao.ISSUE_TOKENS(), token.address);

// 2 - Check permission 
// this should be 'false'
bool yesICan = daoBase.isCanDoAction(msg.sender, daoBase.ISSUE_TOKENS());

// 3 - Mint 100 tokens to employee1
// now this will create new proposal and start new voting automatically
daoBase.issueTokens(repToken.address, employee1, 100);
```

In this case Thetta will automatically create a new Proposal and start a new Voting. Once voting is finished with "yes", action will be called automatically:

![](https://lh6.googleusercontent.com/LDt350Tq8oWYWtUMVBqR6fS_8uA2aHd9VncFhKSVryFuhmdf5d1ivfluON2KDb_IiW1JNwEj7ORb7-jvIYA-6uiI0puC3D7vHOJ8Y1txAEjQW_5FX8lELOA-fJ_RXq18UUMGPqGU)




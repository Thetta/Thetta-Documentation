# 4 - How Votings work

### Votings

But sometimes you will want to automatically start voting if some action is not allowed directly:

```text
_dao.allowActionByVoting(dao.ISSUE_TOKENS(), token.address);
```

In this case Thetta will automatically create a new Proposal and start a new Voting. Once voting is finished with "yes", action is called:

![](https://lh6.googleusercontent.com/LDt350Tq8oWYWtUMVBqR6fS_8uA2aHd9VncFhKSVryFuhmdf5d1ivfluON2KDb_IiW1JNwEj7ORb7-jvIYA-6uiI0puC3D7vHOJ8Y1txAEjQW_5FX8lELOA-fJ_RXq18UUMGPqGU)




# Example 2 - Roadmap

For example, we have a roadmap with a following milestones:

1. release a prototype;
2. release a production ready solution;
3. make an additional stuff \(unlimited money\).

Our roadmap moneyflow will accept money directly from investors. 

Each milestone will be a fund, which will accept money until it fulfilled. 

We can implement this with a topdown splitter and 3 funds, the first two will be with a cup, and the last one – without.

```javascript
WeiTopDownSplitter roadmap = new WeiTopDownSplitter('Roadmap');

WeiFund prototype = new WeiFind(1000*1**18, false, false, 0);
WeiFund productionReady = new WeiFind(10000*1**18, false, false, 0);
WeiFund additionalStuff = new WeiFind(0, true, false, 0);

roadmap.addChild(prototype);
roadmap.addChild(productionReady);
roadmap.addChild(additionalStuff);
```

Our roadmap is ready! Now, if we want to specify our expenses for each fund, we can create moneflows for each of them and flush funds there. This logic can be implemented in a DaoClient contract, for example.


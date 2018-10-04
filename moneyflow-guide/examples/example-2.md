# Example 2 - Budget

Lets implement a budget scheme from section 1. Our entry point is ****the **WeiTopDownSplitter**, that have 3 connected **WeiUnsortedSplitters**: Spends, Bonuses and Rest. While it is a top-down splitter, relative bonuses and the rest will calculates from a `inputSum – Spends`:

```javascript
WeiTopDownSplitter allOutpults = new WeiTopDownSplitter('AllOutpults');

WeiUnsortedSplitter spends = new WeiUnsortedSplitter('Spends');
WeiUnsortedSplitter bonuses = new WeiUnsortedSplitter('Bonuses');
WeiUnsortedSplitter rest = new WeiUnsortedSplitter('Rest');

// CONNECTIONS
allOutpults.addChild(spends);
spends.addChild(salaries);
allOutpults.addChild(bonuses);
allOutpults.addChild(rest);
```

Then we will create and connect Salaries, Other and Tasks to the Spends splitter:

```javascript

WeiUnsortedSplitter salaries = new WeiUnsortedSplitter('Salaries');
WeiUnsortedSplitter other = new WeiUnsortedSplitter('Other');
WeiUnsortedSplitter tasks = new WeiUnsortedSplitter('Tasks');

// CONNECTIONS
spends.addChild(salaries);
spends.addChild(other);
spends.addChild(tasks);
```

Lets create and connect Salaries, Other and Tasks expenses to the Spends splitter:

```javascript
WeiAbsoluteExpense employee1 = new WeiAbsoluteExpense(10**17);
WeiAbsoluteExpense employee2 = new WeiAbsoluteExpense(15**17);
WeiAbsoluteExpense employee3 = new WeiAbsoluteExpense(8**17);

WeiAbsoluteExpense office = new WeiAbsoluteExpense(5**17);
WeiAbsoluteExpense internet = new WeiAbsoluteExpense(3**17);

WeiAbsoluteExpense task1 = new WeiAbsoluteExpense(5**17);
WeiAbsoluteExpense task2 = new WeiAbsoluteExpense(3**17);
WeiAbsoluteExpense task3 = new WeiAbsoluteExpense(10**17);

// CONNECTIONS
salaries.addChild(employee1);
salaries.addChild(employee2);
salaries.addChild(employee3);

other.addChild(office);
other.addChild(internet);

tasks.addChild(task1);
tasks.addChild(task2);
tasks.addChild(task3);

```

And, for the rest, bonuses and funds:

```javascript
WeiRelativeExpense bonus1 = new WeiRelativeExpense(10000);
WeiRelativeExpense bonus2 = new WeiRelativeExpense(10000);
WeiRelativeExpense bonus3 = new WeiRelativeExpense(20000);
WeiUnsortedSplitter rest = new WeiUnsortedSplitter('Rest');
WeiRelativeExpenseWithPeriod reserveFund = new WeiRelativeExpenseWithPeriod(250000, 0, false);
WeiRelativeExpenseWithPeriod dividendsFund = new WeiRelativeExpenseWithPeriod(750000, 0, false);

bonuses.addChild(bonus1);
bonuses.addChild(bonus2);
bonuses.addChild(bonus3);

rest.addChild(reserveFund);
rest.addChild(dividendsFund);

```

That is all!


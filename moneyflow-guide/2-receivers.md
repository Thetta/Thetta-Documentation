# 2 - Receivers

Receiver is an abstract class, that have a **processFunds\(\)**, **isNeedsMoney\(\)**, **getPartsPerMillion\(\)** methods. 

**WeiReceiver** will revert, if you send money directly, so use payable **processFunds\(\)** function instead. It is a universal function to transfer money between moneyflow elements.

Receiver have two inheritors – **Erc20Receiver** _\(not implemented yet\)_ and **WeiReceiver**. 

Since now moneyflow only works with ether, each moneyflow element is a WeiReceiver.

WeiReceiver that have two methods – **getMinWeiNeeded\(\)** and **getTotalWeiNeeded\(\)**. 

The basic algorithm for moneyflow is to request  **getMinWeiNeeded\(\)** or **getTotalWeiNeed\(\)** from moneyflow entry point with a specified flow amount, and then send money by  **processFunds\(\)** function with am amount you get from **getMinWeiNeeded\(\)** or **getTotalWeiNeed\(\)**.

The difference between **getMinWeiNeeded\(\)** or **getTotalWeiNeed\(\)** exist in cases, where we have relative expenses or fund. If moneyflow consist only absolute expenses and no funds, the behavior of this two functions will be the same.

WeiReceiver have 4 inheritors – **WeiTopDownSplitter**, **WeiUnsortedSplitter**, **WeiFund**, **WeiExpense**.

 The first two elements,  is also **Splitters**, and the last two is **Destinations**.




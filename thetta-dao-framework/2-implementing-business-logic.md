# 2 - Implementing business logic

Imagine you have this business logic in your DApp or Organization:

```text
pragma solidity ^0.4.22;
import "zeppelin-solidity/contracts/ownership/Ownable.sol";

contract CakeOrderingDapp is Ownable {
   function buySomeCake() public onlyOwner;
}
```

Let **buySomeCake** method transfer Ether to some online bakery and order a cake. 

{% hint style="info" %}
Only owner \(initially, this is who deployed it\) of this contract can call **buySomeCake** method 
{% endhint %}

Imagine you want **CakeOrderingOrganization** to be controlled not only by yourself, but by ALL your friends. So  that is where Thetta comes in:

```text
pragma solidity ^0.4.22;
import "@thetta/core/contracts/DaoClient.sol";
import "@thetta/core/contracts/DaoBase.sol";

contract CakeOrderingCore is DaoClient {
   function buySomeCakeInternal() internal { 
      // some cake ordering business logic goes here
      ... 
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

 Ok, what have we done?

1. We left all busines logic in **CakeOrderingCore** contract.
2. We added **CakeOrderingOrganization** contract that will controlled by your friends!
3. We added new action **buySomeCake** and attached to it custom permission **BUY\_SOME\_CAKE.**

{% hint style="info" %}
Please check that **buySomeCakeInternal** method is marked with an **internal** modifier
{% endhint %}

\_\_


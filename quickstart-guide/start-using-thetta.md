# 1 - Installing Thetta

## Installing Thetta DAO Framework

Start by going to your project's directory and type:

```bash
$ npm install --save @thetta/core
```

This will install **Thetta core** to the _node\_modules_ directory.  
You can then use **Thetta** **core** from your Solidity contracts by importing it like this:

```javascript
pragma solidity ^0.4.24;

import "@thetta/core/contracts/DaoClient.sol";

contract YourCompany is DaoClient {
  // your business logic goes here
}
```




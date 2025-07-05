#solidity #proxy-pattern #concept 
Initializer is a special function to initialize the logic contract in the [[Proxy Pattern]] like the constructor. Calling constructor in the [[logic contract]] affects variables in itself, not the exact storage in the [[proxy contract]].
## Concepts
In the proxy pattern, the proxy contract saves the exact storage variables that managed in the contract lifecycle. But calling the constructor of proxy delegate-calls the logic contract, which changes the variables in the logic contract itself, not reflecting the exact storage variables in the proxy contract. So the initialization in the proxy pattern is implemented with an alternative way called initializer, which is a special function that should be called after deployment.
### Characteristics
- Called only once: prevent multiple initialization.
- Should be called: if not, there will be a problem called [[Uninitialized Contracts]].
- visibility: generally `public` for in/external use, at least `external` is ok.
- inheritance: initializer does not support implicit C3-linearization call like in constructor, so make sure to manually call the initializers in parent contracts.

## Implementation
- [OpenZeppelin Upgradable](https://docs.openzeppelin.com/contracts/5.x/upgradeable#multiple-inheritance)

### Best Practices
- `initializer` vs `onlyInitializing` modifier: Use `initializer` in the last derived contract and `onlyinitializing` in the parent contract. More general, `initializer` is for public-facing function and `onlyInitializer` is for internal usage called by the public initializer.
```solidity
// contracts/MyContract.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import "@openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol";

contract BaseContract is Initializable {
    uint256 public y;

    function initialize() public onlyInitializing {
        y = 42;
    }
}

contract MyContract is BaseContract {
    uint256 public x;

    function initialize(uint256 _x) public initializer {
        BaseContract.initialize(); // Do not forget this call!
        x = _x;
    }
}
```
- `_init` vs `_init_unchained`: `_init` calls the linearlized parent constructor while `_init_unchained` excludes the parents' and calls only itself. There might possibilities for calling the initializer of the parent contracts twice, if there is multiple inheritance. According to docs, it is not recommended to use `_init_unchained` manually.
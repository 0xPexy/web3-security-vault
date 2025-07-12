#vulnerability #uninitialized-contracts 
The uninitialized contracts is a vulnerability that the [[Initializer]] is not called during the contracts deployment, so can derive security issues like exploiting ownership or destroying the core functionalities.
## Concepts
In [[Proxy Pattern]], there are initializers for the initialization in the logic contract. This function is designed to be called only once during the contract's lifecycle, which is typically enforced by the `initializer` modifier. However, if the deployer miss the initialization, anyone can call the initializer if public, set the variables to weird values that cause disability in the contract.
### ELI5
You made an account for an anonymous website which doesn't have password recovery. The website set default password to 0000. You forgot to change the password after register, someone changed the password to weird value and you cannot use the account anymore.
## Risk Factors
Lots of proxies are adopting the [[UUPSProxy]] to ensure public upgrade, whose initializer is set to a public method. This means anyone can call the initializer if uninitialized, which might cause potential issues like below.
- Ownership Takeover: Attacker set the `owner` address in `Ownable` contracts. 
- DoS: Attacker set the variables like `onwer` to zero address or upgrade to unavailable contracts like missing fallbacks.
- Malfunctions: Attacker set the variables to weird values to take profit.
## Mitigations
- Make sure to call initializer in the contract creation transaction.
- Use OpenZeppelin contracts, especially `_disableInitializers` in all logic contracts. This ensures the logic contracts are not initialized at all.
```solidity
/// @custom:oz-upgrades-unsafe-allow constructor
constructor() {
    _disableInitializers();
}
```

## Case Studies
- [[Parity Wallet Freeze]]: The developers miss calling initializer and the wallet is locked by a user.
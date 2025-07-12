#vulnerability 
Contract centralization is a vulnerability that the core state mutations are only depends on some few accounts.
## Concepts
There are some state-transition methods that should be called only verified accounts to prevent malicious behaviors. For examples, [[UpgradeableProxy]] should have access control, otherwise malicious code can be deployed. However, restricting to few users can cause the problem explained in [[Centralization]].
This is especially important in [[Private Audit]] because it's be significant attack vector in the real production, in contrast to [[Competitive Audit]] it is often considered as known issues.
### ELI5
There is a notebook ledger to manage bonus and minus points of classmates which is managed by the teacher-deputing class president. One day you gone to school, all of your bonus points were changed to minus with traces. You think the points are all manipulated by the class president and talked to teacher, but the president says somebody stole the notebook and manipulated. This shows two disadvantages in [[Centralization]]; The notebook was stolen or the only president manipulated.
### Risk Factors
- Key Compromise: Attackers can access easily if centralized keys are stolen.
- Rug Pull: Admins or Dev teams can do malicious behaviors like upgrading to weird code or exploiting user funds.
- Censorship: Admins can do black or whitelisting.
### Elements
- [[Contract]]
- [[Privileged Roles]]
### Mitigations
- [[Timelock]]: Delays the transaction execution.
- [[DAO]]: Ensures stake-based voting 
- [[Role-Based Access Control]]: Separates the access control. 
## Implementation Examples
### Contracts
- [[UpgradeableProxy]]: Can be an attack vector only few specified owners can upgrade it
- [[Wormhole Bridge]]: If multi-sig keys are stolen, can exploit user funds
### Functions
- [[Modifiers]]: restricting like `onlyOwner` might be attacked if owner address exploited.
### Case Studies
- [[Oasis Counter Exploit]]: Misconfiguration in the roles and privileges in the contract is used for unintended counter-exploit.
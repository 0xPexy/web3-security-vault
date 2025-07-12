#implementation-vulnerabillity #concept 
**ToCToU(Time-of-Check to Time-of-Use)** is a [[Implementation Vulnerability]] that the result is affected by the execution order in the [[race condition]].
## Concept
In the blockchain system, there is the two-way that race condition matters. 
- Intra-transaction: In a single transaction, calling other contracts is available and can access the storage of each others by calling the `external` functions. Even though the calling order is deterministic in aspect of the observer out of the contracts, the victim contract gives the control flow to the attacker contract without knowing the result, so it could be a race condition.
- Inter-transactions: Multiple transactions can access the same storage and the execution order can change the result. The order is not deterministic due to reasons like network delay, miner's reordering, submitting higher gas, etc.
The race condition is inevitable itself, but if there is no way for preventing the unexpected result done by preceding call or transaction, this severely breaks the [[Data Integrity]], leading the victim call or transaction to refer the manipulated storage.
## Classification
### Intra-Transaction (Single Transaction) ToCToU
- [[CEI Violation]]: Interacting with another contract before completely finishing the checking-effecting allows reentrancy, can be exploitable as [[Reentrancy Attack]].
### Inter-Transaction (Multiple Transaction) ToCToU

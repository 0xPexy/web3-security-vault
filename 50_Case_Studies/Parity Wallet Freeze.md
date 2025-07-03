#case-study #uninitialized-contracts
This is a case of [[Uninitialized Contracts]] that uninitialized Parity [[Multi-Signatures]] Wallet contract is locked by the later initializer call by an user name "ghost".
## Concept
The Parity Wallet had an initializer `initWallet(address[] _owners, uint256 _required, uint256 _daylimit)` which uninitialized in deployment. The user name "ghost" called the [initializer](https://etherscan.io/tx/0x05f71e1b2cb4f03e547739db15d080fd30c989eda04d37ce6264c5686e0722c9) with `_owners` to zero addresses.
## References
- [GitHub Issue Link](https://github.com/openethereum/parity-ethereum/issues/6995)
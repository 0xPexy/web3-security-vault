#case-study #oracle #vulnerability 
Cream Finance was exploited for $130M due to a [[Price Oracle Manipulation]] vulnerability, which stemmed from an insecure method of calculating the value of Yearn's yUSDVault token.
## Background
Cream supports the [[Yearn Finance]] yTokens as collaterals to mint cryUSD, which wraps yUSDVault to cream-yUSD to borrow assets in the Cream. 
### Price Oracle
The price of yUSDVault is defined by `pricePerShare = yUSD balance / totalSupply of yUSDVault` in Cream's internal `PriceOracleProxy`. 
The protocol did not use robust off-chain oracles like [[Chainlink]] for this asset, likely because this internal calculation method had previously worked for similar vault tokens (like cTokens).
## Process
The attacker uses 2 contracts, A was for flash loans $500M DAI for minting, B was for $2B ETH for borrowing.
1. The attacker init flash loan in A $500M DAI from MakerDAO.
2. Deposit $500M DAI in Yearn Vault(DAI->yDAI) then [[Curve Finance]] yPool(yDAI->yUSD).
3. Deposit $500M yUSD in yUSDVault. This mints $500M vault so increases the total supply of yUSDVault from $11M to $511M.
4. Deposit $500M yUSDVault in Cream to mint $500M cryUSD.
5. Init flash loan in B $2B ETH from AAVE and deposit to Cream for collateral to borrow yUSDVault.(ETH->cETHER)
6. Borrow $500M in yUSDVault by $2B cETHER collateral.
7. Repeat borrow, transfer and deposit to set A balance with $1.5B cryUSD and $500M yUSDVault, B has debt with $1.5B with collateral $2B.
8. Buy $3M DUSD from Curve to exclude the potential risk. DUSD is by DeFiDollar, which has $3M shares of previous $11M supply in yUSDVault.
9. Redeem $503M yUSD from yUSDVault, the yUSD balance of yUSDVault is $8M and the total supply of yUSDVault is $8M($511M-$503M), meaning `pricePerShare = 1`.
10. Transfer $8M yUSD to the yUSDVault. This only increases the balance without increasing the total supply of vault. This is not deposit but transfer which manipulated only doubling the numerator, ends up with `pricePerShare = 16 / 8 = 2`. 
11. The attacker's $1.5B cryUSD, depending on the yUSDVault price, perceived as $3B in the Cream.
12. Repay the $500M and $2B for flash loans, and borrow all of funds $130M with $1B volatile profit perceived by the Cream.
## Mitigations
- Do not use a single oracle; Use a hybrid way.
- Use flash loan aware design.
## Implications
- Attacker is familiar with DeFi: deposits stablecoins for avoiding blacklists in Curve; used BTC bridge to diversify the exploited assets; used ParaSwap than 1inch for anonymity and airdrop; used EIP-1559
- Isolated lending markets like Kashi in SushiSwap prevents the total exploit even if an asset is hacked.

## References
- [Creamed Cream – Learn the Secret Recipe (Cream Hack Analysis) by Mudit Gupta](https://mudit.blog/cream-hack-analysis/)
- [Hack Analysis: Cream Finance Oct 2021 by ImmuneFi](https://medium.com/immunefi/hack-analysis-cream-finance-oct-2021-fc222d913fc5)
- [Explained: The CREAM Finance Hack (October 2021)](https://www.halborn.com/blog/post/explained-the-cream-finance-hack-october-2021)
Prepared by: [Elia Bordoni](https://elia-bordoni-blockchain-dev.netlify.app/)

### Damn Vulnerable DeFi: Unstoppable

## The assertion if (convertToShares(totalSupply) != balanceBefore) revert InvalidBalance(); cause DOS if transfer token directly to the contract without passing throgh the deposit function

**Description** The vault relies on the invariant that the total number of shares must always correspond to the total value of the underlying tokens. When users call the `UnstoppableVault::deposit` function, they receive share tokens that maintain this 1:1 relationship between shares and underlying assets.
However, the contract also exposes a `UnstoppableVault::transfer` function. If an external user directly transfers even a single underlying token to the vault, the invariant will be permanently broken, as the contract’s balance of underlying tokens will exceed the value represented by the total supply of shares.

<details>
<summary>vulnerability</summary>

```solidity

```

</details>

**Impact** This results in a permanent Denial of Service (DOS) for the flash loan functionality, since the check
`convertToShares(totalSupply) == totalAssets()` will always fail.

**ProofOfConcept** Add the following to the `Unstoppable.t.sol` test file on `Unstoppable.t.sol::test_unstoppable` and run the test. Check log to see `UnstoppableVault::convertToShares(totalSupply)` and `UnstoppableVault::totalAssets()` values.

<details>
<summary>Test Code</summary>

```solidity

```

</details>

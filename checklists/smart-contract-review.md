# Smart Contract Review Checklist

## Accounting and invariants
- [ ] Sum of user balances == token balance held by contract, per asset
- [ ] Shares * price per share == assets under management, check rounding direction
- [ ] Rounding always favors the protocol, never the withdrawer
- [ ] First depositor / inflation attack: is share price manipulable at low TVL
- [ ] Donation attack: does a direct transfer change the share price

## Access control
- [ ] Every external or public state-changing function has an explicit guard
- [ ] `initialize()` callable once, not front-runnable on a fresh proxy
- [ ] Upgrade authority is a multisig or timelock, not an EOA
- [ ] Role renounce and transfer paths do not strand funds

## Oracles and pricing
- [ ] Spot price from a single DEX pool means flash-loan manipulable
- [ ] TWAP window length versus cost of manipulation
- [ ] Staleness check on Chainlink `updatedAt`
- [ ] Decimal normalization between tokens of different precision
- [ ] Sequencer uptime feed used on L2 deployments

## Bridges specifically
- [ ] Message replay: is the nonce or consumed flag set before or after the external call
- [ ] Signature domain separation: chainId, contract address, nonce
- [ ] Validator set rotation: does an in-flight message survive it
- [ ] Rate limits per epoch, and what happens exactly at the boundary
- [ ] Emergency pause does not block withdrawals of already-bridged assets
- [ ] Token mapping: can a fake local token be registered for a real remote asset

## External calls
- [ ] Reentrancy: state updated before external call on every path
- [ ] Read-only reentrancy via view functions consumed by third parties
- [ ] Return value of transfer and transferFrom checked, non-reverting tokens
- [ ] Fee-on-transfer and rebasing tokens handled or explicitly excluded

## Arithmetic
- [ ] Every division: which way it rounds, and who eats the dust
- [ ] Unchecked blocks: can any subtraction underflow
- [ ] Casting between uint256, int256 and smaller types

## Testing
- [ ] Fuzz the invariants above with Foundry `invariant_` tests
- [ ] Fork test against real mainnet state at a pinned block
- [ ] Differential test: reference implementation versus target

# Smart Contract & Bridge Audit Notes

Working notes from independent smart contract and cross-chain bridge security reviews.
Everything here is either public already-patched information or original analysis of
open-source code.

## What lives here

- `checklists/` - the invariant and edge-case lists I run against every target
- `notes/` - per-protocol analysis of public repositories
- `pocs/` - minimal Foundry / Anchor reproductions, never against live funds

## Method

1. **Read the accounting, not the marketing.** Most bridge bugs reduce to one thing:
   the balance the contract believes diverges from the balance that exists.
2. **Enumerate trust boundaries.** Who can pause, upgrade, mint, set fees, change the
   oracle? If the answer is an EOA, that is already a finding.
3. **Attack the invariants, not the functions.** Write the invariant in one line, then
   find the sequence of calls that breaks it.
4. **Prove it in code.** A finding without a runnable test is a hypothesis.

## Scope statement

I only test programs that publish a scope and authorize testing. No live-fund
exploitation, no third-party systems outside a published bounty scope.

## Contact

Bounty profile: hackenproof.com/hackers/digitalisasidesa1

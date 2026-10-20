# 0xseam

Independent smart contract and cross-chain bridge security research.

- HackenProof: https://hackenproof.com/hackers/0xseam
- GitHub: https://github.com/0xseam

## Focus

Bridges and the accounting layer underneath them. The bug class I look for most:
the number the contract believes it holds diverging from the number that actually
exists. That single framing covers donation attacks, fee-on-transfer accounting,
partial fills, replay, and most of the drainers I have read.

## Public work

- `checklists/smart-contract-review.md` - invariant and edge-case list used on every target
- `notes/how-i-review-a-bridge.md` - the actual order of operations I follow

## Scope discipline

Only published, authorized bounty scopes. No live-fund exploitation. Findings are
reported to the program, not published before the fix ships.

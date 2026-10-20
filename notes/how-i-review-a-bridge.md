# How I Review a Bridge

A bridge is an accounting system with a messaging layer bolted on. That framing
finds bugs faster than reading function by function.

## 1. Map the trust surface

| Question | Why it matters |
|---|---|
| Who can mint on the destination chain | unlimited mint is total loss |
| Who can pause | pause that blocks withdrawals is a soft rug |
| Who sets fees and rate limits | unbounded fee drains users |
| Is there an upgrade path | upgradeable plus EOA owner equals game over |
| How are validators rotated | rotation bugs allow forged messages |

## 2. Find the state that must never diverge

For every asset, write down two numbers:

- what the contract thinks is locked (internal accounting)
- what the contract actually holds (token balanceOf)

Any code path that changes one without the other is a candidate. This catches
donation attacks, fee-on-transfer accounting, and partial-fill bugs in one sweep.

## 3. Trace a single message end to end

Pick one realistic transfer and follow it through every hop:

1. lock or burn on source, is there a receipt
2. emit event, what exactly is in the payload
3. off-chain attestation, who signs, what domain
4. execute on destination, is replay protection set before the call
5. failure path, does the user get funds back or are they stuck

The failure path is where bridges bleed. Success paths get tested by users every
day; failure paths get tested by attackers.

## 4. Then attack

Only after the above, start looking for:

- signature malleability and domain reuse across chains
- nonce handling under concurrent messages
- validator set changes with messages in flight
- decimal and token-mapping confusion
- reentrancy through the token callback during a bridge-out

## Reporting discipline

Every finding needs: invariant stated, breaking sequence, runnable PoC, and the
realistic impact in funds at risk. If the impact is "theoretical", say so.

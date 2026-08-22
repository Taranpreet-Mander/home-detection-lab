# attacks

the attacks i run in the lab to test my detections.

**rules for myself:**
- only my own victim vm
- only on the host-only network
- never against anything i dont own (illegal + not the point)

## log of what ive tried

| # | attack | tool | detected? | notes |
|---|--------|------|-----------|-------|
| 1 | brute force login | hydra | yes | [failed-logins](../detections/failed-logins.md) |
| 2 | powershell download | powershell | partly | [suspicious-powershell](../detections/suspicious-powershell.md) - my rule is buggy |

## want to try next

- [ ] atomic red team tests (https://github.com/redcanaryco/atomic-red-team)
- [ ] make a new local user (persistence)
- [ ] nmap scan from kali (recon)
- [ ] scheduled task persistence

for each one add a row and write down if i caught it or not. the ones i cant catch are probably the most useful to learn from.

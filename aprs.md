---
label: APRs
---

# APRs

### Reward APR

Reward APR is based on token rewards. These are emitted every second, so the calculated APR closely matches the expected returns.

Projects usually set these on a weekly basis, for example Aerodrome sets reward rates for each pool every Thursday at 00:00 UTC. The reward rate is then fixed until the next epoch change.

### Fee APR

Fee APR is based on swap fees. These only accrue whenever a swap takes place in that pool. In order to calculate an APR estimate we take the previous 7 days worth of swap fees and compare with the current amount of funds in the pool.

This is a rough estimate, and the actual returns may vary, as the previous week's swap volume may not be representative of the current week.

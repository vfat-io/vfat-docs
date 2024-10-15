# Automatic Rebalance

When a concentrated liquidity (CL) position is out of range, it needs rebalancing. This is done by withdrawing the single token in the position (since out of range positions are only comprised of one token), swapping an appropriate amount of it for the other token, and then creating a new CL position in-range.

Auto-Rebalance does this by monitoring all out of range positions in a loop. When one is found, we retrieve quotes for swaps from all supported aggregators, then simulate the transaction using the best one. If it results in less dust (leftover tokens) and price impact than the user has specified, the rebalance takes place, otherwise the attempt is repeated in the next loop.

Auto-Rebalance does not take place if the protocol fees would be less than the gas cost. Depending on the chain this could mean the position needs to be larger than $1,000 (L2s) or $10,000 (Ethereum).

The new position will use the same width as the previous one, but centered under the new price. For example a -1% +2% width position (-100 ticks, +200 ticks from active tick space), will remain -1% +2% after rebalancing.

Auto-Rebalance will automatically be activated for the new position as well, using the same settings as the last position.

Enabling Auto-Rebalance is done in the Rebalance tab in an active CL position (whether in range or not).

## Required Configuration

### Rewards

There are two ways to handle the rewards that a position has accumulated before it went out of range, they can be either compounded back into the position or harvested out to the user's wallet. Optionally when harvesting it can be converted to a different token. The fee charged is 1.8% of rewards. 

### Slippage

The user has to select an appropriate slippage. This is used both for the dust calculation and for price impact. For instance in a pool with large liquidity rebalancing may work with as low as 0.1%, but for smaller pools or larger positions a higher value would be required.

Further to slippage there are two advanced configuration options. These are optional, and they are used to create a custom range in which Auto-Rebalance should take place. The default setting, when not using advanced configuration, is to always attempt to rebalance when the position is out of range.

## Advanced Configuration

### Buffer

This adds a buffer on either side of the position range, in which rebalancing should not take place. For example if a user's position is in the 3000-3300 range, a buffer of -1% on the lower side will mean that rebalance will not take place when the price is between 2970 and 3000. While the price is in that buffer the position will remain as is.

Similarly in that example a buffer of +1% on the upper side will mean that rebalance will not take place between 3300 and 3333.

Buffer settings are relative, so a buffer of -1% +1% will remain so when a position is rebalanced (-1% from the new lower end, +1% from the new upper end).

### Stop Loss

Lastly stop loss values can be set on either side. Continuing the previous example, a stop loss value of 2700 on the lower side will mean that automatic rebalancing will stop completely if the price drops below 2700.

The same mechanism on the upper side if setting a stop loss of 3600 would disable all automatic rebalancing once the price goes above 3600.

Stop loss settings are absolute, so a stop loss setting of 2700, 3600 will remain the same when a position is rebalanced.

### Advanced Settings Example

Putting all advanced settings together, for the example position of 3000-3300, creates two ranges on either side of the user's position where automatic rebalance will take place:

2700-2970 and 3333-3600

As well as four ranges where automatic rebalance will not take place:

0-2700, 2970-3000, 3300-3333, 3600-infinite

The advanced settings are entirely optional, the default behaviour when not setting a buffer and stop loss would be to rebalance whenever the position is out of range.

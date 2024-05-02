---
label: Sickle
order: 101
---

# Sickle

[Sickle](https://basescan.org/address/0xFfF75D099baeE29F447866bC5299Cd67C04761C8#code#F1#L1) is a smart contract wallet, deployed for each vfat.io user on each chain in order to manage their yield farming positions. It is similar to DSProxy.

It is deployed by the [SickleFactory](https://basescan.org/address/0x71D234A3e1dfC161cc1d081E6496e76627baAc31#code#F1#L1), the first time a user opens a position on a chain.

It has a [Multicall](https://basescan.org/address/0xFfF75D099baeE29F447866bC5299Cd67C04761C8#code#F3#L41) function which batches actions together such as swapping, adding liquidity, staking etc. This function is gated, it can only be called by strategies registered in [SickleRegistry](https://basescan.org/address/0x2ef5eafa8711e2441bd519eed5d09f8dfef2ecf3#code#F1#L1), and it can only target contracts also registered in [SickleRegistry](https://basescan.org/address/0x2ef5eafa8711e2441bd519eed5d09f8dfef2ecf3#code#F1#L33).

Strategies are multicall recipes. Their functions can either be called by anyone if they don't require a deployed Sickle (`deposit`) or only by Sickle owners (`withdraw`, `harvest`, `compound`, etc).

The currently deployed ones are:

[FarmStrategy](https://basescan.org/address/0x5A72C0f4Bf7f3Ddf1370780d405e29149b128A04#code) is the main strategy, its functions include `deposit` and `withdraw` (swapping in any token onto a staked liquidity position, or swapping out to any token), as well as `harvest` (claiming rewards and swapping them) or `compound`, each of which can be done in a single transaction. It also offers combined actions `exit` (harvest + withdraw) and `rebalance` (withdraw one position and deposit into another).

[SimpleFarmStrategy](0x9b381108Ef12A138a5b7cF231Fbbef4f20e72306) has `deposit`, `withdraw`, `harvest` and `exit`, but it doesn't do any swaps. It can be used to deposit/withdraw the LP token itself, and to harvest the reward token.

[NftFarmStrategy](https://basescan.org/address/0x3B8886C3f6d3BA4a75D3BEcb3c83864C0C01e1F3#writeContract) has `depositErc721` and `withdrawErc721` for depositing an ERC721 token or withdrawing it directly from a staking contract.

[SweepStrategy](https://basescan.org/address/0x29D82976C8babb7d5a82c78c6Ef4c2a2dDc64125#writeContract) allows you to sweep any tokens that have been left in your Sickle, or sent there by accident.

# Party Protocol - Invitational Contest

## Contest Details
- Total Prize Pool: $17,050 USDC
  - HM awards: $11,178 USDC
  - QA awards: $1,315 USDC
  - Gas awards: $657 USDC
  - Judge awards: $3,400 USDC
  - Scout awards: $500 USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-05-party-dao-invitational/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts May 26, 2023 20:00 UTC
- Ends May 30, 2023 20:00 UTC

## Automated Findings / Publicly Known Issues

Automated findings output for the audit can be found [here](add link to report) within 24 hours of audit opening.

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

## Overview

Party Protocol aims to be a standard for group coordination, providing on-chain functionality for essential group behaviors:

1. **Formation:** Assembling a group and combining resources.

1. **Coordination:** Making decisions and taking action together.

1. **Distribution:** Sharing resources with group members.

Rather than pursuing a generic approach to start, we released the first version of the Party Protocol with a focus on NFTs, largely inspired by our initial work on PartyBid. The next release will expand the protocol past that, introducing the new concept of parties that can hold and use ETH.

The new type of party in this next release which we've referred to as "ETH parties", to distinguish from previous "NFT parties" that were created to acquire NFTs, have less guard rails than NFT parties did. This was by design, it allows ETH parties much more flexibility and purpose. Due to this, however, a 51% attack happening to a party has become much more of a concern.

To protect members from the threat of a 51% attack, we have added a "rage quit" ability (like that of MolochDAO's). If this attack were to happen, everyone else in the party could rage quit during the execution delay and take their share of the party's fungible assets with them and the malicious proposal would have no impact.

Rage quit can is set by host of the party and can be changed at any time, unless it is locked to `ENABLE_RAGEQUIT_PERMENANTLY` or `DISABLE_RAGEQUIT_PERMENANTLY`. To allow it to have more states than just simply on or off, rage quit is implemented as a timestamp until which rage quit is enabled to support a wider set of party configurations. For example, a party that does not wish to enable rage quit by default but wishes to still have the option to enable it for a limited time in case of an emergency to allow members to exit could still do so.

This is a critical feature for the protocol, and we want to make sure it is implemented correctly. We are looking for a security audit of the rage quit functionality, particularly its interaction with governance.

## Documentation

For more information on Party Protocol, see the documentation [here](https://github.com/code-423n4/2023-04-party/tree/main/docs).

- [Overview](https://github.com/code-423n4/2023-04-party/blob/main/docs/README.md)
- [Crowdfund](https://github.com/code-423n4/2023-04-party/blob/main/docs/crowdfund.md)
- [Governance](https://github.com/code-423n4/2023-04-party/blob/main/docs/governance.md)

## Quickstart Command

Here's a one-liner to immediately get started with the codebase. It will clone the project, build it, run every test, and display gas reports:

```bash
export ETH_RPC_URL='<your_alchemy_mainnet_url_here>' && rm -Rf 2023-04-party || true && git clone https://github.com/code-423n4/2023-04-party -j8 --recurse-submodules && cd 2023-04-party && foundryup && forge install && yarn install && forge test -f $ETH_RPC_URL --gas-report
```

## Scope

- `PartyGovernaneNFT` (only code related to `setRageQuit()` and `rageQuit()`)
- `PartyGovernance` (only code related to `lastBurnTime`)

## Scoping Details

```
- If you have a public code repo, please share it here: There's a private internal PR covering the new features, but the protocol is here: https://github.com/PartyDAO/party-protocol
- How many contracts are in scope?: 2
- Total SLoC for these contracts?: 77
- How many external imports are there?: 0
- How many separate interfaces and struct definitions are there for the contracts within scope?: 0
- Does most of your code generally use composition or inheritance?: inheritance
- How many external calls?: 0
- What is the overall line coverage percentage provided by your tests?: 80
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?: true
- Please describe required context: Understanding how party cards are minted and how governance works and how voting power is updated and snapshotted will help with understanding the full scope of what happens when a user rage quits from the party.
- Does it use an oracle?: no
- Does the token conform to the ERC20 standard?: no
- Are there any novel or unique curve logic or mathematical models?: no
- Does it use a timelock function?: no
- Is it an NFT?: true
- Does it have an AMM?: false
- Is it a fork of a popular project?: false
- Does it use rollups?: false
- Is it multi-chain?: false
- Does it use a side-chain?: false
```

## Slither Issue

Note that slither does not seem to be working with the repo as-is 🤷, resulting in an enum type not found error:

```
slither.solc_parsing.exceptions.ParsingError: Type not found enum Crowdfund.CrowdfundLifecycle
```

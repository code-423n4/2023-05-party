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

### Publicly Known Issues
There are some potential risks with this feature that are known possibilities. These are not considered vulnerabilities and are described below:

1. Ragequitting too many tokens at once might run out of gas and fail. The end user can only ragequit a limited number of tokens.
2. If a user intentionally or accidentally excludes a token in their ragequit, they forfeit that token and will not be able to claim it.
3. After a user rage quits, the votes delegated to them will not be undone. Those who delegated to them will need to redelegate their votes.

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

## Overview

Party Protocol aims to be a standard for group coordination, providing on-chain functionality for essential group behaviors:

1. **Formation:** Assembling a group and combining resources.

1. **Coordination:** Making decisions and taking action together.

1. **Distribution:** Sharing resources with group members.

The first version of the Party Protocol had a focus on NFTs. The next release will expand the protocol, introducing the new concept of parties that can hold and use ETH to do anything.

The new type of party in this next release is something we've referred to as "ETH parties," to distinguish from previous "NFT parties." ETH parties have fewer restrictions and guardrails than NFT parties did. This is intentional, since it allows ETH parties much more flexibility and freedom. However, this also means that malicious proposals are more of a concern. If a malicious contributor is able to obtain a high amount of voting power in a Party such that they can pass proposals on their own, they could pass a proposal to drain a Party of its assets. Party Hosts already have a veto power to block malicious proposals, but we decided to add additional protections against this type of attack.

To protect members from the threat of a 51% attack (or any attack where a single actor has enough votes to pass a proposal), we have added a "rage quit" ability like that of MolochDAO's. If a malicious proposal were to pass, rage quit would allow everyone else in the party to quit during the execution delay, each taking their share of the party's fungible assets with them.

Rage quit can be enabled or disabled by a Party Host and can be changed at any time, unless it is locked to `ENABLE_RAGEQUIT_PERMENANTLY` or `DISABLE_RAGEQUIT_PERMENANTLY`. To offer more flexibility than just on or off, rage quit is implemented as a timestamp until which rage quit is enabled. This supports a wider set of party configurations. For example, a Party Host could enable rage quit for a limited time, after which it would be disabled again.

The rage quit feature is to the protocol, and we want to make sure it is implemented correctly. We are looking for a security audit of the rage quit functionality, particularly its interaction with governance.

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

- `PartyGovernanceNFT` (new functions `setRageQuit()` and `rageQuit()`)
- `PartyGovernance` (new function `lastBurnTime`)
- In addition to code in these new functions, any vulnerabilities or unintended effects of these functions related to voting or the party's assets is in scope.

### Files in scope
|File|[SLOC](#nowhere "(nSLOC, SLOC, Lines)")|Description and [Coverage](#nowhere "(Lines hit / Total)")|Libraries|
|:-|:-:|:-|:-|
|_Contracts (1)_|
|[contracts/party/PartyGovernanceNFT.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/party/PartyGovernanceNFT.sol) [💰](#nowhere "Payable Functions") [Σ](#nowhere "Unchecked Blocks")|[270](#nowhere "(nSLOC:230, SLOC:270, Lines:413)")|[92.63%](#nowhere "(Hit:88 / Total:95)")| `@openzeppelin/*`|
|_Abstracts (1)_|
|[contracts/party/PartyGovernance.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/party/PartyGovernance.sol) [🖥](#nowhere "Uses Assembly") [💰](#nowhere "Payable Functions") [👥](#nowhere "DelegateCall") [🧮](#nowhere "Uses Hash-Functions")|[729](#nowhere "(nSLOC:642, SLOC:729, Lines:1103)")|[76.82%](#nowhere "(Hit:169 / Total:220)")||
|Total (over 2 files):| [999](#nowhere "(nSLOC:872, SLOC:999, Lines:1516)") |[81.59%](#nowhere "Hit:257 / Total:315")|


### All other source contracts (not in scope)
|File|[SLOC](#nowhere "(nSLOC, SLOC, Lines)")|Description and [Coverage](#nowhere "(Lines hit / Total)")|Libraries|
|:-|:-:|:-|:-|
|_Contracts (32)_|
|[contracts/gatekeepers/AllowListGateKeeper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/gatekeepers/AllowListGateKeeper.sol) [🖥](#nowhere "Uses Assembly")|[25](#nowhere "(nSLOC:21, SLOC:25, Lines:37)")|[100.00%](#nowhere "(Hit:7 / Total:7)")| `@openzeppelin/*`|
|[contracts/utils/Proxy.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/Proxy.sol) [🖥](#nowhere "Uses Assembly") [💰](#nowhere "Payable Functions") [👥](#nowhere "DelegateCall")|[26](#nowhere "(nSLOC:26, SLOC:26, Lines:37)")|[100.00%](#nowhere "(Hit:2 / Total:2)")||
|[contracts/gatekeepers/TokenGateKeeper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/gatekeepers/TokenGateKeeper.sol)|[29](#nowhere "(nSLOC:25, SLOC:29, Lines:51)")|[100.00%](#nowhere "(Hit:7 / Total:7)")||
|[contracts/party/Party.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/party/Party.sol) [💰](#nowhere "Payable Functions")|[36](#nowhere "(nSLOC:36, SLOC:36, Lines:54)")|[100.00%](#nowhere "(Hit:1 / Total:1)")||
|[contracts/renderers/fonts/PixeldroidConsoleFont.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/fonts/PixeldroidConsoleFont.sol)|[36](#nowhere "(nSLOC:36, SLOC:36, Lines:56)")|[100.00%](#nowhere "(Hit:1 / Total:1)")| `solmate/*`|
|[contracts/crowdfund/AuctionCrowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/AuctionCrowdfund.sol) [💰](#nowhere "Payable Functions")|[38](#nowhere "(nSLOC:35, SLOC:38, Lines:70)")|[100.00%](#nowhere "(Hit:14 / Total:14)")||
|[contracts/market-wrapper/FoundationMarketWrapper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/market-wrapper/FoundationMarketWrapper.sol)|[39](#nowhere "(nSLOC:35, SLOC:39, Lines:104)")|[0.00%](#nowhere "(Hit:0 / Total:10)")||
|[contracts/party/PartyFactory.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/party/PartyFactory.sol) [🌀](#nowhere "create/create2")|[39](#nowhere "(nSLOC:32, SLOC:39, Lines:48)")|[80.00%](#nowhere "(Hit:4 / Total:5)")||
|[contracts/proposals/OperatorProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/OperatorProposal.sol)|[44](#nowhere "(nSLOC:37, SLOC:44, Lines:68)")|[100.00%](#nowhere "(Hit:10 / Total:10)")||
|[contracts/proposals/FractionalizeProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/FractionalizeProposal.sol)|[51](#nowhere "(nSLOC:49, SLOC:51, Lines:81)")|[100.00%](#nowhere "(Hit:10 / Total:10)")||
|[contracts/market-wrapper/ZoraMarketWrapper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/market-wrapper/ZoraMarketWrapper.sol)|[53](#nowhere "(nSLOC:48, SLOC:53, Lines:119)")|[0.00%](#nowhere "(Hit:0 / Total:14)")||
|[contracts/renderers/RendererStorage.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/RendererStorage.sol)|[53](#nowhere "(nSLOC:50, SLOC:53, Lines:93)")|[33.33%](#nowhere "(Hit:2 / Total:6)")| `solmate/*`|
|[contracts/market-wrapper/KoansMarketWrapper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/market-wrapper/KoansMarketWrapper.sol)|[56](#nowhere "(nSLOC:52, SLOC:56, Lines:119)")|[0.00%](#nowhere "(Hit:0 / Total:21)")||
|[contracts/market-wrapper/NounsMarketWrapper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/market-wrapper/NounsMarketWrapper.sol)|[56](#nowhere "(nSLOC:52, SLOC:56, Lines:118)")|[0.00%](#nowhere "(Hit:0 / Total:21)")||
|[contracts/utils/PartyHelpers.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/PartyHelpers.sol)|[76](#nowhere "(nSLOC:64, SLOC:76, Lines:99)")|[100.00%](#nowhere "(Hit:19 / Total:19)")||
|[contracts/globals/Globals.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/globals/Globals.sol)|[83](#nowhere "(nSLOC:83, SLOC:83, Lines:110)")|[38.10%](#nowhere "(Hit:8 / Total:21)")||
|[contracts/crowdfund/RollingAuctionCrowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/RollingAuctionCrowdfund.sol) [💰](#nowhere "Payable Functions") [🧮](#nowhere "Uses Hash-Functions")|[89](#nowhere "(nSLOC:79, SLOC:89, Lines:191)")|[90.91%](#nowhere "(Hit:30 / Total:33)")| `@openzeppelin/*`|
|[contracts/crowdfund/CollectionBuyCrowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol) [💰](#nowhere "Payable Functions")|[101](#nowhere "(nSLOC:91, SLOC:101, Lines:166)")|[80.00%](#nowhere "(Hit:12 / Total:15)")||
|[contracts/crowdfund/CrowdfundNFT.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/CrowdfundNFT.sol)|[102](#nowhere "(nSLOC:93, SLOC:102, Lines:157)")|[68.97%](#nowhere "(Hit:20 / Total:29)")||
|[contracts/crowdfund/BuyCrowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/BuyCrowdfund.sol) [💰](#nowhere "Payable Functions")|[109](#nowhere "(nSLOC:102, SLOC:109, Lines:180)")|[90.91%](#nowhere "(Hit:20 / Total:22)")||
|[contracts/proposals/ListOnZoraProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/ListOnZoraProposal.sol) [🧮](#nowhere "Uses Hash-Functions") [♻️](#nowhere "TryCatch Blocks")|[152](#nowhere "(nSLOC:139, SLOC:152, Lines:206)")|[100.00%](#nowhere "(Hit:24 / Total:24)")||
|[contracts/operators/CollectionBatchBuyOperator.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/operators/CollectionBatchBuyOperator.sol) [🖥](#nowhere "Uses Assembly") [💰](#nowhere "Payable Functions")|[155](#nowhere "(nSLOC:146, SLOC:155, Lines:236)")|[94.44%](#nowhere "(Hit:51 / Total:54)")| `@openzeppelin/*`|
|[contracts/crowdfund/CollectionBatchBuyCrowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/CollectionBatchBuyCrowdfund.sol) [🖥](#nowhere "Uses Assembly") [💰](#nowhere "Payable Functions")|[168](#nowhere "(nSLOC:166, SLOC:168, Lines:254)")|[91.11%](#nowhere "(Hit:41 / Total:45)")| `@openzeppelin/*`|
|[contracts/crowdfund/CrowdfundFactory.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/CrowdfundFactory.sol) [💰](#nowhere "Payable Functions")|[188](#nowhere "(nSLOC:154, SLOC:188, Lines:253)")|[96.30%](#nowhere "(Hit:26 / Total:27)")||
|[contracts/proposals/ArbitraryCallsProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/ArbitraryCallsProposal.sol) [🖥](#nowhere "Uses Assembly") [🧮](#nowhere "Uses Hash-Functions")|[199](#nowhere "(nSLOC:175, SLOC:199, Lines:258)")|[96.83%](#nowhere "(Hit:61 / Total:63)")||
|[contracts/crowdfund/ETHCrowdfundBase.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/ETHCrowdfundBase.sol)|[212](#nowhere "(nSLOC:206, SLOC:212, Lines:342)")|[96.77%](#nowhere "(Hit:90 / Total:93)")||
|[contracts/proposals/ProposalExecutionEngine.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/ProposalExecutionEngine.sol) [🖥](#nowhere "Uses Assembly") [🧮](#nowhere "Uses Hash-Functions")|[215](#nowhere "(nSLOC:205, SLOC:215, Lines:308)")|[83.78%](#nowhere "(Hit:62 / Total:74)")||
|[contracts/crowdfund/InitialETHCrowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/InitialETHCrowdfund.sol) [💰](#nowhere "Payable Functions") [📤](#nowhere "Initiates ETH Value Transfer")|[275](#nowhere "(nSLOC:247, SLOC:275, Lines:398)")|[93.75%](#nowhere "(Hit:60 / Total:64)")||
|[contracts/distribution/TokenDistributor.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/distribution/TokenDistributor.sol) [🖥](#nowhere "Uses Assembly") [💰](#nowhere "Payable Functions") [👥](#nowhere "DelegateCall")|[275](#nowhere "(nSLOC:228, SLOC:275, Lines:372)")|[87.88%](#nowhere "(Hit:58 / Total:66)")||
|[contracts/crowdfund/ReraiseETHCrowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/ReraiseETHCrowdfund.sol) [💰](#nowhere "Payable Functions") [📤](#nowhere "Initiates ETH Value Transfer")|[291](#nowhere "(nSLOC:259, SLOC:291, Lines:485)")|[90.76%](#nowhere "(Hit:108 / Total:119)")||
|[contracts/renderers/CrowdfundNFTRenderer.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/CrowdfundNFTRenderer.sol)|[323](#nowhere "(nSLOC:303, SLOC:323, Lines:364)")|[94.92%](#nowhere "(Hit:56 / Total:59)")||
|[contracts/renderers/PartyNFTRenderer.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/PartyNFTRenderer.sol) [🧮](#nowhere "Uses Hash-Functions")|[424](#nowhere "(nSLOC:385, SLOC:424, Lines:473)")|[90.00%](#nowhere "(Hit:72 / Total:80)")||
|_Abstracts (20)_|
|[contracts/utils/EIP165.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/EIP165.sol)|[6](#nowhere "(nSLOC:6, SLOC:6, Lines:12)")|[100.00%](#nowhere "(Hit:1 / Total:1)")||
|[contracts/proposals/OpenseaHelpers.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/OpenseaHelpers.sol)|[10](#nowhere "(nSLOC:7, SLOC:10, Lines:14)")|-||
|[contracts/tokens/ERC1155Receiver.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/tokens/ERC1155Receiver.sol)|[10](#nowhere "(nSLOC:10, SLOC:10, Lines:14)")|[100.00%](#nowhere "(Hit:1 / Total:1)")||
|[contracts/utils/Multicall.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/Multicall.sol) [👥](#nowhere "DelegateCall")|[13](#nowhere "(nSLOC:13, SLOC:13, Lines:18)")|[0.00%](#nowhere "(Hit:0 / Total:4)")||
|[contracts/tokens/ERC721Receiver.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/tokens/ERC721Receiver.sol)|[19](#nowhere "(nSLOC:14, SLOC:19, Lines:28)")|[100.00%](#nowhere "(Hit:2 / Total:2)")||
|[contracts/utils/Implementation.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/Implementation.sol)|[21](#nowhere "(nSLOC:21, SLOC:21, Lines:30)")|-||
|[contracts/utils/ReadOnlyDelegateCall.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/ReadOnlyDelegateCall.sol) [👥](#nowhere "DelegateCall") [♻️](#nowhere "TryCatch Blocks")|[24](#nowhere "(nSLOC:24, SLOC:24, Lines:37)")|[100.00%](#nowhere "(Hit:4 / Total:4)")||
|[contracts/proposals/ZoraHelpers.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/ZoraHelpers.sol)|[26](#nowhere "(nSLOC:15, SLOC:26, Lines:42)")|-||
|[contracts/proposals/DistributeProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/DistributeProposal.sol)|[30](#nowhere "(nSLOC:28, SLOC:30, Lines:40)")|[100.00%](#nowhere "(Hit:3 / Total:3)")||
|[contracts/proposals/ListOnOpenseaProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/ListOnOpenseaProposal.sol)|[33](#nowhere "(nSLOC:31, SLOC:33, Lines:53)")|[100.00%](#nowhere "(Hit:2 / Total:2)")||
|[contracts/proposals/AddAuthorityProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/AddAuthorityProposal.sol)|[35](#nowhere "(nSLOC:33, SLOC:35, Lines:52)")|[90.00%](#nowhere "(Hit:9 / Total:10)")||
|[contracts/proposals/ProposalStorage.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/ProposalStorage.sol) [🖥](#nowhere "Uses Assembly") [👥](#nowhere "DelegateCall") [🧮](#nowhere "Uses Hash-Functions")|[40](#nowhere "(nSLOC:36, SLOC:40, Lines:57)")|[87.50%](#nowhere "(Hit:7 / Total:8)")||
|[contracts/vendor/solmate/ERC20.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/vendor/solmate/ERC20.sol) [🧮](#nowhere "Uses Hash-Functions") [🔖](#nowhere "Handles Signatures: ecrecover") [Σ](#nowhere "Unchecked Blocks")|[114](#nowhere "(nSLOC:106, SLOC:114, Lines:199)")|[26.92%](#nowhere "(Hit:7 / Total:26)")||
|[contracts/vendor/solmate/ERC721.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/vendor/solmate/ERC721.sol) [Σ](#nowhere "Unchecked Blocks")|[126](#nowhere "(nSLOC:116, SLOC:126, Lines:219)")|[86.84%](#nowhere "(Hit:33 / Total:38)")||
|[contracts/vendor/solmate/ERC1155.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/vendor/solmate/ERC1155.sol) [Σ](#nowhere "Unchecked Blocks")|[171](#nowhere "(nSLOC:135, SLOC:171, Lines:253)")|[25.00%](#nowhere "(Hit:11 / Total:44)")||
|[contracts/crowdfund/BuyCrowdfundBase.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol) [🖥](#nowhere "Uses Assembly")|[175](#nowhere "(nSLOC:151, SLOC:175, Lines:239)")|[100.00%](#nowhere "(Hit:41 / Total:41)")||
|[contracts/crowdfund/AuctionCrowdfundBase.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/AuctionCrowdfundBase.sol) [💰](#nowhere "Payable Functions") [👥](#nowhere "DelegateCall")|[210](#nowhere "(nSLOC:191, SLOC:210, Lines:351)")|[90.91%](#nowhere "(Hit:60 / Total:66)")||
|[contracts/renderers/RendererBase.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/RendererBase.sol)|[239](#nowhere "(nSLOC:226, SLOC:239, Lines:277)")|[44.67%](#nowhere "(Hit:67 / Total:150)")| `contracts/*`|
|[contracts/proposals/ListOnOpenseaAdvancedProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/ListOnOpenseaAdvancedProposal.sol) [🖥](#nowhere "Uses Assembly")|[319](#nowhere "(nSLOC:295, SLOC:319, Lines:433)")|[96.88%](#nowhere "(Hit:93 / Total:96)")||
|[contracts/crowdfund/Crowdfund.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/crowdfund/Crowdfund.sol) [🖥](#nowhere "Uses Assembly") [💰](#nowhere "Payable Functions") [👥](#nowhere "DelegateCall") [🧮](#nowhere "Uses Hash-Functions") [♻️](#nowhere "TryCatch Blocks")|[525](#nowhere "(nSLOC:458, SLOC:525, Lines:771)")|[66.27%](#nowhere "(Hit:110 / Total:166)")||
|_Libraries (9)_|
|[contracts/utils/LibAddress.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/LibAddress.sol)|[11](#nowhere "(nSLOC:11, SLOC:11, Lines:16)")|[0.00%](#nowhere "(Hit:0 / Total:4)")||
|[contracts/utils/LibRawResult.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/LibRawResult.sol) [🖥](#nowhere "Uses Assembly")|[13](#nowhere "(nSLOC:13, SLOC:13, Lines:18)")|-||
|[contracts/utils/LibSafeERC721.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/LibSafeERC721.sol)|[15](#nowhere "(nSLOC:15, SLOC:15, Lines:27)")|[0.00%](#nowhere "(Hit:0 / Total:4)")||
|[contracts/globals/LibGlobals.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/globals/LibGlobals.sol)|[23](#nowhere "(nSLOC:23, SLOC:23, Lines:43)")|-||
|[contracts/utils/LibERC20Compat.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/LibERC20Compat.sol) [🖥](#nowhere "Uses Assembly")|[27](#nowhere "(nSLOC:27, SLOC:27, Lines:33)")|[0.00%](#nowhere "(Hit:0 / Total:11)")||
|[contracts/proposals/LibProposal.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/LibProposal.sol)|[29](#nowhere "(nSLOC:21, SLOC:29, Lines:34)")|[0.00%](#nowhere "(Hit:0 / Total:8)")||
|[contracts/utils/vendor/Base64.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/vendor/Base64.sol) [🖥](#nowhere "Uses Assembly")|[42](#nowhere "(nSLOC:42, SLOC:42, Lines:64)")|[0.00%](#nowhere "(Hit:0 / Total:6)")||
|[contracts/utils/LibSafeCast.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/LibSafeCast.sol)|[48](#nowhere "(nSLOC:48, SLOC:48, Lines:57)")|[0.00%](#nowhere "(Hit:0 / Total:19)")||
|[contracts/utils/vendor/Strings.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/utils/vendor/Strings.sol)|[61](#nowhere "(nSLOC:57, SLOC:61, Lines:84)")|[0.00%](#nowhere "(Hit:0 / Total:35)")||
|_Interfaces (23)_|
|[contracts/renderers/IParty1_1.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/IParty1_1.sol)|[4](#nowhere "(nSLOC:4, SLOC:4, Lines:6)")|-||
|[contracts/distribution/ITokenDistributorParty.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/distribution/ITokenDistributorParty.sol)|[5](#nowhere "(nSLOC:5, SLOC:5, Lines:16)")|-||
|[contracts/proposals/vendor/IOpenseaConduitController.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/vendor/IOpenseaConduitController.sol)|[5](#nowhere "(nSLOC:5, SLOC:5, Lines:8)")|-||
|[contracts/renderers/IERC721Renderer.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/IERC721Renderer.sol)|[5](#nowhere "(nSLOC:5, SLOC:5, Lines:8)")|-||
|[contracts/gatekeepers/IGateKeeper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/gatekeepers/IGateKeeper.sol)|[8](#nowhere "(nSLOC:4, SLOC:8, Lines:16)")|-||
|[contracts/operators/IOperator.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/operators/IOperator.sol) [💰](#nowhere "Payable Functions")|[9](#nowhere "(nSLOC:4, SLOC:9, Lines:20)")|-||
|[contracts/renderers/fonts/IFont.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/fonts/IFont.sol)|[9](#nowhere "(nSLOC:9, SLOC:9, Lines:34)")|-||
|[contracts/tokens/IERC721Receiver.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/tokens/IERC721Receiver.sol)|[9](#nowhere "(nSLOC:4, SLOC:9, Lines:11)")|-||
|[contracts/tokens/IERC20.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/tokens/IERC20.sol)|[10](#nowhere "(nSLOC:10, SLOC:10, Lines:18)")|-||
|[contracts/market-wrapper/IMarketWrapper.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/market-wrapper/IMarketWrapper.sol)|[13](#nowhere "(nSLOC:9, SLOC:13, Lines:86)")|-||
|[contracts/renderers/IMetadataRegistry1_1.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/renderers/IMetadataRegistry1_1.sol)|[13](#nowhere "(nSLOC:4, SLOC:13, Lines:16)")|-||
|[contracts/proposals/IProposalExecutionEngine.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/IProposalExecutionEngine.sol)|[18](#nowhere "(nSLOC:16, SLOC:18, Lines:39)")|-||
|[contracts/globals/IGlobals.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/globals/IGlobals.sol)|[20](#nowhere "(nSLOC:20, SLOC:20, Lines:40)")|-||
|[contracts/party/IPartyFactory.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/party/IPartyFactory.sol)|[21](#nowhere "(nSLOC:14, SLOC:21, Lines:39)")|-||
|[contracts/tokens/IERC721.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/tokens/IERC721.sol)|[22](#nowhere "(nSLOC:17, SLOC:22, Lines:36)")|-||
|[contracts/vendor/markets/IFoundationMarket.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/vendor/markets/IFoundationMarket.sol) [💰](#nowhere "Payable Functions")|[26](#nowhere "(nSLOC:19, SLOC:26, Lines:34)")|-||
|[contracts/proposals/vendor/FractionalV1.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/vendor/FractionalV1.sol) [💰](#nowhere "Payable Functions")|[30](#nowhere "(nSLOC:22, SLOC:30, Lines:51)")|-||
|[contracts/vendor/markets/INounsAuctionHouse.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/vendor/markets/INounsAuctionHouse.sol) [💰](#nowhere "Payable Functions")|[35](#nowhere "(nSLOC:32, SLOC:35, Lines:80)")|-||
|[contracts/tokens/IERC1155.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/tokens/IERC1155.sol)|[39](#nowhere "(nSLOC:24, SLOC:39, Lines:48)")|-||
|[contracts/vendor/markets/IKoansAuctionHouse.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/vendor/markets/IKoansAuctionHouse.sol) [💰](#nowhere "Payable Functions")|[40](#nowhere "(nSLOC:37, SLOC:40, Lines:74)")|-||
|[contracts/vendor/markets/IZoraAuctionHouse.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/vendor/markets/IZoraAuctionHouse.sol) [💰](#nowhere "Payable Functions")|[45](#nowhere "(nSLOC:37, SLOC:45, Lines:71)")|-||
|[contracts/distribution/ITokenDistributor.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/distribution/ITokenDistributor.sol) [💰](#nowhere "Payable Functions")|[76](#nowhere "(nSLOC:44, SLOC:76, Lines:156)")|-||
|[contracts/proposals/vendor/IOpenseaExchange.sol](https://github.com/code-423n4/2023-05-party/blob/main/contracts/proposals/vendor/IOpenseaExchange.sol) [💰](#nowhere "Payable Functions")|[134](#nowhere "(nSLOC:121, SLOC:134, Lines:154)")|-||
|Total (over 84 files):| [7029](#nowhere "(nSLOC:6298, SLOC:7029, Lines:10483)") |[74.34%](#nowhere "Hit:1327 / Total:1785")|

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

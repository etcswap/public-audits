# Security Vulnerability Assessment - Q2 2022

# Disclaimer

A Security and Vulnerability Assessment is considered a review of a point in time for an information system. The findings in this document reflect the information gathered and reviewed during the assessment period and not any future changes made outside of the date and time of this report. 

This assessment was prioritized according the known vulnerabilities, best practices, and most likely attacks resulting in a loss of funds and incorrect accounting error. Regularly occurring assessments and official contract code review is recommended to ensure best practices are being followed and that no new vulnerabilities have been introduced to the system over time. 

# ETC Swap

[ETC Swap](https://etcswap.org) is an information system that allows the trading and swapping of one token to another using the Ethereum Classic mainnet chain. The ETC Swap contracts are based off the [Uniswap-V2](https://uniswap.org) Core and Periphery open-source contracts with a customized front end for user interaction.

ETC Swap also utilizes the “Wrapped ETC” (WETC) token to help facilitate token transfer and transfer of assets. The Canonical [Wrapped ETC](https://wrappedether.org) project is based off best practices observed in the Ethereum Foundation mainnet chain decentralized finance (DeFi) technology stack and the Canonical [Wrapped ETH](https://weth.io) project's open-source contracts.

# Summary

In Q2 of the year 2022 the ETC Swap internal Security and Development teams performed an internal “first pass” security assessment and review of the ETC Swap information system. This work was intended to review all changes, all core and periphery contracts, and any additional work required in order to launch an initial version of ETC Swap in a secure manner that adheres to best practices and mitigates known risks to the system. 

The Security Assessment encompasses known issues to the Uniswap-V2 core and periphery contracts, the front end ETC Swap website, and any other items identified during the internal configuration and testing of the ETC Swap information system.

As a result of the Assessment, the following findings were found: 

- All security vulnerabilities from ConsenSys Uniswap-V1 audit were remediated and re-checked in V2
- No security vulnerabilities identified in Uniswap-V2 contracts
- ETC Swap contracts from Uniswap-V2 were peer reviewed for validity, accuracy, and identification of any new bugs introduced due to naming or code changes. No findings were found.

# Assessment Goals

The goals for this Security Assessment is to:

- Ensure known risks and issues to Uniswap-V1 and V2 are not present in ETC Swap
- Identify potential vulnerabilities and exploits
- Scope risk of found vulnerabilities and exploits
- Detail findings and remediation suggestions in this report

# Assessment Scope

The scope of this Security Assessment pertains to:

- ETC Swap Landing website ([https://etcswap.org](https://etcswap.org))
- ETC Swap App website ([https://swap.ethereumclassic.com](https://swap.ethereumclassic.com))
- ETC Swap Core Contracts ([forked from Uniswap-V2 v1.0.1](https://blockscout.com/etc/mainnet/address/0x0307cd3D7DA98A29e6Ed0D2137be386Ec1e4Bc9C/contracts))
- ETC Swap Periphery Contracts ([forked from Uniswap-V2 v1.0.1](https://blockscout.com/etc/mainnet/address/0x79Bf07555C34e68C4Ae93642d1007D7f908d60F5/contracts))

# Commit Hashes and Versions

ETC Swap was forked off the Uniswap-V2 Core and Periphery contracts from commit hash`4dd59067c76dea4a0e8e4bfdda41877a6b16dedc v1.0.1`from May 18, 2020 - https://github.com/Uniswap/v2-core/commit/4dd59067c76dea4a0e8e4bfdda41877a6b16dedc

The Solidity compiler version used during deployment is 0.6.6 in accordance with the hard-coded requirement for SafeMath libraries. 

# Prior Audits

## dApp - Uniswap-V2 Audit

[https://dapp.org.uk/reports/uniswapv2.html](https://dapp.org.uk/reports/uniswapv2.html) - Uniswap-V2 Audit Report for `commit 8160750` - https://github.com/Uniswap/v2-core/commit/816075049f811f1b061bca81d5d040b96f4c07eb

### Findings
| Recommendation |	Type |	Severity |	Likelihood |	Accepted |	Commit |
| --------------------- | ------- | ---------- | -------------- | ------------- | -------------- |
| Router: incompatible with token with fees on transfer | Bug | Medium | High | Yes | - | 
| Pair: fix to liquidity deflation introduces race condition | Bug | Medium | Medium | Yes | uniswap-v2-core@cbe801b | 
| Math: integer overflow in sqrt	| Bug	| Low	| Low	| Yes	| uniswap-v2-core@d1c8612 | 
| ERC20: make name, decimals, symbol constant | Improvement	| - | -	| Yes	| uniswap-v2-core@cbe801b | 
| ERC20: remove forfeit	| Improvement	| - | - | Yes	| uniswap-v2-core@cbe801b | 
| Factory: use .creationCode when retrieving Pair | bytecode | Improvement	| - | - | Yes	| uniswap-v2-core@f2d4021 | 
| Pair: replace block height with timestamp | Improvement	| - | - | Yes	| uniswap-v2-core@a55aa4b |
| Factory: replace allPairs array with a counter | Improvement	| - | - | No | - |
| Meta: replace math libraries with an inherited contract | Improvement | - | - | No | - |	 
| Pair: divide by zero in burn | Improvement	| - | - | No | - |	 


## ConsenSys - Uniswap-V1 Audit

Blog Post: [https://consensys.net/diligence/blog/2019/04/uniswap-audit/](https://consensys.net/diligence/blog/2019/04/uniswap-audit/)

Repo: https://github.com/ConsenSys/Uniswap-audit-report-2018-12

### Findings
| Chapter | Issue Title  | Initial Report Status | ETCSwap Status | Severity |
| ------------- | ------------- | ------------- | ------------- | ------------- | 
 | 2.1 | [Liquidity pool can be stolen in some tokens (e.g. ERC-777)](#31-liquidity-pool-can-be-stolen-in-some-tokens-(e-g--erc-777)) | Open | Closed w/ Uni-V2 | **Major** | 
 | 2.2 | [Frontrunners can skim ~2.5% from every transaction.](#32-frontrunners-can-skim-~2-5%-from-every-transaction-) | Open | Closed w/ Uni-V2 | **Medium** | 
 | 2.3 | [Gaps in test coverage](#33-gaps-in-test-coverage) | Open | Closed w/ Uni-V2 | **Minor** | 
 | 2.4 | [Consider using transferFrom() in removeLiquidity() function](#34-consider-using-transferfrom()-in-removeliquidity()-function) | Open | Closed w/ Uni-V2 | **Minor** | 
 | 2.5 | [Different 'deadline' behaviour](#35-different-deadline-behaviour) | Open | Closed w/ Uni-V2 | **Minor** | 
 | 2.6 | [Redundant checks in factory contract](#36-redundant-checks-in-factory-contract) | Open | Closed w/ Uni-V2 | **Minor** | 
 | 2.7 | [The factory contract should use a constructor](#37-the-factory-contract-should-use-a-constructor) | Open | Closed w/ Uni-V2 | **Minor** |

# Threat Model

Threat modeling is a process by which potential threats, such as structural vulnerabilities or the absence of appropriate safeguards, can be identified, enumerated, and mitigation can be prioritized. The purpose of threat modeling is to provide defenders with a systematic analysis of what controls or defenses need to be included, given the nature of the system, the probable attacker's profile, the most likely attack vectors, and the assets most desired by an attacker. Threat modeling answers questions like:

- “*Where am I most vulnerable to attack?”*
- ”*What are the most relevant threats?”*
- “W*hat do I need to do to safeguard against these threats?*”

The ETC Swap Security team feels the following threat models fit the most practical use cases for an attacker. 

1. An attacker (Alice) wants to withdraw liquidity fraudulently without having provided a token pair to the pool. Alice makes direct contract calls bypassing the website in an attempt to gain free tokens from the Liquidity Pool.
2. An attacker (Bob) wants to provide a faked version of a known token into the Liquidity Pool in an attempt to withdraw the legitimate token through the ETCSwap token swap UI.
3. An attacker (Charlie) tries to manipulate the price of a token in order to perform a “flash crash” so that he may take advantage of arbitrage opportunities. 
4. An attacker (Darla) has lost money due to impermanent loss. She decides to perform a distributed denial of service (DDoS) on the ETCSwap token swap site in order to prevent other users from utilizing the service. 

# Findings

## Router01 versus Router02

| Status | Closed - Not Vulnerable |
| --- | --- |
| Severity | High |
| Description | Per the Uniswap-V2 documentation (https://docs.uniswap.org/protocol/V2/reference/smart-contracts/router-01), Router 01 should not be used due to a “low severity bug and the fact that some methods do not work with tokens that take fees on transfer”. It has been confirmed that the ETCSwap token swap site is using UniswapV2Router02.sol as opposed to UniswapV2Router01.sol. |

## UNKNOWN Token Bug

| Status | Closed - Not Able to Reproduce |
| --- | --- |
| Severity | Medium |
| Description | There is a bug in Uniswap-V2 that can affect token listings making them appear “UNKNOWN” per https://hackernoon.com/the-unknown-bug-on-uniswap-continues-to-plague-the-platform-4e3534el. At this time the bug has not been observed on ETCSwap. While that does not indicate that bug does not exist, further testing is required in order to validate the existence and risk of this issue. |

## Safe Math Operations

| Status | Closed - Not Vulnerable |
| --- | --- |
| Severity | High |
| Description | We are using SafeMath libraries to perform integer declarations. This can be viewed throughout code in the various contracts - https://github.com/etcswap/v2-core/blob/4dd59067c76dea4a0e8e4bfdda41877a6b16dedc/contracts/UniswapV2Pair.sol#L12 |

## SafeMath Solidity Version

| Status | Closed - Not Vulnerable |
| --- | --- |
| Severity | Low |
| Description | Per Github Issue #57 of the v2-periphery contracts (https://github.com/Uniswap/v2-periphery/issues/57)  there is hard dependence on Solidity 0.6.6 for the SafeMath operations. ETCSwap used version 0.6.6 when deploying the contracts in order to take advantage of and to ensure that the SafeMath library and operations would be utilized correctly. This is defined in the Periphery contracts in line 1 of https://github.com/Uniswap/v2-periphery/blob/2efa12e0f2d808d9b49737927f0e416fafa5af68/contracts/libraries/SafeMath.sol#L1 |

## Periphery - **possible division by 0 in UniswapV2Library.sol**

| Status | Closed - Not Vulnerable |
| --- | --- |
| Severity | Medium |
| Description | There is mention of a possibility that a divide by 0 operation can occur in the UniswapV2Library.sol file. Per Github Issue #35 in the v2-periphery contracts the denominatar may be 0 causing one to pull the entire amount of one reserve out of a Uniswap market. Per the documentation on https://ethereum.org/ig/developers/tutorials/uniswap-v2-annotated-code/ - “To avoid cases of division by zero, there is a minimum number of liquidity tokens that always exist (but are owned by account zero). That number is MINIMUM_LIQUIDITY, a thousand.” - `uint public constant MINIMUM_LIQUIDITY = 10**3;` |

## Periphery - **TransferHelper: TRANSFER_FROM_FAILED**

| Status | Closed - Not a Uniswap V2/ETC Swap Issue |
| --- | --- |
| Severity | Medium |
| Description | Per Github Issue #96 of the v2-periphiery contracts (https://github.com/Uniswap/v2-periphery/issues/96), there can be an issue estimating gas fees causing strange behaviors and transaction processing issues under certain contract call conditions. After reviewing the issue it was determined that this is a larger issue with ERC-20 and contracts calling transactions for another address. ”The error happens because it is not possible to approve a permission for the UNISWAP_V2_ROUTER to spend tokens from the ContractSwapOnUniswap address, why by default the ERC20 approve function will not recognize ContractSwapOnUniswap as msg.sender. That is, the TransferHelper: TRANSFER_FROM_FAILED error occurs because the UNISWAP_V2_ROUTER does not have the necessary permission in the token contract to spend balance of ContractSwapOnUniswap” |

## **ERC-20 Reentrancy**

| Status | Closed - Not Vulnerable |
| --- | --- |
| Severity | High |
| Description | Per the Ethereum.org Uniswap-V2 Contract Walk Through (https://ethereum.org/en/developers/tutorials/uniswap-v2-annotated-code/), the security vulnerabilities based on reentrancy abuse are mitigated via a locking mutex in Uniswap-V2 as seen in https://github.com/etcswap/v2-core/blob/master/contracts/UniswapV2Pair.sol#L30 |

## **ERC-777 Reentrancy**

| Status | Closed - Not Vulnerable |
| --- | --- |
| Severity | High |
| Description | This issue has been identified as a vulnerability with external contracts and ERC777 tokens with Uniswap-V1 and V2. This issue was highlighted in the Consensys Audit of Uniswap-V1. In Uniswap-V2, ERC777 support was added with a mutex to ensure locking preventing contract reentrancy. The Uniswap-V2 Whitepaper at https://uniswap.org/whitepaper.pdf states that “To fully support such tokens, Uniswap-V2 includes a “lock” that directly prevents reentrancy to all public state changing functions. This also protects against reentrancy from the user-specified callback in a flash swap” |

## **ETC Swap Site - Denial Of Service**

| Status | Open - Fix In Progress |
| --- | --- |
| Severity | High |
| Description | The ETC Swap interface currently does not have any front end content delivery network and denial of service protection. It is recommended to use a CloudFlare and/or CloudFront distribution in order to absolve and mitigate any network traffic based denial of service attacks. This will ensure a high uptime for trading and liquidity activity. |

# Appendix A: References

| Name | Link |
| ------- | ------ |
| Uniswap Whitepaper | [https://uniswap.org/whitepaper.pdf](https://uniswap.org/whitepaper.pdf) |
| Uniswap-V2 Contract Walkthrough | [https://ethereum.org/en/developers/tutorials/uniswap-v2-annotated-code/](https://ethereum.org/en/developers/tutorials/uniswap-v2-annotated-code/) |
| Uniswap-V2 Core Contract Repo | https://github.com/Uniswap/v2-core |
| Uniswap-V2 Core Contract Issues | [https://github.com/Uniswap/v2-core/issues](https://github.com/Uniswap/v2-core/issues) |
| Uniswap-V2 Periphery Contract Repo | https://github.com/Uniswap/v2-periphery |
| Uniswap-V2 Periphery Contract Issues | [https://github.com/Uniswap/v2-periphery/issues](https://github.com/Uniswap/v2-periphery/issues) |
| Uniswap-V2 Docs - Security Guidance | [https://docs.uniswap.org/protocol/V2/concepts/advanced-topics/security](https://docs.uniswap.org/protocol/V2/concepts/advanced-topics/security) |
| Uniswap-V2 Documentation | [https://docs.uniswap.org/protocol/V2/reference/smart-contract/router-01](https://docs.uniswap.org/protocol/V2/reference/smart-contracts/router-01) |
| How to resolve TransferHelper error: TRANSFER_FROM_FAILED | [https://medium.com/@italo.honorato/how-to-resolve-transferhelper-error-transfer-from-failed-fb4c8bf6488c](https://medium.com/@italo.honorato/how-to-resolve-transferhelper-error-transfer-from-failed-fb4c8bf6488c) |
| ETCSwap-V2 Core Contracts Repo | https://github.com/etcswap/v2-core |
| ETCSwap-V2 Periphery Contracts Repo | https://github.com/etcswap/v2-periphery |
| ETCSwap Token Swap | https://etcswap.org |
| HackPedia: 16 Solidity Hacks/Vulnerabilities, their Fixes and Real World Examples | [https://medium.com/hackernoon/hackpedia-16-solidity-hacks-vulnerabilities-their-fixes-and-real-world-examples-f3210eba5148](https://medium.com/hackernoon/hackpedia-16-solidity-hacks-vulnerabilities-their-fixes-and-real-world-examples-f3210eba5148) |

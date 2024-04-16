# An Architectural Review of The Uniswap SwapRouter Contract

## Overview of Uniswap
Before a reviewing the SwapRouter Contract, I consider it orderly to do a review of parent parent protocol just to aid not so techy individuals an entrypoin to understanding this review. Uniswap stands as a decentralized exchange (DEX) protocol which is pivotal in the cyrpto and blockchain ecosystem. It is built on the Ethereum blockchain, designed to enable peer-to-peer trading of ERC-20 tokens without the need for intermediaries or third-parties. It's system architechure is built around three main components:

**1. Protocol:** This component comprises non-upgradable smart contracts that establish an automated market maker(AMM). The protocol layer facilitates peer-to-peer market making and swapping of ERC-20 tokens on the Ethereum blockchain.

**2. Interface:** This is the web interface layer (GUI) that offers a intiutive user-friendly way for users to engage with the protocol. This abstracts lots of technicallity way making it easy for both old and new entrants to effectivitely use the protocol's services.

**3. Governance:** Being a decentralised platform, this layer allows the Uniswap community to influence the protocol's future through a governance mechanism, enabled by the UNI token, which represents voting power within the Uniswap ecosystem.

This is the much I will cover in this overview of one of the DEXs that have shaped the ecosystmen and would proceed to focus this review on the subject of review-e SwapRouter contract. In this review I would attempt to do a proper and indepth, detailing of the SwapRouter contracts structure, functionaities, and the dependencies that enable its operation.

## SwapRouter

**What is the SwapRouter?**
Uniswap protocol is divided into `core` and `periphery` components to achieve modularity and enhance user experience. The core consists of factories, while the periphery comprises domain-specific contracts. Among these domain-specific contracts is the SwapRouter. As a periphery contract, the SwapRouter interacts with the core contracts, designed to manage swaps, including multi-hop swaps that involve more than two tokens in a single transaction.

## Contract Overview



### Contract Structure

**Name:** Uniswap V3 Swap Router
**Purpose:** Facilitates stateless execution of swaps
**License:** GPL-2.0

#### Pragmas:
The `SwapRouter` employs two pragmas:
1. Solidity version: 0.7.6
2. ABIEncoderV2: Enables passing structs as function arguments

#### Imports:
1. `SafeCast.sol`, `TickMath.sol`, `IUniswapV3Pool.sol` from Uniswap V3 core contracts.
2. `ISwapRouter.sol`: The swap router Interface.

#### Contract Inheritance

This contract inherits from about six other contracts, each with specific functions:
1. `ISwapRouter`: Implements functions defined in the `ISwapRouter` interface.
2. `PeripheryImmutableState`: Implements immutable state variables for the Uniswap V3 periphery contracts.
3. `PeripheryValidation`: Implements validation functions for Uniswap V3 periphery contracts.
4. `PeripheryPaymentsWithFee`: Provides functions for making payments with fees.
5. `Multicall`: Allows multiple calls to different contract functions in a single transaction.
6. `SelfPermit`: Implements a self-permitting mechanism for certain contract functions.

#### Libraries

1. `Path`: Contains functions for manipulating and decoding swap paths.
2. `PoolAddress`: Provides functions for computing Uniswap V3 pool addresses.
3. `CallbackValidation`: Implements validation for swap callback functions.

#### Events

The `SwapRouter` contract does not define any events.

#### Functions

The SwapRouter contains nine functions:
1. **Constructor**: Initializes the contract with the Uniswap V3 factory and WETH9 address.
2. **getPool**: A private function to retrieve the Uniswap V3 pool for a given token pair and fee.
3. **uniswapV3SwapCallback**: A callback function invoked by Uniswap V3 pool swaps.
4. **exactInputInternal**: Performs a single exact input swap.
5. **exactInputSingle**: Executes a single exact input swap with given parameters.
6. **exactInput**: Executes a series of exact input swaps with given parameters.
7. **exactOutputInternal**: Performs a single exact output swap.
8. **exactOutputSingle**: Executes a single exact output swap with given parameters.
9. **exactOutput**: Executes a series of exact output swaps with given parameters.

### Security

## Conclusion



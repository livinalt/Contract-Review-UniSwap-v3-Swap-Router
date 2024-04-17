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

The SwapRouter is a robut feature which has been implemented by the Uniswap protocol. It performs unique stateless transactions, meaning swap transactions can be executed reliably and deterministically without relying on external state.  To achieve a stateless transaction, it must receive all input params from user and must have its own internal logic though it may make use of libraries for certain operations. The SwapRouter contract inherits from the `ISwapRouter`, `PeripheryImmutableState`, `PeripheryValidation`, `PeripheryPaymentsWithFee`, `Multicall`, `SelfPermit` libraries and contracts allowing it to access functions and variables defined in these inherited contracts. 


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

1. `ISwapRouter`:

This appears to be a significant contract interface in the SwapRouter. It appears to do the major heavy lifting as its functions and elements are evident through out the contract.  This interface facilitate several swap operations to be performed in the SwapRouter contract. Its important to note that the ISwapRouter inherits the `IUniswapV3SwapCallback` contract and much more that the `ISwapRouter` contract contains four structs layouts (`ExactInputSingleParams`, `ExactInputParams`, `ExactOutputSingleParams`, `ExactOutputParams`) for various levels of swaps operations and their which are used in defining the params data in the implemented functions of the SwapROuter Contract.

2. `PeripheryImmutableState`: 

This is an abstract contract. It implements immutable state variables for the Uniswap V3 periphery contracts with only a contrructor whose parameters are the results from the two functions implemented by the the `IPeripheryImmutableState` with contains two functions WETH9 and factory which it inherits. 

3. `PeripheryValidation`: 

This contract inherited is another abstract contract and it relies on another contract `BlockTimeStamp` which returns the block timestamp required to implement validation functions.

4. `PeripheryPaymentsWithFee`: 

This inherited contract provides functions for making payments with fees, returning of balances held in accounts using the inherited contract `PeripheryPayments` performing deposit and withdraw functions inherited from the IERC20 contract

5. `Multicall`: 

This contract inheritance allows multiple calls to different functions of a contract in a single transaction thereby having multiple response. This creates for increase speed in transactions as opposed to the single call operations. This further inherits an Interface `IMultiCall`. It receives `bytes[] calldata data`as argument.

6. `SelfPermit`: 

This implements a self-permitting mechanism for certain contract functions. This means allowing the contract to spend tokens on behalf on the owner, Where the owner is the originator of the transaction. It inherits the `IERC20` `ISelfPermit` and `IERC20PermitAllowed` to implement this self-permiting feature. 

#### Libraries

1. `Path`: Contains functions for manipulating and decoding paths data for the multihop swap and returns the available pools in a path.

2. `PoolAddress`: This contract provides functions for computing Uniswap V3 pool addresses and the Poolkey. The pool key contains details with ordered tokens assignment.

3. `CallbackValidation`: Implements validation for swap callback functions by returning a valid V3 pool address. It inherits the `PoolAddress` and `IUniswapV3Pool` contracts for this.

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



---
id: creating-a-pool
title: Creating a Pool Instance
---

A "Pool" as we refer to it here does not mean an actual Convexus pool, but a model of one created with the SDK. This model will help us interact with the protocol, or manipulate data relevant to the protocol, in a way that does not require continually fetching pool data from the Convexus SCOREs - which can be time intensive and computationally costly.

## Importing the ABI

First you need to import the ConvexusPool ABI, either from the SDK repository or directly onchain using the `icx_getScoreApi` RPC call. It is also available [here](https://github.com/Convexus-Protocol/convexus-sdk-js/blob/master/packages/sdk/src/artifacts/contracts/ConvexusPool/ConvexusPool.json).

Depending on your local configuration, you may need to update your tsconfig.json to allow importing of `json` files with `"resolveJsonModule": true,`.

```typescript
import { Pool } from '@convexus/sdk'
import { Token } from '@convexus/sdk-core'
import IConvexusPool from 'artifacts/contracts/ConvexusPool/ConvexusPool.json'
```

Now we'll use the ICON Toolkit and update the `Contract` object with our imported ABI, alongside the pool address, provider, and network ID. Here we use the Berlin Network (nid 0x7).

```typescript
import IconService from 'icon-sdk-js'
import { Contract } from '@convexus/icon-toolkit';

const poolAddress = 'cx4f5661a3dfaafbc11d13d3ea80474870b37369ca'
const httpProvider = new IconService.HttpProvider('https://berlin.net.solidwallet.io/api/v3');
const iconService = new IconService(httpProvider);

const poolContract = new Contract(poolAddress, IConvexusPool, iconService, 7)
```

## Creating The Interfaces

Create two interfaces with types that are appropriate for the data we need. We won't be using all of this data, but some extra data is fetched for context.

```typescript
import { BigintIsh } from '@convexus/icon-toolkit';

interface Immutables {
  factory: string
  token0: string
  token1: string
  fee: number
  tickSpacing: number
  maxLiquidityPerTick: BigintIsh
}

interface State {
  liquidity: BigintIsh
  sqrtPriceX96: BigintIsh
  tick: number
  observationIndex: number
  observationCardinality: number
  observationCardinalityNext: number
  feeProtocol: number
  unlocked: boolean
}
```

## Fetching Immutable Data

Fetch the immutable data from the deployed pool contract and return it to create a model of the pool.

```typescript
async function getPoolImmutables(): Promise<Immutables> {
  const [factory, token0, token1, fee, tickSpacing, maxLiquidityPerTick] = await Promise.all([
    poolContract.factory(),
    poolContract.token0(),
    poolContract.token1(),
    poolContract.fee(),
    poolContract.tickSpacing(),
    poolContract.maxLiquidityPerTick(),
  ])

  const immutables = {
    factory,
    token0,
    token1,
    fee: parseInt(fee, 16),
    tickSpacing,
    maxLiquidityPerTick,
  }

  return immutables
}
```

Fetch the state data in with the same `Promise.all` style. This approach queries state data concurrently, rather than sequentially, to avoid out of sync data that may be returned if sequential queries are executed over the span of two blocks.

> `sqrtPriceX96` and `sqrtRatioX96`, despite being named differently, are interchangeable values.

## Fetching State Data

```typescript
async function getPoolState() {
  const [liquidity, slot] = await Promise.all([poolContract.liquidity(), poolContract.slot0()])

  const PoolState: State = {
    liquidity,
    sqrtPriceX96: slot['sqrtPriceX96'],
    tick: parseInt(slot['tick'], 16),
    observationIndex: slot['observationIndex'],
    observationCardinality: slot['observationCardinality'],
    observationCardinalityNext: slot['observationCardinalityNext'],
    feeProtocol: slot['feeProtocol'],
    unlocked: slot['unlocked'],
  }

  return PoolState
}
```

## Creating the Pool Instance

Create a function called `main`, which calls previously written functions, and uses the returned data to construct two `Token` instances and a SDK `Pool` instance.

> The final constructor argument when creating a Pool, `ticks`, is optional. `ticks` takes all tick data, including the liquidity within, which can be used to model the result of a swap. Because this can add up to a lot of data fetched from the SCORE, it is optional and may be left out when not needed. In this example, we have left it out.

```typescript
async function main() {
  const [immutables, state] = await Promise.all([getPoolImmutables(), getPoolState()])

  const TokenA = new Token(immutables.token0, 6, 'USDC', 'USD Coin')
  const TokenB = new Token(immutables.token1, 18, 'CXS', 'Convexus Token')

  const poolExample = new Pool(
    TokenA,
    TokenB,
    immutables.fee,
    state.sqrtPriceX96.toString(),
    state.liquidity.toString(),
    state.tick
  )
  console.log("poolExample=", poolExample)
}

main()
```

If everything is working, the script should return something like this:

```typescript
Pool {
  "token0": Token {
    "decimals": 6,
    "symbol": "USDC",
    "name": "USD Coin",
    "isNative": false,
    "isToken": true,
    "address": "cx7a5be2907bc1f4d6fae0462df08e7a21972d7347"
  },
  "token1": Token {
    "decimals": 18,
    "symbol": "CXS",
    "name": "Convexus Token",
    "isNative": false,
    "isToken": true,
    "address": "cxd57fe1c5e63385f412b814634102609e8a987e3a"
  },
  "fee": 500,
  "sqrtRatioX96": JSBI(4) [
    0,
    0,
    0,
    64
  ],
  "liquidity": JSBI(3) [
    1027604588,
    776826414,
    1734
  ],
  "tickCurrent": 0,
  "tickDataProvider": NoTickDataProvider {},
  "poolFactoryProvider": NoPoolFactoryProvider {}
}
```

## The Final Script

```typescript
import React from 'react';
import IconService from 'icon-sdk-js'
import { Contract, BigintIsh } from '@convexus/icon-toolkit';
import { Token } from '@convexus/sdk-core';
import { Pool } from '@convexus/sdk';
import IConvexusPool from './artifacts/contracts/ConvexusPool/ConvexusPool.json'

const poolAddress = 'cx4f5661a3dfaafbc11d13d3ea80474870b37369ca'
const httpProvider = new IconService.HttpProvider('https://berlin.net.solidwallet.io/api/v3');
const iconService = new IconService(httpProvider);
const poolContract = new Contract(poolAddress, IConvexusPool, iconService, 7)

interface Immutables {
  factory: string
  token0: string
  token1: string
  fee: number
  tickSpacing: number
  maxLiquidityPerTick: BigintIsh
}

interface State {
  liquidity: BigintIsh
  sqrtPriceX96: BigintIsh
  tick: number
  observationIndex: number
  observationCardinality: number
  observationCardinalityNext: number
  feeProtocol: number
  unlocked: boolean
}

async function getPoolImmutables(): Promise<Immutables> {
  const [factory, token0, token1, fee, tickSpacing, maxLiquidityPerTick] = await Promise.all([
    poolContract.factory(),
    poolContract.token0(),
    poolContract.token1(),
    poolContract.fee(),
    poolContract.tickSpacing(),
    poolContract.maxLiquidityPerTick(),
  ])

  const immutables = {
    factory,
    token0,
    token1,
    fee: parseInt(fee, 16),
    tickSpacing,
    maxLiquidityPerTick,
  }

  return immutables
}

async function getPoolState() {
  const [liquidity, slot] = await Promise.all([poolContract.liquidity(), poolContract.slot0()])

  const PoolState: State = {
    liquidity,
    sqrtPriceX96: slot['sqrtPriceX96'],
    tick: parseInt(slot['tick'], 16),
    observationIndex: slot['observationIndex'],
    observationCardinality: slot['observationCardinality'],
    observationCardinalityNext: slot['observationCardinalityNext'],
    feeProtocol: slot['feeProtocol'],
    unlocked: slot['unlocked'],
  }

  return PoolState
}

async function main() {
  const [immutables, state] = await Promise.all([getPoolImmutables(), getPoolState()])

  const TokenA = new Token(immutables.token0, 6, 'USDC', 'USD Coin')
  const TokenB = new Token(immutables.token1, 18, 'CXS', 'Convexus Token')

  const poolExample = new Pool(
    TokenA,
    TokenB,
    immutables.fee,
    state.sqrtPriceX96.toString(),
    state.liquidity.toString(),
    state.tick
  )
  console.log("poolExample=", poolExample)
}

main()
```

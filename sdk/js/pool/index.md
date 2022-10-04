---
id: creating-a-pool
title: Creating a Pool Instance
---

A "Pool" as we refer to it here does not mean an actual Convexus pool, but a model of one created with the SDK. This model will help us interact with the protocol, or manipulate data relevant to the protocol, in a way that does not require continually fetching pool data from the Convexus SCOREs - which can be time intensive and computationally costly.

## Importing the ABI & instantiate a Contract

First you need to import the ConvexusPool ABI, either from the SDK repository or directly onchain using the `Contract::getAbi` method of the `icx_getScoreApi` RPC call. The ABI is also available [here](https://github.com/Convexus-Protocol/convexus-sdk-js/blob/master/packages/sdk/src/artifacts/contracts/ConvexusPool/ConvexusPool.json).

```typescript
import { Pool } from '@convexus/sdk'
```

Now we'll use the ICON Toolkit and update the `Contract` object with our imported ABI, alongside the pool address, provider, and network ID. Here we use the Berlin Network (nid 0x7).

```typescript
import { Contract } from '@convexus/icon-toolkit';
import IconService from 'icon-sdk-js'

// Create an ICON service object
const httpProvider = new IconService.HttpProvider('https://berlin.net.solidwallet.io/api/v3')
const iconService = new IconService(httpProvider)

// Create a Pool Contract instance
const poolAddress = 'cxcc48f601776b5998625017c7d220d09a150aa2ad'
const poolAbi = await Contract.getAbi(iconService, poolAddress)
const poolContract = new Contract(poolAddress, poolAbi, iconService, iconService, 7)
```

## Creating the Pool Instance

In order to create a SDK `Pool` instance, we may use the `Pool::fromContract` helper method, which reads the Pool contract state and initialize a `Pool` object based on these values.

```typescript
const pool = Pool.fromContract(poolContract)
console.log(pool)
```

The script should output the following object:

```typescript
Pool {
  token0: Token {
    decimals: 18,
    symbol: 'bnUSD',
    name: 'bnUSD',
    isNative: false,
    isToken: true,
    address: 'cx196276887ec398a1fb41b335f9260fbb300c684b'
  },
  token1: Token {
    decimals: 6,
    symbol: 'USDC',
    name: 'USDC',
    isNative: false,
    isToken: true,
    address: 'cxa818e190782c7bc64c1ec12512c7f8f3171fc8cf'
  },
  fee: 500,
  sqrtRatioX96: JSBI(3) [ 218103808, 511891382, 68719, sign: false ],
  liquidity: JSBI(2) [ 674318285, 31788869, sign: false ],
  tickCurrent: -276325,
  tickDataProvider: NoTickDataProvider {},
  poolFactoryProvider: NoPoolFactoryProvider {}
}
```

## The Final Script

```typescript
import { Pool } from '@convexus/sdk'
import { Contract } from '@convexus/icon-toolkit';
import IconService from 'icon-sdk-js'

// Create an ICON service object
const httpProvider = new IconService.HttpProvider('https://berlin.net.solidwallet.io/api/v3');
const iconService = new IconService(httpProvider);
const berlinNid = 7;

// Create a Pool Contract instance
const poolAddress = 'cxcc48f601776b5998625017c7d220d09a150aa2ad'
const poolAbi = await Contract.getAbi(iconService, poolAddress)
const poolContract = new Contract(poolAddress, poolAbi, iconService, iconService, berlinNid);

const pool = await Pool.fromContract(poolContract)
console.log(pool)
```

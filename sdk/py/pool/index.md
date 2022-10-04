---
id: creating-a-pool
title: Creating a Pool Instance
---

A "Pool" as we refer to it here does not mean an actual Convexus pool, but a model of one created with the SDK. This model will help us interact with the protocol, or manipulate data relevant to the protocol, in a way that does not require continually fetching pool data from the Convexus SCOREs - which can be time intensive and computationally costly.

## Importing the ABI & instantiate a Contract

First you need to import the ConvexusPool ABI, either from the SDK repository or directly onchain using the `Contract::getAbi` method of the `icx_getScoreApi` RPC call. The ABI is also available [here](https://github.com/Convexus-Protocol/convexus-sdk-js/blob/master/packages/sdk/src/artifacts/contracts/ConvexusPool/ConvexusPool.json).

```python
from convexus.sdk import Pool
```

Now we'll use the ICON Toolkit and update the `Contract` object with our imported ABI, alongside the pool address, provider, and network ID. Here we use the Berlin Network (nid 0x7).

```python
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider
from convexus.icontoolkit import Contract

# Create an ICON service object
httpProvider = HTTPProvider("https://berlin.net.solidwallet.io", 3)
iconService = IconService(httpProvider)

# Create a Pool Contract instance
poolAddress = "cxcc48f601776b5998625017c7d220d09a150aa2ad"
poolAbi = Contract.getAbi(iconService, poolAddress)
poolContract = Contract(poolAddress, poolAbi, iconService, iconService, 7)
```

## Creating the Pool Instance

In order to create a SDK `Pool` instance, we may use the `Pool::fromContract` helper method, which reads the Pool contract state and initialize a `Pool` object based on these values.

`Pool.fromContract` is an asynchronous call, so it needs to be called within an asyncio loop:

```python
import asyncio

async def main (poolContract):
  pool = await Pool.fromContract(poolContract)
  print(pool)

asyncio.run(main())
```



The script should output the following object:

```python
{
  "token0": {
    "decimals": 18, 
    "symbol": "bnUSD", 
    "name": "bnUSD", 
    "address": "cx196276887ec398a1fb41b335f9260fbb300c684b"
  }, 
  "token1": {
    "decimals": 6, 
    "symbol": "USDC", 
    "name": "USDC", 
    "address": "cxa818e190782c7bc64c1ec12512c7f8f3171fc8cf"
  }, 
  "fee": 500, 
  "sqrtRatioX96": 79228162514264334008320, 
  "liquidity": 34133038857275341, 
  "tickCurrent": -276325, 
  "tickDataProvider": <convexus.sdk.entities.tickDataProvider.NoTickDataProvider object at 0x00000172F8ED5180>,
  "poolFactoryProvider": <convexus.sdk.entities.factoryProvider.NoPoolFactoryProvider object at 0x00000172F8ED51B0>
  "_Pool__token0Price": None,
  "_Pool__token1Price": None
}
```

## The Final Script

```python
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider
from convexus.sdk import Pool
from convexus.icontoolkit import Contract
import asyncio

# Create an ICON service object
httpProvider = HTTPProvider("https://berlin.net.solidwallet.io", 3)
iconService = IconService(httpProvider)

# Create a Pool Contract instance
poolAddress = "cxcc48f601776b5998625017c7d220d09a150aa2ad"
poolAbi = Contract.getAbi(iconService, poolAddress)
poolContract = Contract(poolAddress, poolAbi, iconService, iconService, 7)

async def main ():
  pool = await Pool.fromContract(poolContract)
  print(pool)

asyncio.run(main())
```

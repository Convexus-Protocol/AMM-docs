# 📖 Introduction

Liquidity methods lets you handle your position within the Convexus Pools. Liquidity providers are rewarded with transaction fees depending of the size of their position and their liquidity range.

# 📜 Write Methods

## `mint`

- 📚 Description: 
  - Adds liquidity for the given `recipient`/`tickLower`/`tickUpper` position
  - The caller of this method receives a callback in the form of `convexusMintCallback` in which they must pay any `token0` or `token1` owed for the liquidity. 
  - The amount of `token0`/`token1` due depends on `tickLower`, `tickUpper`, the amount of liquidity, and the current price.
- 🔒 Access: 
  - Everyone
- 🔎 Event Logs emitted:
  -  [`Mint`](#mint)

### 🖊️ Signature

```java
@External
public PairAmounts mint (
    Address recipient,
    int tickLower,
    int tickUpper,
    BigInteger amount,
    byte[] data
)
```

- `recipient`: The address for which the liquidity will be created
- `tickLower`: The lower tick of the position in which to add liquidity
- `tickUpper`: The upper tick of the position in which to add liquidity
- `amount`: The amount of liquidity to mint
- `data`: Any data that should be passed through to the callback

### 🧪 Example call

```java
{
  "to": ConvexusPool,
  "method": "mint",
  "params": {
    "recipient": "hx1234567890123456789012345678901234567890",
    "tickLower": "-0xd89b4",
    "tickUpper": "0xd89b4",
    "amount": "0x10000",
    "data": ""
  },
}
```

# 🔎 Event Logs

## `Mint`

- 📚 Description: 
  - Emitted when liquidity is minted for a given position

### 🖊️ Signature

```java
@EventLog(indexed = 3)
protected void Mint (
  Address recipient, 
  int tickLower, 
  int tickUpper, 
  Address sender, 
  BigInteger amount,
  BigInteger amount0, 
  BigInteger amount1
)
```

- `sender`: The address that minted the liquidity
- `owner`: The owner of the position and recipient of any minted liquidity
- `tickLower`: The lower tick of the position
- `tickUpper`: The upper tick of the position
- `amount`: The amount of liquidity minted to the position range
- `amount0`: How much token0 was required for the minted liquidity
- `amount1`: How much token1 was required for the minted liquidity

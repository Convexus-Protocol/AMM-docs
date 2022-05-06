# 📖 Introduction

Once in possession of a Convexus position NFT, the liquidity provider may change his position, either increase or decrease their position liquidity.

# 📜 Write Methods

## `increaseLiquidity`

- 📚 Description: 
  - Increases the amount of liquidity in an existing position, with tokens paid by the `caller`
- 🔒 Access:
  - Everyone
- 🚩 Requirements:
  - The liquidity provider should have deposited the correct amount of tokens beforehand (see [`deposit`](create-position.md#deposit))
  - The `deadline` should be valid
- 🔎 Event Logs emitted:
  - [`IncreaseLiquidity`](create-position.md#increaseliquidity)

### 🖊️ Signature

```java
@External
public IncreaseLiquidityResult increaseLiquidity (
  IncreaseLiquidityParams params
)
```

- `params`: The params necessary to increase liquidity of an existing position, encoded as [`IncreaseLiquidityParams`](#increaseliquidityparams)

### 🧪 Example call

```java
{
  "to": NonFungiblePositionManager,
  "method": "increaseLiquidity",
  "params": {
    "params": {
      "tokenId": "0x123",
      "amount0Desired": "0x21e19e0c9bab2400000", // 10000 * 10**18
      "amount1Desired": "0x21e19e0c9bab2400000", // 10000 * 10**18
      "amount0Min": "0", // Shouldn't be 0 in production
      "amount1Min": "0", // Shouldn't be 0 in production
      "deadline": "0x61e92f6b"
    }
  }
}
```

## `decreaseLiquidity`

- 📚 Description: 
  - Decreases the amount of liquidity in a position and accounts it to the position
  - After decreasing the liquidity, the tokens may be retrieved by calling the [`collect`](collect-rewards.md#collect) method
- 🔒 Access:
  - Owner or approved address of the NFT position
- 🚩 Requirements:
  - The `deadline` should be valid
- 🔎 Event Logs emitted:
  - [`DecreaseLiquidity`](#decreaseliquidity-1)

### 🖊️ Signature

```java
@External
public PairAmounts decreaseLiquidity (
  DecreaseLiquidityParams params
)
```

- `params`: The params necessary to increase liquidity of an existing position, encoded as [`DecreaseLiquidityParams`](#decreaseliquidityparams)

### 🧪 Example call

```java
{
  "to": NonFungiblePositionManager,
  "method": "decreaseLiquidity",
  "params": {
    "params": {
      "tokenId": "0x123",
      "liquidity": "0x100000",
      "amount0Min": "0", // Shouldn't be 0 in production
      "amount1Min": "0", // Shouldn't be 0 in production
      "deadline": "0x61e92f6b"
    }
  }
}
```


## `burn`

- 📚 Description: 
  - Burns a token ID, which deletes it from the NFT contract
- 🔒 Access:
  - Owner or approved address of the NFT position
- 🚩 Requirements:
  - The token must have 0 liquidity and all tokens must be collected first

### 🖊️ Signature

```java
@External
public void burn (BigInteger tokenId)
```

- `tokenId`: The ID of the token that is being burned

### 🧪 Example call

```java
{
  "to": NonFungiblePositionManager,
  "method": "burn",
  "params": {
    "tokenId": "0x123",
  }
}
```

# ⚙️ Structures

## `IncreaseLiquidityParams`

```java
class IncreaseLiquidityParams {
  BigInteger tokenId;
  BigInteger amount0Desired;
  BigInteger amount1Desired;
  BigInteger amount0Min;
  BigInteger amount1Min;
  BigInteger deadline;
}
```

- `tokenId`: The ID of the token for which liquidity is being increased
- `amount0Desired`: The desired amount of token0 to be spent
- `amount1Desired`: The desired amount of token1 to be spent
- `amount0Min`: The minimum amount of token0 to spend, which serves as a slippage check
- `amount1Min`: The minimum amount of token1 to spend, which serves as a slippage check
- `deadline`: The time by which the transaction must be included to effect the change

## `DecreaseLiquidityParams`

```java
class DecreaseLiquidityParams {
  BigInteger tokenId;
  BigInteger liquidity;
  BigInteger amount0Min;
  BigInteger amount1Min;
  BigInteger deadline;
}
```

- `tokenId`: The ID of the token for which liquidity is being decreased
- `liquidity`: The amount by which liquidity will be decreased
- `amount0Min`: The minimum amount of token0 that should be accounted for the burned liquidity
- `amount1Min`: The minimum amount of token1 that should be accounted for the burned liquidity
- `deadline`: The time by which the transaction must be included to effect the change

# 🔎 Event Logs

## `DecreaseLiquidity`

- Emitted when liquidity is decreased for a position NFT

```java
@EventLog
public void DecreaseLiquidity (
  BigInteger tokenId, 
  BigInteger liquidity, 
  BigInteger amount0,
  BigInteger amount1
)
```

- `tokenId`: The ID of the token for which liquidity was increased
- `liquidity`: The amount by which liquidity for the NFT position was increased
- `amount0`: The amount of token0 that was paid for the increase in liquidity
- `amount1`: The amount of token1 that was paid for the increase in liquidity

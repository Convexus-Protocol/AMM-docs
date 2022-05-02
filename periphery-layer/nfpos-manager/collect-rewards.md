# 📖 Introduction

After creating an active position on a pool, the fees generated from swap transactions on this pool will be rewarded to the liquidity providers. For claiming their rewards, liquidity providers need to call the `collect` method from the `NonFungiblePositionManager` contract.

# 📜 Write Methods

## `collect`

- 📚 Description: 
  - Collects up to a maximum amount of fees owed to a specific position to the recipient. May also be used after decreasing the liquidity in order to get back the owed tokens.
- 🔒 Access: 
  - Everyone
- 🔎 Event Logs emitted:
  -  [`Collect`](#collect-1)

### 🖊️ Signature

```java
@External
public PairAmounts collect (CollectParams params)
```

- `params`: The params necessary to collect rewards, encoded as [`CollectParams`](#collectparams)

### 🧪 Example call

```java
{
  "to": NonFungiblePositionManager,
  "method": "collect",
  "params": {
    "params": {
      "tokenId": "0x123", // The ID of the NFT for which tokens are being collected
      "recipient": recipient,
      "amount0Max": "0x3635c9adc5dea00000", // 1000 * 10**18
      "amount1Max": "0x3635c9adc5dea00000" // 1000 * 10**18
    }
  }
}
```

# ⚙️ Structures

## `CollectParams`

```java
public class CollectParams {
  BigInteger tokenId;
  Address recipient;
  BigInteger amount0Max;
  BigInteger amount1Max;
}
```

- `tokenId`: The ID of the NFT for which tokens are being collected
- `recipient`: The account that should receive the tokens
- `amount0Max`: The maximum amount of token0 to collect
- `amount1Max`: The maximum amount of token1 to collect

# 🔎 Event Logs

## `Collect`

- Emitted when tokens are collected for a position NFT
- The amounts reported may not be exactly equivalent to the amounts transferred, due to rounding behavior

```java
@EventLog
public void Collect (
  BigInteger tokenId, 
  Address recipient, 
  BigInteger amount0Collect, 
  BigInteger amount1Collect
)
```

- `tokenId`: The ID of the token for which underlying tokens were collected
- `recipient`: The address of the account that received the collected tokens
- `amount0`: The amount of token0 owed to the position that was collected
- `amount1`: The amount of token1 owed to the position that was collected
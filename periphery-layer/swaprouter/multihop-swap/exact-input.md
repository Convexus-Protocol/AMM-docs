# 📖 Introduction

Exact input multi hop swaps will swap a fixed amount on a given input token for the maximum amount possible for a given output, and can include an arbitrary number of intermediary swaps.

# 📜 Write Methods

## `exactInput`

- 📚 Description: 
  - Swaps `amountIn` of one token for as much as possible of another along the specified path
- 🔒 Access: 
  - Everyone

### 🖊️ Signature

```java
// @External - this method is external through tokenFallback
private void exactInput (
    Address caller,
    Address tokenIn,
    BigInteger amountIn,
    ExactInputParams params
)
```

- `caller`: The method caller. This field is handled by tokenFallback
- `tokenIn`: The tokenIn address. This field is handled by tokenFallback
- `amountIn` The token amount sent. This field is handled by tokenFallback
- `params` The parameters necessary for the multi-hop swap, encoded as [`ExactInputParams`](#exactinputparams)

### 🧪 Example call

```java
{
  "to": token0, // equivalent to tokenIn
  "method": "transfer",
  "params": {
    "_to": SwapRouter,
    "_value": "0xde0b6b3a7640000", // 10**18 - equivalent to amountIn
    "_data": hex({
      "method": "exactInput",
      "params": {
        "path": hex([token0, 3000, token2, 3000, token1]) // Multiple pool swaps are encoded through bytes called a `path`. A path is a sequence of token addresses and poolFees that define the pools used in the swaps. The format for pool encoding is (tokenIn, fee, tokenOut/tokenIn, fee, tokenOut) where tokenIn/tokenOut parameter is the shared token across the pools. Since we are swapping token0 to token2 and then token2 to token1 the path encoding is (token0, 0.3%, token2, 0.3%, token1).
        "recipient": recipient,
        "deadline": "0x61e92f6b", // in seconds
        "amountOutMinimum": "0x0", // we are setting to zero, but this is a significant risk in production. For a real deployment, this value should be calculated using our SDK or an onchain price oracle - this helps protect against getting an unusually bad price for a trade due to a front running sandwich or another type of price manipulation
      }
    })
  },
}
```

# ⚙️ Structures

## `ExactInputParams`

```java
class ExactInputParams {
  byte[] path;
  Address recipient;
  BigInteger deadline;
  BigInteger amountOutMinimum;
}
```

- `path`: A sequence of [`tokenIn`, `fee`, `tokenOut`], which are the variables needed to compute each pool contract address in our sequence of swaps. The multihop swap router code will automatically find the correct pool with these variables, and execute the swap needed within each pool in our sequence. Please see the [Swap path documentation](/commons/swap-path.md#how-to-encode-a-swap-path) for more information about how to encode a path.
- `recipient`: The destination address of the outbound asset-
- `deadline`: The unix time after which a transaction will be reverted, to protect against long delays and the increased chance of large price swings therein
- `amountOutMinimum`: The maximum amount of token0 willing to be swapped for the specified amountOut of token1

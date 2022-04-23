# 📖 Introduction

An exact output swap will swap a variable amount of the input token for a fixed amount of the outbound token. This is the less common technique for multihop swaps. The code for swapping is largely the same except for one notable difference, the `Path` is encoded backwards, as an exact output swap is executed in reverse order to pass down the necessary variables for the chain of transactions.

# 📜 Methods

## `exactOutput`

- 📚 Description: 
  - Swaps as little as possible of one token for `amountOut` of another along the specified path (reversed)
- 🔒 Access: 
  - Everyone
  
### 🖊️ Signature

```java
// @External - this method is external through tokenFallback
private void exactOutput (
    Address caller, 
    Address tokenIn, 
    BigInteger amountInMaximum, 
    ExactOutputParams params
)
```

- `caller`: The method caller. This field is handled by tokenFallback
- `tokenIn`: The tokenIn address. This field is handled by tokenFallback
- `amountInMaximum`: The maximum amount of `token0` willing to be swapped for the specified amountOut of `token1`. This field is handled by tokenFallback.
- `params`: The parameters necessary for the swap, encoded as [`ExactOutputParams`](#exactoutputparams)

### 🧪 Example call

```java
{
  "to": token0, // equivalent to tokenIn
  "method": "transfer",
  "params": {
    "_to": SwapRouter,
    "_value": "0xde0b6b3a7640000", // 10**18 - equivalent to amountInMaximum
    "_data": hex({
      "method": "exactOutput",
      "params": {
        "path": hex([token1, 3000, token2, 3000, token0]) // The parameter path is encoded as (tokenOut, fee, tokenIn/tokenOut, fee, tokenIn). The tokenIn/tokenOut field is the shared token between the two pools used in the multiple pool swap. In this case token2 is the "shared" token. For an exactOutput swap, the first swap that occurs is the swap which returns the eventual desired token. In this case, our desired output token is token1 so that swap happens first, and is encoded in the path accordingly.
        "recipient": recipient,
        "deadline": "0x61e92f6b", // in seconds
        "amountOut": "0x1bc16d674ec80000", // 2 * 10**18
      }
    })
  },
}
```

# ⚙️ Structures

## `ExactOutputParams`

```java
class ExactOutputParams {
  byte[] path;
  Address recipient;
  BigInteger deadline;
  BigInteger amountOut;
}
```

- `path`: A sequence of [`tokenOut`, `fee`, `tokenIn`], which are the variables needed to compute each pool contract address in our sequence of swaps. For an exactOutput swap, the first swap that occurs is the swap which returns the eventual desired token. The multihop swap router code will automatically find the correct pool with these variables, and execute the swap needed within each pool in our sequence. Please see the [Swap path documentation](/commons/swap-path.md#how-to-encode-a-swap-path) for more information about how to encode a path.
- `recipient`: The destination address of the outbound asset
- `deadline`: The unix time after which a transaction will be reverted, to protect against long delays and the increased chance of large price swings therein
- `amountOut`: The desired amount of outbound token after the swap

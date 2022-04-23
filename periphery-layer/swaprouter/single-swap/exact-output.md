# 📖 Introduction

Exact Output swaps a minimum possible amount of the input token for a fixed amount of the outbound token. This is the less common swap style - but useful in a variety of circumstances.

Because this example transfers in the inbound asset in anticipation of the swap - its possible that some of the inbound token will be left over after the swap is executed. The Router will send them back automatically using an `excess` method name in the IRC2 `data` field.

# 📜 Write Methods

## `exactOutputSingle`

```java
// @External - this method is external through tokenFallback
private void exactOutputSingle (
    Address caller, 
    Address tokenIn, 
    BigInteger amountInMaximum, 
    ExactOutputSingleParams params
)
```

- Swaps as little as possible of one token for `amountOut` of another token
- Access: Everyone
- `caller`: The method caller. This field is handled by tokenFallback
- `tokenIn`: The tokenIn address. This field is handled by tokenFallback
- `amountInMaximum`: The maximum amount of `token0` willing to be swapped for the specified amountOut of `token1`. This field is handled by tokenFallback.
- `params`: The parameters necessary for the swap, encoded as [`ExactOutputSingleParams`](#exactoutputsingleparams)

### 🧪 Example call

```java
{
  "to": token0, // equivalent to tokenIn
  "method": "transfer",
  "params": {
    "_to": SwapRouter,
    "_value": "0xde0b6b3a7640000", // 10**18 - equivalent to amountInMaximum
    "_data": hex({
      "method": "exactOutputSingle",
      "params": {
        "tokenOut": token1,
        "fee": "0xbb8", // 3000, 0.3%
        "recipient": recipient,
        "deadline": "0x61e92f6b", // in seconds
        "amountOut": "0x1bc16d674ec80000", // 2 * 10**18
        "sqrtPriceLimitX96": "0x0" // We set this to zero - which makes this parameter inactive. In production, this value can be used to set the limit for the price the swap will push the pool to, which can help protect against price impact or for setting up logic in a variety of price-relevant mechanisms
      }
    })
  },
}
```

# ⚙️ Structures

## `ExactOutputSingleParams`

```java
class ExactOutputSingleParams {
  Address tokenOut;
  int fee;
  Address recipient;
  BigInteger deadline;
  BigInteger amountOut;
  BigInteger sqrtPriceLimitX96;
}
```

- `tokenOut`: The contract address of the outbound token
- `fee`: The fee tier of the pool, used to determine the correct pool contract in which to execute the swap
- `recipient`: The destination address of the outbound token
- `deadline`: The unix time after which a transaction will be reverted, to protect against long delays and the increased chance of large price swings therein
- `amountOut`: The desired amount of `token1` received after the swap
- `sqrtPriceLimitX96`: The [Q64.96](/commons/q6496.md) sqrt price limit

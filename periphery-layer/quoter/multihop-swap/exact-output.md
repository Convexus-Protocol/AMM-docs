# 📖 Introduction

An exact output swap will swap a variable amount of the input token for a fixed amount of the outbound token. This is the less common technique for multihop swaps. The code for swapping is largely the same except for one notable difference, the `Path` is encoded backwards, as an exact output swap is executed in reverse order to pass down the necessary variables for the chain of transactions.

# 📜 Methods

## `quoteExactOutput`

- 📚 Description: 
  - Returns the amount in required to receive the given exact output amount for a swap of a multiple pools
- 🔒 Access: 
  - Everyone
  
> 📝 Note
> 
> This method also supports swapping native ICX, use `ICX::ADDRESS` as tokenIn or tokenOut addresses

### 🖊️ Signature

```java
@External(readonly = true)
public QuoteMultiResult quoteExactOutput (
    QuoteExactOutputParams params
)
```

- `params`: The parameters necessary for the swap, encoded as [`QuoteExactOutputParams`](#quoteexactoutputparams)

### 🧪 Example call

```java
{
  "to": Quoter,
  "method": "quoteExactOutput",
  "params": {
    "params": {
      "path": hex([token1, 3000, token2, 3000, token0]) // Path must be provided in reverse order
      "amountOut": "0x1bc16d674ec80000", // 2 * 10**18
    }
  }
}
```

# ⚙️ Structures

## `QuoteExactOutputParams`

```java
class QuoteExactOutputParams {
  byte[] path;
  BigInteger amountOut;
}
```

- `path`: The path of the swap, i.e. each token pair and the pool fee. Path must be provided in reverse order. Please see the [Swap path documentation](/commons/swap-path.md#how-to-encode-a-swap-path) for more information about how to encode a path.
- `amountOut`: The amount of the last token to receive

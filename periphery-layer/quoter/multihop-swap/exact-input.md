# 📖 Introduction

Exact input multi hop quoter will quote a swap from a fixed amount on a given input token for the maximum amount possible for a given output, and can include an arbitrary number of intermediary swaps.

# 📜 Write Methods

## `quoteExactInput`

- 📚 Description: 
  - Returns the amount out received for a given exact input swap without executing the swap
- 🔒 Access: 
  - Everyone

> 📝 Note
> 
> This method also supports swapping native ICX, use `ICX::ADDRESS` as tokenIn or tokenOut addresses

### 🖊️ Signature

```java
@External(readonly = true)
public QuoteMultiResult quoteExactInput (
    QuoteExactInputParams params
)
```

- `params` The parameters necessary for the exact input swap, encoded as [`QuoteExactInputParams`](#quoteexactinputparams)

### 🧪 Example call

```java
{
  "to": Quoter,
  "method": "quoteExactInput",
  "params": {
    "params": {
      "path": hex([token0, 3000, token2, 3000, token1]) // The path of the swap, i.e. each token pair and the pool fee
      "amountIn": "0x1bc16d674ec80000", // 2 * 10**18
    }
  }
}
```

# ⚙️ Structures

## `QuoteExactInputParams`

```java
class QuoteExactInputParams {
  byte[] path;
  BigInteger amountIn;
}
```

- `path`: A sequence of [`tokenIn`, `fee`, `tokenOut`], which are the variables needed to compute each pool contract address in our sequence of swaps. The multihop swap router code will automatically find the correct pool with these variables, and execute the swap needed within each pool in our sequence. Please see the [Swap path documentation](/commons/swap-path.md#how-to-encode-a-swap-path) for more information about how to encode a path.
- `amountIn`: The amount of the first token to swap

# 📖 Introduction

Exact Output swaps a minimum possible amount of the input token for a fixed amount of the outbound token. This is the less common swap style - but useful in a variety of circumstances.

# 📜 Write Methods

## `quoteExactOutputSingle`

- 📚 Description: 
  - Returns the amount in required to receive the given exact output amount but for a swap of a single pool
- 🔒 Access: 
  - Everyone

> 📝 Note
> 
> This method also supports swapping native ICX, use `ICX::ADDRESS` as tokenIn or tokenOut addresses

### 🖊️ Signature

```java
@External(readonly = true)
public QuoteResult quoteExactOutputSingle (QuoteExactOutputSingleParams params)
```

- `params`: The params for the quote, encoded as [`QuoteExactOutputSingleParams`](#quoteexactoutputsingleparams)

### 🧪 Example call

```java
{
  "to": Quoter,
  "method": "quoteExactOutputSingle",
  "params": {
    "params": {
      "tokenIn": tokenIn,
      "tokenOut": tokenOut,
      "amount": "0x1bc16d674ec80000", // 2 * 10**18
      "fee": "0xbb8", // 3000, 0.3%
      "sqrtPriceLimitX96": "0x0" // We set this to zero - which makes this parameter inactive. In production, this value can be used to set the limit for the price the swap will push the pool to, which can help protect against price impact or for setting up logic in a variety of price-relevant mechanisms
    }
  }
}
```

# ⚙️ Structures

## `QuoteExactOutputSingleParams`

```java
class quoteExactOutputSingleParams {
  Address tokenIn;
  Address tokenOut;
  BigInteger amount;
  int fee;
  BigInteger sqrtPriceLimitX96;
}
```

- `tokenIn`: The contract address of the outbound token
- `tokenOut`: The contract address of the outbound token
- `amount`: The destination address of the outbound token
- `fee`: The desired amount of `token1` received after the swap
- `sqrtPriceLimitX96`: The [Q64.96](/commons/q6496.md) sqrt price limit

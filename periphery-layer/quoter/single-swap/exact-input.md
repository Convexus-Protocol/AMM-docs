# 📖 Introduction

Exact Input swaps a fixed amount of the input token for a minimum amount of the outbound token. This is the most common swap style.

# 📜 Write Methods

## `quoteExactInputSingle`

- 📚 Description: 
  - Returns the amount out received for a given exact input but for a swap of a single pool
- 🔒 Access: 
  - Everyone

> 📝 Note
> 
> This method also supports swapping native ICX, use `ICX::ADDRESS` as tokenIn or tokenOut addresses

### 🖊️ Signature

```java
@External(readonly = true)
public QuoteResult quoteExactInputSingle (QuoteExactInputSingleParams params)
```

- `params`: The params for the quote, encoded as [`QuoteExactInputSingleParams`](#quoteexactinputsingleparams)

### 🧪 Example call

```java
{
  "to": Quoter,
  "method": "quoteExactInputSingle",
  "params": {
    "params": {
      "tokenIn": tokenIn,
      "tokenOut": tokenOut,
      "amountIn": "0xde0b6b3a7640000", // 10**18
      "fee": "0xbb8", // 3000, 0.3%
      "sqrtPriceLimitX96": "0x0" // We set this to zero - which makes this parameter inactive. In production, this value can be used to set the limit for the price the swap will push the pool to, which can help protect against price impact or for setting up logic in a variety of price-relevant mechanisms
    }
  }
}
```

# ⚙️ Structures

## `QuoteExactInputSingleParams`

```java
class quoteExactInputSingleParams {
  Address tokenIn;
  Address tokenOut;
  BigInteger amountIn;
  int fee;
  BigInteger sqrtPriceLimitX96;
}
```

- `tokenIn`: The token being swapped in
- `tokenOut`: The token being swapped out
- `amountIn`: The desired input amount
- `fee`: The fee of the token pool to consider for the pair
- `sqrtPriceLimitX96`: The price limit of the pool that cannot be exceeded by the swap
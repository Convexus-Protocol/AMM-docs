# 📖 Introduction

Exact Input swaps a fixed amount of the input token for a minimum amount of the outbound token. This is the most common swap style.

# 📜 Write Methods

## `exactInputSingle`

- 📚 Description: 
  - Swaps `amountIn` of one token for as much as possible of another token
- 🔒 Access: 
  - Everyone

> 📝 Note
> 
> This method also supports swapping native ICX, see [`exactInputSingleIcx`](#exactinputsingleicx)

### 🖊️ Signature

```java
// @External - this method is external through tokenFallback
private void exactInputSingle (
    Address caller, 
    Address tokenIn, 
    BigInteger amountIn, 
    ExactInputSingleParams params
)
```

- `caller`: The method caller. This field is handled by tokenFallback
- `tokenIn`: The tokenIn address. This field is handled by tokenFallback
- `amountIn`: The token amount sent. This field is handled by tokenFallback
- `params`: The parameters necessary for the swap, encoded as [`ExactInputSingleParams`](#exactinputsingleparams)

### 🧪 Example call

```java
{
  "to": token0, // equivalent to tokenIn
  "method": "transfer",
  "params": {
    "_to": SwapRouter,
    "_value": "0xde0b6b3a7640000", // 10**18 - equivalent to amountIn
    "_data": hex({
      "method": "exactInputSingle",
      "params": {
        "tokenOut": token1,
        "fee": "0xbb8", // 3000, 0.3%
        "recipient": recipient,
        "deadline": "0x61e92f6b", // in seconds
        "amountOutMinimum": "0x0", // we are setting to zero, but this is a significant risk in production. For a real deployment, this value should be calculated using our SDK or an onchain price oracle - this helps protect against getting an unusually bad price for a trade due to a front-running sandwich or another type of price manipulation
        "sqrtPriceLimitX96": "0x0" // We set this to zero - which makes this parameter inactive. In production, this value can be used to set the limit for the price the swap will push the pool to, which can help protect against price impact or for setting up logic in a variety of price-relevant mechanisms
      }
    })
  },
}
```

## `exactInputSingleIcx`

- 📚 Description: 
  - Swaps `amountIn` of native ICX for as much as possible of another token
- 🔒 Access: 
  - Everyone

### 🖊️ Signature

```java
@External
@Payable
public void exactInputSingleIcx (ExactInputSingleParams params)
```

- `params`: The parameters necessary for the swap, encoded as [`ExactInputSingleParams`](#exactinputsingleparams)

### 🧪 Example call

```java
{
  "to": SwapRouter, // equivalent to tokenIn
  "method": "exactInputSingleIcx",
  "value": "0xde0b6b3a7640000", // 10**18
  "params": {
    "params": {
      "tokenOut": token1,
      "fee": "0xbb8", // 3000, 0.3%
      "recipient": recipient,
      "deadline": "0x61e92f6b", // in seconds
      "amountOutMinimum": "0x0", // we are setting to zero, but this is a significant risk in production. For a real deployment, this value should be calculated using our SDK or an onchain price oracle - this helps protect against getting an unusually bad price for a trade due to a front-running sandwich or another type of price manipulation
      "sqrtPriceLimitX96": "0x0" // We set this to zero - which makes this parameter inactive. In production, this value can be used to set the limit for the price the swap will push the pool to, which can help protect against price impact or for setting up logic in a variety of price-relevant mechanisms
    }
  },
}
```

# ⚙️ Structures

## `ExactInputSingleParams`

```java
class ExactInputSingleParams {
  Address tokenOut;
  int fee;
  Address recipient;
  BigInteger deadline;
  BigInteger amountOutMinimum;
  BigInteger sqrtPriceLimitX96;
}
```

- `tokenOut`: The contract address of the outbound token
- `fee`: The fee tier of the pool, used to determine the correct pool contract in which to execute the swap
- `recipient`: The destination address of the outbound token
- `deadline`: The unix time after which a swap will fail, to protect against long-pending transactions and wild swings in prices
- `amountOutMinimum`: The minimum amount of token in output
- `sqrtPriceLimitX96`: The [Q64.96](/commons/q6496.md) sqrt price limit

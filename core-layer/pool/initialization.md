# 📖 Introduction

These methods are useful for initializing a Convexus Pool. In most use cases, you shouldn't need to call these methods, as the `Convexus Pool Factory` will automatically call them on a new pool creation.

# 📜 Write Methods

## `initialize`

- 📚 Description: 
  - Sets the initial price for the pool. May only be called only once.
  - Price is represented as a [Q64.96 value](/commons/q6496.md).
- 🔒 Access: 
  - Everyone
- 🔎 Event Logs emitted:
  -  [`Initialized`](#initialized)

### 🖊️ Signature

```java
@External
public void initialize (BigInteger sqrtPriceX96)
```

- `sqrtPriceX96`: the initial sqrt price of the pool as a [Q64.96](/commons/q6496.md)

### 🧪 Example call

```java
{
  "to": ConvexusPool,
  "method": "initialize",
  "params": {
    "sqrtPriceX96": "0x50f44d8921243b6cdba25b3c" // encodePriceSqrt(1, 10)
  },
}
```

# 🔎 Event Logs

## `Initialized`

- 📚 Description: 
  - Emitted exactly once by a pool when [`initialize`](#initialize) is first called on the pool.
  - `Mint`/`Burn`/`Swap` eventlogs cannot be emitted by the pool before `Initialized`

### 🖊️ Signature

```java
@EventLog
protected void Initialized (
  BigInteger sqrtPriceX96, 
  int tick
)
```

- `sqrtPriceX96`: The initial sqrt price of the pool, as a [Q64.96](/commons/q6496.md)
- `tick`: The initial tick of the pool, i.e. log base 1.0001 of the starting price of the pool

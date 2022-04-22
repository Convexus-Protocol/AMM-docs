## `ConvexusPool::initialize`

### 📜 Method Call

- Sets the initial price for the pool. May only be called once.
- Access: Everyone
- Price is represented as a sqrt(amountToken1/amountToken0) [Q64.96 value](/Convexus-Commons/Librairies/docs/README.md#how-to-encode-a-q6496-price)

```java
@External
public void initialize (BigInteger sqrtPriceX96)
```

- `sqrtPriceX96`: the initial sqrt price of the pool as a [Q64.96](/Convexus-Commons/Librairies/docs/README.md#how-to-encode-a-q6496-price)
- Emits a [`Initialize`](#convexuspoolinitializeeventlog) event log

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

## `ConvexusPool::Initialize (EventLog)`

### 🔎 Event Log

```java
@EventLog
protected void Initialize (BigInteger sqrtPriceX96, int tick)
```

-  Emitted exactly once by a pool when `initialize` is first called on the pool. 
- `Mint`/`Burn`/`Swap` cannot be emitted by the pool before `Initialize`
- `sqrtPriceX96`: The initial sqrt price of the pool, as a [Q64.96](/Convexus-Commons/Librairies/docs/README.md#how-to-encode-a-q6496-price)
- `tick`: The initial tick of the pool, i.e. log base 1.0001 of the starting price of the pool
